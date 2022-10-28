## Use `unchecked` to save gas

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534

## Use calldata instead of memory 

The `memory` keyword in a function argument causes the underlying code to copy the argument into memory. `Calldata` causes the code to read the transaction without copying it directly, ultimately saves gases.

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L32-L33

## `public` function not called by the contract should be `external`

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L74
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32

## `internal` functions should be `inlined` to save gas

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353

## Splitting `require()` statements that use && saves Gas

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173

## Multiple `address` mappings can be combined into a single mapping of an address to a struct, where appropriate.

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L23-L28
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L19-L20
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57-L59
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L25-L27