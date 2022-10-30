# Gas Report
## Optimizations found [5]

### [G-01] Use nested if instead of && [ⓘ](https://github.com/devanshbatham/Solidity-Gas-Optimization-Tips#6--use-nested-if-and-avoid-multiple-check-combinations) 

Breaking apart require and if statements saves gas.

#### Findings [2]:

*/src/Oracle.sol*
Line(s): [97](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L97), [136](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L136)
```solidity
97:	if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
136:	if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
```


### [G-02] Break Apart Require Statments Instead of && [ⓘ](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-11-splitting-require-statements-that-use--saves-gas)

Breaking apart require and if statements saves gas.

#### Findings [7]:

*/src/Market.sol*
Line(s): [75](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L75), [162](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L162), [173](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L173), [184](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L184), [195](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L195), [448](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L448), [512](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L512)
```solidity
75:	require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
162:	require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173:	require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184:	require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195:	require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
448:	require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
512:	require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```


### [G-03] Unless Used for Variable Packing uint8 May Be More Expensive Than Using uint256 [ⓘ](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-15-usage-of-uint8-may-increase-gas-cost)

Same as bool's uint8 data causes uneeded overhead.

#### Findings [11]:

*/src/Oracle.sol*
Line(s): [20](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L20), [85](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L85), [86](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L86), [87](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L87), [119](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L119), [120](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L120), [121](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L121)
```solidity
20:	uint8 tokenDecimals;
85:	uint8 feedDecimals = feeds[token].feed.decimals();
86:	uint8 tokenDecimals = feeds[token].tokenDecimals;
87:	uint8 decimals = 36 - feedDecimals - tokenDecimals;
119:	uint8 feedDecimals = feeds[token].feed.decimals();
120:	uint8 tokenDecimals = feeds[token].tokenDecimals;
121:	uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

*/src/Market.sol*
Line(s): [422](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L422), [486](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L486)
```solidity
422:	function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
486:	function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
```

*/src/DBR.sol*
Line(s): [13](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/DBR.sol#L13), [220](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/DBR.sol#L220)
```solidity
13:	uint8 public constant decimals = 18;
220:	uint8 v,
```

### [G-04] `immutable` variables never initialized in the constructor can be made `constant` to save gas

`dola` is made `immutable` even though it is never set in the constructor and is always the same value. Consider changing `dola` to a `constant` data type to save gas.

#### Findings [1]:

*src/market.sol*
Line(s): [44](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L44)

```solidity
44:	IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
```

**Suggested change:**

```solidity
44:	IERC20 public constant dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
```

### [G-05] Using `unchecked` when arithmetic cannot overflow saves gas

There are mutliple occurances where arithmetic is performed that cannot overflow / underflow based on a previous require. Adding an `unchecked` brace around these occurances will save gas (Solidity will not force an underflow / overflow).

**NOTE:** Findings show the check before each line that allows the second line to be `unchecked`.  Only the line following the check should be unchecked (NOT the check itself).

```solidity
unchecked {
	//Code
}
```

#### Findings [7]:

*src/Market.sol*
Line(s): [261-362](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L361-L362), [378-379](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L378-L379), [533-534](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533-L534)

```solidity
361:	if(collateralBalance <= minimumCollateral) return 0;
362:	return collateralBalance - minimumCollateral;
```
```solidity
378:	if(collateralBalance <= minimumCollateral) return 0;
379:	return collateralBalance - minimumCollateral;
```
```solidity
533:	require(debt >= amount, "Insufficient debt");
534:	debts[user] -= amount;
```

*src/Fed.sol*
Line(s): [123-124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L123-L124)
```solidity
123:	if(supply >= marketValue) return 0;
124:	return marketValue - supply;
```

*src/DBR.sol*
Line(s): [110-111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L110-L111), [123-124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124), [136-137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137)

```solidity
110:	if(totalDueTokensAccrued > _totalSupply) return 0;
111:	return _totalSupply - totalDueTokensAccrued;
```
```solidity
123:	if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
124:	return balances[user] - dueTokensAccrued[user] - accrued;
```
```solidity
136:	if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
137:	return dueTokensAccrued[user] + accrued - balances[user];
```