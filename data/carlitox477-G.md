# INVEscrow: declaring xINV as private rather than public can sabe gas
Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table.

Change [this line](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L32) for:
```solidity
IXINV private immutable xINV;
```

# INVEscrow#pay: token state variable can be cached
This will save gas, replace [this function](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L59-L64) for:
```solidity
function pay(address recipient, uint amount) public {
    IERC20 _token = token;
    require(msg.sender == market, "ONLY MARKET");
    uint invBalance = _token.balanceOf(address(this));
    if(invBalance < amount) xINV.redeemUnderlying(amount - invBalance); // we do not check return value because next call will fail if this fails anyway
    _token.transfer(recipient, amount);
}
```

# SimpleERC20Escrow#initialize: extra parameter can be omitted
The second parameter is never used, so it can be deleted in order to save gas. Change [this line](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L25) for:
```solidity
function initialize(IERC20 _token) external {
```

# BorrowController#borrowAllowed: extra parameters can be omitted
The second parameter is never used, so it can be deleted in order to save gas. Change [this line](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L46) for:
```solidity
function borrowAllowed(address msgSender) external view returns (bool) {
```

# DolaBorrowingRights: name and symbol can be declared as immutable to save gas
This will save gas during contract creation.

# DolaBorrowingRights#balanceOf: dueTokensAccrued\[user\] and balances\[user\] can be cached
This will save gas in function [balanceOf](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/DBR.sol#L120-L125).

```solidity
function balanceOf(address user) public view returns (uint) {
    uint debt = debts[user];
    uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days; // accrued debt?
    uint256 _dueTokensAccrued = dueTokensAccrued[user];
    uint256 _balance = balances[user];
    if(_dueTokensAccrued+ accrued > _balance) return 0;
    return _balance - _dueTokensAccrued - accrued;
}
```

# Fed#expansion state variable globalSupply can be cached to save gas
To avoid 3 state variable access to globalSupply replace [this code](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L92-L93) for:

```
_globalSupply = globalSupply + amount;
require(_globalSupply <= supplyCeiling);
globalSupply = _globalSupply;
```

# Fed#contraction can avoid supplies\[market\] storage access
Replace [this line of code](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L110) for ```supplies[market] = supply - amount;```

# Oracle#claimOperator can avoid access to pendingOperator and operator state variables
Change [claimOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L66-L71) to 

```solidity
function claimOperator() public {
    require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
    operator = msg.sender;
    pendingOperator = address(0);
    emit ChangeOperator(msg.sender);
}
```

# Oracle#viewPrice can avoid access to fixedPrices\[token\] and feeds\[token\]
Change [viewPrice](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L78-L105) to 

```solidity
function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
    uint256 _fixedPrice= fixedPrices[token] 
    if(_fixedPrice > 0) return _fixedPrice;
    IChainlinkFeed _feed = feeds[token].feed

    if(_feed.feed != IChainlinkFeed(address(0))) {
        // get price from feed
        uint price = _feed.feed.latestAnswer();
        require(price > 0, "Invalid feed price");
        // normalize price
        uint8 feedDecimals = _feed.feed.decimals();
        uint8 tokenDecimals = _feed.tokenDecimals;
        uint8 decimals = 36 - feedDecimals - tokenDecimals;
        uint normalizedPrice = price * (10 ** decimals);
        uint day = block.timestamp / 1 days;
        // get today's low
        uint todaysLow = dailyLows[token][day];
        // get yesterday's low
        uint yesterdaysLow = dailyLows[token][day - 1];
        // calculate new borrowing power based on collateral factor
        uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
        uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
        if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
            uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
            return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
        }
        return normalizedPrice;

    }
    revert("Price not found");
}
```

# Oracle#viewPrice can avoid access to fixedPrices\[token\] and feeds\[token\]
Change [getPrice](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L112-L144) to 

```solidity
function getPrice(address token, uint collateralFactorBps) external returns (uint) {
    uint256 _fixedPrice= fixedPrices[token] 
    if(_fixedPrice > 0) return _fixedPrice;
    IChainlinkFeed _feed = feeds[token]
    if(_feed.feed != IChainlinkFeed(address(0))) {
        // get price from feed
        uint price = _feed.feed.latestAnswer();
        require(price > 0, "Invalid feed price");
        // normalize price
        uint8 feedDecimals = _feed.feed.decimals();
        uint8 tokenDecimals = _feed.tokenDecimals;
        uint8 decimals = 36 - feedDecimals - tokenDecimals;
        uint normalizedPrice = price * (10 ** decimals);
        // potentially store price as today's low
        uint day = block.timestamp / 1 days;
        uint todaysLow = dailyLows[token][day];
        if(todaysLow == 0 || normalizedPrice < todaysLow) {
            dailyLows[token][day] = normalizedPrice;
            todaysLow = normalizedPrice;
            emit RecordDailyLow(token, normalizedPrice);
        }
        // get yesterday's low
        uint yesterdaysLow = dailyLows[token][day - 1];
        // calculate new borrowing power based on collateral factor
        uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
        uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
        if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
            uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
            return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
        }
        return normalizedPrice;

    }
    revert("Price not found");
}
```