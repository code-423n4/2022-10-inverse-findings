### G-01 SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

*Number of Instances Identified: 8*

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
249: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```


https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
448: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
512: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

### G-02 DONT COMPARE BOOLEAN EXPRESSIONS TO BOOLEAN LITERALS

*Number of Instances Identified: 1*

`if (<x> == true)` => `if (<x>)`, `if (<x> == false)` => `if (!<x>)`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```


### G-03 REQUIRE or REVERT STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

*Number of Instances Identified: 4*


https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
326: require(markets[msg.sender], "Only markets can call onForceReplenish");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
214: require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```


### G-04 DUPLICATED REQUIRE() or REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

*Number of Instances Identified: 6*

Saves deployment costs

The following require and revert checks have been used more than once within the same contract, hence they should be refactored to a modifier or a function

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
195: require(balanceOf(from) >= amount, "Insufficient balance");
314: require(markets[msg.sender], "Only markets can call onRepay");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
48: require(msg.sender == gov, "ONLY GOV");
76: require(msg.sender == chair, "ONLY CHAIR");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
104: revert("Price not found");
117: require(price > 0, "Invalid feed price");
```

### G-05 USAGE OF UINTS or INTS SMALLER THAN 32 BYTES - 256 BITS INCURS OVERHEAD

*Number of Instances Identified: 6*


> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
85: uint8 feedDecimals = feeds[token].feed.decimals();
86: uint8 tokenDecimals = feeds[token].tokenDecimals;
87: uint8 decimals = 36 - feedDecimals - tokenDecimals;
119: uint8 feedDecimals = feeds[token].feed.decimals();
120: uint8 tokenDecimals = feeds[token].tokenDecimals;
121: uint8 decimals = 36 - feedDecimals - tokenDecimals;
```


### G-06 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Number of Instances Identified: 26*

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
172: balances[msg.sender] -= amount;
174: balances[to] += amount;
196: balances[from] -= amount;
198: balances[to] += amount;
288: dueTokensAccrued[user] += accrued;
289: totalDueTokensAccrued += accrued;
304: debts[user] += additionalDebt;
316: debts[user] -= repaidDebt;
332: debts[user] += replenishmentCost;
360: _totalSupply += amount;
362: balances[to] += amount;
374: balances[from] -= amount;
376:  _totalSupply -= amount;
```


https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
91: supplies[market] += amount;
92: globalSupply += amount;
110: supplies[market] -= amount;
111: globalSupply -= amount;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
395: debts[borrower] += amount;
397: totalDebt += amount;
534: debts[user] -= amount;
535: totalDebt -= amount;
565: debts[user] += replenishmentCost;
568: totalDebt += replenishmentCost;
598: liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
599: debts[user] -= repaidDebt;
600: totalDebt -= repaidDebt;
```

### G-07 USING BOOLS FOR STORAGE INCURS OVERHEAD

*Number of Instances Identified: 6*


```
    // Booleans are more expensive than uint256 or any type that takes up a full    // word because each write operation emits an extra SLOAD to first read the    // slot's contents, replace the bits taken up by the boolean, and then write    // back. This is the compiler's defense against contract upgrades and    // pointer aliasing, and it cannot be disabled.
```

[https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol

```
11: mapping(address => bool) public contractAllowlist;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
24: mapping (address => bool) public minters;
25: mapping (address => bool) public markets;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
52: bool immutable callOnDepositCallback;
53: bool public borrowPaused;
72: bool _callOnDepositCallback
```

### G-08 USE CUSTOM ERRORS RATHER THAN REVERT() or REQUIRE() STRINGS TO SAVE GAS

*Number of Instances Identified: 66*

Some of them include : 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
45: require(msg.sender == operator, "ONLY OPERATOR");
63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
224: require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
```



### G-09 ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 (SAME FOR --I VS I-- OR I -= 1)

*Number of Instances Identified: 5*

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
239: nonces[owner]++
259: nonces[msg.sender]++;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
438: nonces[from]++
502: nonces[from]++,
521: nonces[msg.sender]++;
```


### G-10 USING `> 0` COSTS MORE GAS THAN `!= 0` WHEN USED ON A `UINT` 

*Number of Instances Identified: 20*

some of them include : 

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
328: require(deficit > 0, "No deficit");
```