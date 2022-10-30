
# GAS
## duplicated `require()` check should be refactored
### Summary
duplicated `require()` / `revert()` checks should be
refactored to a modifier or function to save gas
### Details
Event appears twice and can be reduced

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L74
        require(_collateralFactorBps < 10000, "Invalid collateral factor");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L150
        require(_collateralFactorBps < 10000, "Invalid collateral factor");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L75
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L184
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L423
        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L487
        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L448
            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L512
            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L171
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L195
        require(balanceOf(from) >= amount, "Insufficient balance");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L373
        require(balanceOf(from) >= amount, "Insufficient balance");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L83
            require(price > 0, "Invalid feed price");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L117
            require(price > 0, "Invalid feed price");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L49
        require(msg.sender == gov, "ONLY GOV");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L58
        require(msg.sender == gov, "ONLY GOV");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L67
        require(msg.sender == gov, "ONLY GOV");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L76
        require(msg.sender == chair, "ONLY CHAIR");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L87
        require(msg.sender == chair, "ONLY CHAIR");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L104
        require(msg.sender == chair, "ONLY CHAIR");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L88
        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L105
        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L107
        require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L31
        require(market == address(0), "ALREADY INITIALIZED");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/SimpleERC20Escrow.sol#L26
        require(market == address(0), "ALREADY INITIALIZED");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L45
        require(market == address(0), "ALREADY INITIALIZED");

- - - - -

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L104
        revert("Price not found");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L143
        revert("Price not found");

### Mitigation
refactor this checks to different functions to save gas


## splitting `require()` statements that use `&&` saves gas
### Summary
Instead of using the && operator in a single require statement to check multiple conditions, consider using multiple require statements with 1 condition per require statement (saving 3 gas per & ):
### Github Permalinks


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L75
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L162
        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L173
        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L184
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L195
        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L448
            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L512
            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L249
            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");


### Mitigation
Split require statements



## Pack state variables tightly
### Summary
State variables are expected to be ordered by data type, this helps readability and also gas optimization by tightly packing the variables.
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/GovTokenEscrow.sol#L20-L22
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/INVEscrow.sol#L29-L31
### Mitigation
Order in a proper way the state variables to improve readability and to reduce gas usage.




## `>=` cheaper than `>`
### Summary
Strict inequalities ( `>` ) are more expensive than non-strict ones ( `>=` ). This is due to some supplementary checks (`ISZERO`, 3 gas)

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L605
        if(liquidationFeeBps > 0) {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L79
        if(fixedPrices[token] > 0) return fixedPrices[token];

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L96
            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L97
            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L113
        if(fixedPrices[token] > 0) return fixedPrices[token];

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L135
            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L136
            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L133
        if(profit > 0) {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L81
        if(invBalance > 0) {


### Mitigation
Consider using `>= 1` instead of `> 0` to avoid some opcodes



## `<X> += <Y>` costs more gas than `<X> = <X> + <Y>` for state variables
### Summary
`x+=y` costs more gas than x=x+y for state variables

### Github Permalinks


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L395
        debts[borrower] += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L397
        totalDebt += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L565
        debts[user] += replenishmentCost;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L568
        totalDebt += replenishmentCost;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L174
            balances[to] += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L198
            balances[to] += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L288
        dueTokensAccrued[user] += accrued;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L289
        totalDueTokensAccrued += accrued;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L304
        debts[user] += additionalDebt;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L332
        debts[user] += replenishmentCost;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L360
        _totalSupply += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L362
            balances[to] += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L91
        supplies[market] += amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L92
        globalSupply += amount;

- Same with `-=`

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L534
        debts[user] -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L535
        totalDebt -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L599
        debts[user] -= repaidDebt;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L600
        totalDebt -= repaidDebt;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L172
        balances[msg.sender] -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L196
        balances[from] -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L316
        debts[user] -= repaidDebt;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L374
        balances[from] -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L376
            _totalSupply -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L110
        supplies[market] -= amount;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L111
        globalSupply -= amount;



### Mitigation
Don't use `+=` for state variables as it cost more gas.



## usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
### Summary
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

### Details
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html 
Use a larger size than downcast where needed

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L13
    uint8 public constant decimals = 18;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L13
    uint8 public constant decimals = 18;

### Mitigation
Consider using some data type that uses 32 bytes, for example uint256


## `abi.encode()` is less gas efficient than `abi.encodePacked()`
### Summary
In general, `abi.encodePacked` is more gas-efficient.

### Details
Changing the abi.encode function to `abi.encodePacked` can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.
### Github Permalinks


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L104
                abi.encode(

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L431
                            abi.encode(

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L495
                            abi.encode(

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L232
                            abi.encode(

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L269
                abi.encode(


### Mitigation
Consider changing abi.encode to `abi.encodePacked`


## Internal functions only called once can be inlined to save gas
### Summary
Not inlining costs `20` to `40` gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.
### Github Permalinks

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L353
    function getWithdrawalLimitInternal(address user) internal returns (uint) {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L372
    function _burn(address from, uint256 amount) internal virtual {



### Mitigation
Consider changing internal function only called once to inline code for gas savings


## Unnecesary storage read
### Summary
A value which value is already known can be used directly rather than reading it from the storage
### Example
```
    function claimOperator() public {
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
        operator = pendingOperator;
        pendingOperator = address(0);
        emit ChangeOperator(operator);
    }
```

pending operator can be cached to avoid unnecesary storage read

Recommendation
Change to:

```
    function claimOperator() public {
        address localPendingOperator = pendingOperator;
        require(msg.sender == localPendingOperator, "ONLY PENDING OPERATOR");
        operator = localPendingOperator;
        pendingOperator = address(0);
        emit ChangeOperator(localPendingOperator);
    }
```

### Github Permalinks

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L74
        emit ChangeOperator(operator);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L70
        emit ChangeOperator(operator);


### Mitigation
Set directly the value to avoid unnecessary storage read to save some gas






## Store using Struct over multiple mappings
### Summary
All these variables could be combine in a Struct in order to reduce the gas cost. 
### Details
As noticed in: 
https://gist.github.com/alexon1234/b101e3ac51bea3cbd9cf06f80eaa5bc2
When multiple mappings that access the same addresses, uints, etc, all of them can be mixed into an struct and then that data accessed like:
mapping(datatype => newStructCreated) newStructMap;
Also, you have this post where it explains the benefits of using Structs over mappings 
https://medium.com/@novablitz/storing-structs-is-costing-you-gas-774da988895e

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L57-L59
- - - - -
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L25-L27
- - - - -
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L23-L28
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L19-L20
### Mitigation
Consider mixing different mappings into an struct when able in order to save gas.

