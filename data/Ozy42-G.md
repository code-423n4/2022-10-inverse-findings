# Gas Optimizations

## DBR.sol

### #balanceOf()
Public function used on function like transfer, transferFrom, _burn (state change functions)
- Remove this line, which use memory slot for nothing because debts[user] is only called one time:
```js
uint256 debt = debts[user];
//...
uint256 accrued = ((block.timestamp - lastUpdated[user]) *
    debt) / 365 days;
```
after:
```js
uint256 accrued = ((block.timestamp - lastUpdated[user]) *
    debts[user]) / 365 days;
```
- Add memory variable for mapping which are called more than one time:
```js
if (dueTokensAccrued[user] + accrued > balances[user]) return 0;
return balances[user] - dueTokensAccrued[user] - accrued;
```
after:
```js
uint256 duTokensAccruedUser = dueTokensAccrued[user];
uint256 balanceUser = balances[user];

if (duTokensAccruedUser + accrued > balanceUser) return 0;
return balanceUser - duTokensAccruedUser - accrued;
```
### #deficitOf()
Public function used on function like onBorrow, onForceReplenish... (state change functions)
- Exactly the same gas opt. as balanceOf()

### #totalSupply()
Public view function, not used in other functions, so not really important.
- replace:
```js
if (totalDueTokensAccrued > _totalSupply) return 0;
return _totalSupply - totalDueTokensAccrued;
```
after:
```js
uint256 totalSupply_ = _totalSupply;
uint256 totalDueTokensAccrued_ = totalDueTokensAccrued;
if (totalDueTokensAccrued_ > totalSupply_) return 0;
return totalSupply_ - totalDueTokensAccrued_;
```

### #accrueDueTokens()
Public function used on function like onBorrow, onRepay, onForceReplenish (state change functions)
- Add memory variable for mapping which are called more than one time & remove memory variable debt:
```js
uint256 debt = debts[user];
if (lastUpdated[user] == block.timestamp) return;
uint256 accrued = ((block.timestamp - lastUpdated[user]) * debt) /
    365 days;
```
after:
```js
uint256 lastUpdatedUser = lastUpdated[user];
if (lastUpdatedUser == block.timestamp) return;
uint256 accrued = ((block.timestamp - lastUpdatedUser) * debt) /
    365 days;
```

## Fed.sol

### #contraction()
Public function
- remove memory variable supply which are called only one time:
```js
uint256 supply = supplies[market];
require(amount <= supply, "AMOUNT TOO BIG");
```
after:
```js
require(amount <= supplies[market], "AMOUNT TOO BIG");
```

## Market.sol

### #setLiquidationFactorBps()
- when a require has 2 operations linked by an AND operator, you can use 2 separate requires to save gas:
```js
require(
    _liquidationFactorBps > 0 && _liquidationFactorBps <= 10000,
    "Invalid liquidation factor"
);
```
after:
```js
require(_liquidationFactorBps > 0, "Invalid liquidation factor");
require(_liquidationFactorBps <= 10000, "Invalid liquidation factor");
```
### #setReplenismentIncentiveBps()
- exactly the same gas opt. as setLiquidationFactorBps()

### #setLiquidationIncentiveBps()
- exactly the same gas opt. as setLiquidationFactorBps()

### #predictEscrow()
- This is not really an gas optimization, but just for the style you can remove the memory variable which mstore the address(this),
and use the assembly opcode address() instead.
```js
address deployer = address(this);
assembly {
    //...
    mstore(add(ptr, 0x38), shl(0x60, deployer))
}
```
after:
```js
assembly {
    //...
    mstore(add(ptr, 0x38), shl(0x60, address()))
}
```

### #getCollateralValue()
Public function used on function like getCreditLimit, getLiquidatableDebt (view functions)
- Remove useless memory variable to save gas:
```js
IEscrow escrow = predictEscrow(user);
uint256 collateralBalance = escrow.balance();
return
    (collateralBalance *
        oracle.viewPrice(address(collateral), collateralFactorBps)) /
    1 ether;
```
after:
```js
return
    (IEscrow(predictEscrow(user)).balance() *
        oracle.viewPrice(address(collateral), collateralFactorBps)) /
    1 ether;
```

### #getCollateralValueInternal()
Internal function used on function like getCreditLimitInternal, forceReplenish (state change functions)
- Exactly the same as getCollateralValue():

### #getCreditLimit()
Public function used on function like getLiquidatableDebt (view functions)
- remove useless memory variable to save gas: 
```js
uint256 collateralValue = getCollateralValue(user);
return (collateralValue * collateralFactorBps) / 10000;
```
after:
```js
return (getCollateralValue(user) * collateralFactorBps) / 10000;
```

### #getCreditLimitInternal()
Internal function used on function like borrowInternal, liquidate (state change functions)
- Exactly the same as getCollateralValue():

### #getWithdrawalLimitInternal()
Internal function used on function like withdrawInternal, withdraw (state change functions)
- Remove useless memory variable to save gas:
```js
IEscrow escrow = predictEscrow(user);
uint256 collateralBalance = escrow.balance();
```
after:
```js
uint256 collateralBalance = IEscrow(predictEscrow(user)).balance();
```
- Add memory variable for collateralFactorBps (stored variable) which is called multiple times in this function:
```js
if (collateralFactorBps == 0) return 0;
uint256 minimumCollateral = (((debt * 1 ether) /
    oracle.getPrice(address(collateral), collateralFactorBps)) *
    10000) / collateralFactorBps;
```
after:
```js
uint256 _collateralFactorBps = collateralFactorBps;
if (_collateralFactorBps == 0) return 0;
uint256 minimumCollateral = (((debt * 1 ether) /
    oracle.getPrice(address(collateral), _collateralFactorBps)) *
    10000) / _collateralFactorBps;
```

### #borrowInternal()
Internal function used on function like borrow, borrowOnBehalf (state change functions)
- Remove useless memory variable to save gas:
```js
uint256 credit = getCreditLimitInternal(borrower);
debts[borrower] += amount;
require(
    credit >= debts[borrower],
    "Exceeded credit limit"
);
```
after:
```js
debts[borrower] += amount;
require(
    getCreditLimitInternal(borrower) >= debts[borrower],
    "Exceeded credit limit"
);
```

### #withdrawInternal()
Internal function used on function like withdraw (state change functions)
- Remove useless memory variable to save gas:
```js
uint256 limit = getWithdrawalLimitInternal(from);
require(
    limit >= amount,
    "Insufficient withdrawal limit"
);
IEscrow escrow = getEscrow(from);
escrow.pay(to, amount);
```
after:
```js
require(
    getWithdrawalLimitInternal(from) >= amount,
    "Insufficient withdrawal limit"
);
IEscrow(getEscrow(from)).pay(to, amount);
```

### #repay()
Public function used on function like repayAndWithdraw (state change functions)
- Remove useless memory variable to save gas:
```js
uint256 debt = debts[user];
require(debt >= amount, "Insufficient debt");
```
after:
```js
require(debts[user] >= amount, "Insufficient debt");
```

### #forceReplenish()
Public function (state change functions)
- Remove useless memory variable to save gas:
```js
uint256 collateralValue = getCollateralValueInternal(user);
require(
    collateralValue >= debts[user],
    "Exceeded collateral value"
);
```
after:
```js
require(
    getCollateralValueInternal(user) >= debts[user],
    "Exceeded collateral value"
);
```

### #getLiquidatableDebt()
Public view function
- Remove useless memory variable to save gas:
```js
uint256 credit = getCreditLimit(user);
if (credit >= debt) return 0;
```
after:
```js
if (getCreditLimit(user) >= debt) return 0;
```

## Oracle.sol

### #getPrice()
Public function (state change function)
- Struct FeedData not called correctly to save gas, it's better to call all the struct in a memory variable and then use it:
```js
if(feeds[token].feed != IChainlinkFeed(address(0))) {
    // get price from feed
    uint price = feeds[token].feed.latestAnswer();
    require(price > 0, "Invalid feed price");
    // normalize price
    uint8 feedDecimals = feeds[token].feed.decimals();
    uint8 tokenDecimals = feeds[token].tokenDecimals;
```
after:
```js
FeedData memory feedData = feeds[token];
if (feedData.feed != IChainlinkFeed(address(0))) {
    // get price from feed
    uint256 price = feedData.feed.latestAnswer();
    require(price > 0, "Invalid feed price");
    // normalize price
    uint8 feedDecimals = feedData.feed.decimals();
    uint8 tokenDecimals = feedData.tokenDecimals;
```
- The var normalizedPrice can be used more efficiently, so in combination with the previous gas optimization we have:
```js
if(feeds[token].feed != IChainlinkFeed(address(0))) {
    // get price from feed
    uint price = feeds[token].feed.latestAnswer();
    require(price > 0, "Invalid feed price");
    // normalize price
    uint8 feedDecimals = feeds[token].feed.decimals();
    uint8 tokenDecimals = feeds[token].tokenDecimals;
    uint8 decimals = 36 - feedDecimals - tokenDecimals;
    uint normalizedPrice = price * (10 ** decimals);
```
after:
```js
FeedData memory feedData = feeds[token];
if (feedData.feed != IChainlinkFeed(address(0))) {
    // get price from feed
    uint256 price = feedData.feed.latestAnswer();
    require(price > 0, "Invalid feed price");
    // normalize price
    uint256 normalizedPrice = price *
        (10**(36 - feedData.feed.decimals() - feedData.tokenDecimals));
```
### #viewPrice()
- Exactly the same gas opt as getPrice()

#### That's all !! Thank you for this contest (my first one).

## Ozy42