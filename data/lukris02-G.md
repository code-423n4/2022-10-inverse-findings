# Gas Optimizations Report for Inverse Finance contest

## Overview
During the audit, 4 gas issues were found.  
Total savings ~1320.

№ | Title | Instance Count
--- | --- | --- 
G-1 | [Use unchecked blocks for subtractions where underflow is impossible](#g-1-use-unchecked-blocks-for-subtractions-where-underflow-is-impossible) | 12
G-2 | [Cache state variables instead of reading them from storage multiple times](#g-2-cache-state-variables-instead-of-reading-them-from-storage-multiple-times) | 4
G-3 | [Use local variable cache instead of accessing mapping or array multiple times](#g-3-use-local-variable-cache-instead-of-accessing-mapping-or-array-multiple-times) | 7
G-4 | [Use inline function for internal function called once](#g-4-use-inline-function-for-internal-function-called-once) | 2

## Gas Optimizations Findings (4)
### G-1. Use unchecked blocks for subtractions where underflow is impossible
##### Description
In Solidity 0.8+, there’s a default overflow and underflow check on unsigned integers. When an overflow or underflow isn’t possible (after require or if-statement), some gas can be saved by using [unchecked blocks](https://docs.soliditylang.org/en/v0.8.17/control-structures.html#checked-or-unchecked-arithmetic).

##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L362
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L379
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L124
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L137
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374

##### Saved
This saves ~35.  
So, ~35*12 = 420

#
### G-2. Cache state variables instead of reading them from storage multiple times
##### Description
Stack read is much cheaper than storage read.

##### Instances
- (pendingOperator) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L68
- (collateralFactorBps) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360
- (collateralFactorBps) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377
- (liquidationFeeBps) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606

##### Saved
This saves ~100.  
So, ~100*4 = 400

#
### G-3. Use local variable cache instead of accessing mapping or array multiple times
##### Description
It saves gas due to not having to perform the same key’s keccak256 hash and/or offset recalculation.

##### Instances
- (feeds[token], 4th access) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L86
- (feeds[token], 4th access) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L120
- (balances[user] and dueTokensAccrued[user], 2nd access) https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L124
- (balances[user] and dueTokensAccrued[user], 2nd access) https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L137
- (lastUpdated[user], 2nd access) https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287

##### Saved
This saves ~40.  
So, ~40*11 = 440

#
### G-4. Use inline function for internal function called once
##### Description
Function calls need two extra JUMP instructions and other stack operations.

##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L372
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226

##### Saved
This saves ~20-40.  
So, ~30*2 = 60