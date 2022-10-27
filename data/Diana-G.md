## G-01 x += y COSTS MORE GAS THAN x = x + y FOR STATE VARIABLES (SAME APPLIES FOR x -= y , x = x - y)

_There are 26 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

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
376: _totalSupply -= amount;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
File: src/Fed.sol

91: supplies[market] += amount;
92: globalSupply += amount;
110: supplies[market] -= amount;
111: globalSupply -= amount;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

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

----------

## G-02 DUPLICATED REQUIRE() OR REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

_There are 12 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

195: require(balanceOf(from) >= amount, "Insufficient balance");
373: require(balanceOf(from) >= amount, "Insufficient balance");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
File: src/Fed.sol

49: require(msg.sender == gov, "ONLY GOV");
58: require(msg.sender == gov, "ONLY GOV");
67: require(msg.sender == gov, "ONLY GOV");
76: require(msg.sender == chair, "ONLY CHAIR");
87: require(msg.sender == chair, "ONLY CHAIR");
104: require(msg.sender == chair, "ONLY CHAIR");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
File: src/Oracle.sol

83: require(price > 0, "Invalid feed price");
117: require(price > 0, "Invalid feed price");
```
-----------

## G-03 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

_There are 6 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
File: src/Oracle.sol

85: uint8 feedDecimals = feeds[token].feed.decimals();
86: uint8 tokenDecimals = feeds[token].tokenDecimals;
87: uint8 decimals = 36 - feedDecimals - tokenDecimals;
119: uint8 feedDecimals = feeds[token].feed.decimals();
120: uint8 tokenDecimals = feeds[token].tokenDecimals;
121: uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

-----------

## G-04 SPLITTING REQURE() STATEMENTS THAT USE && SAVES GAS

_There are 8 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249

```
File: src/DBR.sol

249: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
448: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
512: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

-------------

## G-05 REQUIRE() OR REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

_There are 4 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
326: require(markets[msg.sender], "Only markets can call onForceReplenish");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

76: require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
214: require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

-----------

## G-06 DON'T COMPARE BOOLEAN EXPRESSIONS TO BOOLEAN LITERALS

Comparing to a constant (`true` or `false`) is a bit more expensive than directly checking the returned boolean value.

`if (<x> == true)` => `if (<x>)`, `if (<x> == false)` => `if (!<x>)`

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L350

```
File: src/DBR.sol

350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

-----------

## G-07 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353

```
File: src/Market.sol

353: function getWithdrawalLimitInternal(address user) internal returns (uint) {
```

------------

## G-08 USING BOOLS FOR STORAGE INCURS OVERHEAD

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

_There are 6 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L11

```
File: src/BorrowController.sol

11: mapping(address => bool) public contractAllowlist;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

24: mapping (address => bool) public minters;
25: mapping (address => bool) public markets;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

52: bool immutable callOnDepositCallback;
53: bool public borrowPaused;
72: bool _callOnDepositCallback
```

------------

## G-09 ++i COSTS LESS GAS THAN i++ ESPECIALLY WHEN IT'S USED IN FOR LOOPS (SAME APPLIES FOR --i, i-- TOO)

Prefix increments are cheaper than postfix increments.  

_There are 5 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

239: nonces[owner]++,
259: nonces[msg.sender]++;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

438: nonces[from]++,
502: nonces[from]++,
521: nonces[msg.sender]++;
```

------

## G-10 USE CUSTOM ERRORS RATHER THAN REVERT() OR REQUIRE() STRINGS TO SAVE DEPLOYMENT GAS

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version

Source: [https://blog.soliditylang.org/2021/04/21/custom-errors/](https://blog.soliditylang.org/2021/04/21/custom-errors/):

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the `error` statement, which can be used inside and outside of contracts (including interfaces and libraries).

_There are 66 instances of this issue_

Some instances include:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
File: src/Oracle.sol

36: require(msg.sender == operator, "ONLY OPERATOR");
67: require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
83: require(price > 0, "Invalid feed price");
104: revert("Price not found");
117: require(price > 0, "Invalid feed price");
143: revert("Price not found");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol

```
File: src/escrows/GovTokenEscrow.sol

31: require(market == address(0), "ALREADY INITIALIZED");
44: require(msg.sender == market, "ONLY MARKET");
```

-----------

## G-11 USING GREATER THAN 0 COSTS MORE THAN !=0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas). This change saves 6 gas per instance

_There are 11 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
328: require(deficit > 0, "No deficit");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
561: require(deficit > 0, "No DBR deficit");
592: require(repaidDebt > 0, "Must repay positive debt");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
File: src/Oracle.sol

83: require(price > 0, "Invalid feed price");
117: require(price > 0, "Invalid feed price");
```

---------------
