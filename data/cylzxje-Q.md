#### Low risk: 
##### [L-01] Missing address(0) checks for these inputs
##### [L-02] `liquidationFeeBps` is not initialized
##### [L-03] Important function like `setGov()` should be a 2 step procedure + Timelock

#### Non-critical:
##### [N-01] Internal functions should have `_` prefix
##### [N-02] Use `UPPERCASE` for constant variable

---
### Low risk:
#### [L-01]  Missing address(0) checks for these inputs
In constructor:
src/Market.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L62-L68

src/DBR.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L34

src/Fed.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L37-L40

src/Oracle.so
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32

src/BorrowController.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14

```sol
address _gov,
address _lender,
address _pauseGuardian,
address _escrowImplementation,

address _operator

address _operator

address _operator

address _operator
```

Recommend adding address(0) checks

#### [L-02] `liquidationFeeBps` is not initialized
This storage variable is not initialized in the constructor. This leads to `liquidate()` will revert if it isn't set due to operation errors, ....

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L383

Recommend initializing `liquidationFeeBps` in the constructor

#### [L-03] Important function like `setGov()` should be a 2 step procedure + Timelock
If there's any operation error that set `gov` wrong will damage for the protocol. 

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130

Recommend adding 2 step procedure: register `gov` as  `pending`  and a transaction from `gov` to confirm `pending gov` to be gov. Perhaps combine with some kind of `Timelock` will make it more robust 

###  Non-critical:
#### [N-01]  Internal functions should have `_` prefix
Styling information

Recommend adding `_` prefix to all internal function 

#### [N-02]  Internal functions should have `_` prefix
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13

Recommend changing to UPPERCASE:
```sol
uint8 public constant DECIMALS = 18;
```