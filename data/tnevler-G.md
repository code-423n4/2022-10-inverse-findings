# Report
## Gas Optimizations ## 

### [G-01]: Place subtractions where the operands can't underflow in unchecked {} block
**Context:**

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124
4. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111
5. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L124
6. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L137
7. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172
8. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196
9. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374
10. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L362
11. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L379
12. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534

**Description:**

Some gas can be saved by using an unchecked {} block if an underflow isn't possible because of a previous require() or if-statement.


### [G-02]: If internal function called only once it can be inlined to save gas
**Context:**

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L372

### [G-03]: Catch a value inside a mapping/array in local memory
**Context:**

``` 
        if(escrows[user] != IEscrow(address(0))) return escrows[user];
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246


```
        if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
        return balances[user] - dueTokensAccrued[user] - accrued;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124


```
        if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
        return dueTokensAccrued[user] + accrued - balances[user];
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137


```
        if(lastUpdated[user] == block.timestamp) return;
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286-L287


```
        if(feeds[token].feed != IChainlinkFeed(address(0))) {
            // get price from feed
            uint price = feeds[token].feed.latestAnswer();
            require(price > 0, "Invalid feed price");
            // normalize price
            uint8 feedDecimals = feeds[token].feed.decimals();
            uint8 tokenDecimals = feeds[token].tokenDecimals;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L80-L86


```
        if(feeds[token].feed != IChainlinkFeed(address(0))) {
            // get price from feed
            uint price = feeds[token].feed.latestAnswer();
            require(price > 0, "Invalid feed price");
            // normalize price
            uint8 feedDecimals = feeds[token].feed.decimals();
            uint8 tokenDecimals = feeds[token].tokenDecimals;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L114-L120


**Recommendation:**

Example how to fix. Change:
```
    function balanceOf(address user) public view returns (uint) {
        uint debt = debts[user];
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
        if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
        return balances[user] - dueTokensAccrued[user] - accrued;
    }
```

to:
```
    function balanceOf(address user) public view returns (uint) {
        uint debt = debts[user];
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
        uint256 _dueTokensAccrue = dueTokensAccrued[user];
        uint256 _balance = balances[user];
        if(_dueTokensAccrue + accrued > _balance) return 0;
        return _balance - _dueTokensAccrue - accrued;
    }
```

### [G-04]: Catch state variable in local memory and not re-read it from storage
**Context:**

```
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L359-L360

```
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L376-L377

```
        if(liquidationFeeBps > 0) {
            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605-L606

```
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
        operator = pendingOperator;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67-L68

**Description:** 

Every reading from storage costs about 100 gas while every reading from memory costs only 3 gas. If state variable referred more than once within a function then it is cheaper to cached it in local memory (100 gas) and then read it from memory wnen it is neaded (3 gas) rather than read state variable from storage everytime (100 gas).

**Recommendation:**

Example how to fix. Change:
```
    function getWithdrawalLimitInternal(address user) internal returns (uint) {
        ...
        
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;        
        if(collateralBalance <= minimumCollateral) return 0;
        return collateralBalance - minimumCollateral;
    }
```

to:
```
    function getWithdrawalLimitInternal(address user) internal returns (uint) {
        ...
        
        uint _collateralFactorBps = collateralFactorBps;
        
        if(_collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), _collateralFactorBps) * 10000 / _collateralFactorBps;        
        
        ...
    }
```

### [G-05]: No need to catch state variable in local memory if it only use once
**Context:**

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L532
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L121
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L134
4. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L285
5. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L106

**Recommendation:**

Read the state variable from storage.
