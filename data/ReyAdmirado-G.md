
# Gas


| | issue |
| ----------- | ----------- |
| 1 | [State variables only set in the constructor should be declared immutable](#1-state-variables-only-set-in-the-constructor-should-be-declared-immutable) |
| 2 | [state variable is not used in the contract](#2-state-variable-is-not-used-in-the-contract) |
| 3 | [state variables can be packed into fewer storage slots](#3-state-variables-can-be-packed-into-fewer-storage-slots) |
| 4 | [Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate](#4-multiple-addressid-mappings-can-be-combined-into-a-single-mapping-of-an-addressid-to-a-struct-where-appropriate) |
| 5 | [Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement](#5-add-unchecked--for-subtractions-where-the-operands-cannot-underflow-because-of-a-previous-require-or-if-statement) |
| 6 | [use `>=` instead of `>` because if the operands are equal they will be 0 as well](#6-use--instead-of--because-if-the-operands-are-equal-they-will-be-0-as-well) |
| 7 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#7-x--y-costs-more-gas-than-x--x--y-for-state-variables) |
| 8 | [require()/revert() strings longer than 32 bytes cost extra gas](#8-requirerevert-strings-longer-than-32-bytes-cost-extra-gas) |
| 9 | [splitting require() statements that use `&&` saves gas](#9-splitting-require-statements-that-use--saves-gas) |
| 10 | [internal functions only called once can be inlined to save gas](#10-internal-functions-only-called-once-can-be-inlined-to-save-gas) |
| 11 | [usage of uint/int smaller than 32 bytes (256 bits) incurs overhead](#11-usage-of-uintint-smaller-than-32-bytes-256-bits-incurs-overhead) |
| 12 | [bytes constants are more efficient than string constants](#12-bytes-constants-are-more-efficient-than-string-constants) |
| 13 | [public functions not called by the contract should be declared external instead](#13-public-functions-not-called-by-the-contract-should-be-declared-external-instead) |
| 14 | [Don’t compare boolean expressions to boolean literals](#14-dont-compare-boolean-expressions-to-boolean-literals) |
| 15 | [state variables should be cached in stack variables rather than re-reading them from storage](#15-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage) |
| 16 | [Stack variable used as a cheaper cache for a state variable is only used once](#16-stack-variable-used-as-a-cheaper-cache-for-a-state-variable-is-only-used-once) |
| 17 | [require() or revert() statements that check input arguments should be at the top of the function](#17-require-or-revert-statements-that-check-input-arguments-should-be-at-the-top-of-the-function) |


## 1. State variables only set in the constructor should be declared immutable.

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmaccess (100 gas) with a PUSH32 (3 gas).

- [DBR.sol#L11](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L11)
- [DBR.sol#L12](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L12)


## 2. state variable is not used in the contract

remove it to save gas
- [DBR.sol#L13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)


## 3. state variables can be packed into fewer storage slots

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper.

put it near one of the addresses (L15-16)
- [DBR.sol#L13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)

consider putting these near `gov` because `borrowPaused` has been used with it so it will cause cheaper read.(we focus on `borrowPaused` cheaper read because `callOnDepositCallback` is immutable and has much cheaper read)
- [Market.sol#L52](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L52)
- [Market.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L53)

## 4. Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. 

- [DBR.sol#L27](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L27)
- [DBR.sol#L28](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L28)


## 5. Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement

require(a <= b); x = b - a => require(a <= b); unchecked { x = b - a }
if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }
this will stop the check for overflow and underflow so it will save gas

- [Fed.sol#L124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124)

- [DBR.sol#L111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111)
- [DBR.sol#L137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L137)

- [Market.sol#L362](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L362)
- [Market.sol#L379](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L379)

## 6. use `>=` instead of `>` because if the operands are equal they will be 0 as well

this will not let the operation after the if statement occur which will save gas

- [DBR.sol#L110](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L110)
- [DBR.sol#L123](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123)
- [DBR.sol#L136](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136)


## 7. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas

- [Fed.sol#L91](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91)
- [Fed.sol#L92](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L92)
- [Fed.sol#L110](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110)
- [Fed.sol#L111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L111)

- [DBR.sol#L174](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174)
- [DBR.sol#L198](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198)
- [DBR.sol#L288](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288)
- [DBR.sol#L289](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L289)
- [DBR.sol#L304](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304)
- [DBR.sol#L332](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332)
- [DBR.sol#L360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360)
- [DBR.sol#L362](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L362)

- [DBR.sol#L172](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172)
- [DBR.sol#L196](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196)
- [DBR.sol#L316](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316)
- [DBR.sol#L376](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L376)
- [DBR.sol#L378](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L378)

- [Market.sol#L395](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395)
- [Market.sol#L397](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397)
- [Market.sol#L565](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565)
- [Market.sol#L568](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568)

- [Market.sol#L534](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534)
- [Market.sol#L535](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L535)
- [Market.sol#L599](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L599)
- [Market.sol#L600](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L600)


## 8. require()/revert() strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

- [DBR.sol#L63](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63)
- [DBR.sol#L326](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326)

- [Market.sol#L76](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76)
- [Market.sol#L214](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214)


## 9. splitting require() statements that use `&&` saves gas

this will have a large deployment gas cost but with enough runtime calls the split require version will be 3 gas cheaper

- [DBR.sol#L249](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249)
- [Market.sol#L75](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
- [Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
- [Market.sol#L173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)
- [Market.sol#L184](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)
- [Market.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)
- [Market.sol#L448](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)
- [Market.sol#L512](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512)


## 10. internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

- [DBR.sol#L372](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L372)

- [Market.sol#L226](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226)
- [Market.sol#L353](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353)


## 11. usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

- [Market.sol#L422](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422)
- [Market.sol#L486](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486)

- [Oracle.sol#L85](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L85)
- [Oracle.sol#L86](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L86)
- [Oracle.sol#L87](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L87)
- [Oracle.sol#L119](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L119)
- [Oracle.sol#L120](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L120)
- [Oracle.sol#L121](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L121)


## 12. bytes constants are more efficient than string constants

If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

- [DBR.sol#L11](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L11)
- [DBR.sol#L12](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L12)


## 13. public functions not called by the contract should be declared external instead

- [Fed.sol#L48](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48)
- [Fed.sol#L57](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57)
- [Fed.sol#L66](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66)
- [Fed.sol#L75](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75)
- [Fed.sol#L86](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86)
- [Fed.sol#L103](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103)
- [Fed.sol#L131](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131)

- [DBR.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53)
- [DBR.sol#L62](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62)
- [DBR.sol#L70](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70)
- [DBR.sol#L81](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81)
- [DBR.sol#L90](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90)
- [DBR.sol#L99](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99)
- [DBR.sol#L109](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109)
- [DBR.sol#L146](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146)
- [DBR.sol#L158](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158)
- [DBR.sol#L170](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170)
- [DBR.sol#L188](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188)
- [DBR.sol#L258](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258)
- [DBR.sol#L300](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300)
- [DBR.sol#L313](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313)
- [DBR.sol#L325](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325)

- [BorrowController.sol#L26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)
- [BorrowController.sol#L32](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32)
- [BorrowController.sol#L38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38)
- [BorrowController.sol#L46](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46)

- [Market.sol#L118](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118)
- [Market.sol#L124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124)
- [Market.sol#L130](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130)
- [Market.sol#L136](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136)
- [Market.sol#L142](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142)
- [Market.sol#L149](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149)
- [Market.sol#L161](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161)
- [Market.sol#L172](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172)
- [Market.sol#L183](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183)
- [Market.sol#L194](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194)
- [Market.sol#L203](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203)
- [Market.sol#L212](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212)
- [Market.sol#L267](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267)
- [Market.sol#L370](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L370)
- [Market.sol#L520](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L520)
- [Market.sol#L546](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546)
- [Market.sol#L559](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559)
- [Market.sol#L578](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578)
- [Market.sol#L591](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591)

- [Oracle.sol#L44](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44)
- [Oracle.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53)
- [Oracle.sol#L61](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61)
- [Oracle.sol#L66](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66)


## 14. Don’t compare boolean expressions to boolean literals

`require(<x> == true) => require(<x>)`

- [DBR.sol#L350](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L350)



## 15. state variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 

`collateralFactorBps`
- [Market.sol#L360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360)
- [Market.sol#L377](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377)
`liquidationFeeBps`
- [Market.sol#L605](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605)

here mostly require gonna pass so its possible to have 2 fewer state var read if we cache `pendingOperator` before the require check(use the cached `pendingOperator` stack var for emit instead of `Operator`)
- [Oracle.sol#L69](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L69)
- [DBR.sol#L71](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L71)


## 16. Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it’s cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend

`supply`
- [Fed.sol#L106](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L106)

`debt`
- [DBR.sol#L122](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L122)
- [DBR.sol#L134](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L134)
- [DBR.sol#L147](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L147)
- [DBR.sol#L285](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L285)

- [Market.sol#L357](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L357)
- [Market.sol#L374](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L374)
- [Market.sol#L532](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L532)

`replenishmentCost`
- [DBR.sol#L330](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330)

`implementation`
- [Market.sol#L227](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L227)
- [Market.sol#L293](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L293)

`deployer`
- [Market.sol#L294](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L294)

`collateralBalance`
- [Market.sol#L314](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L314)
- [Market.sol#L325](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L325)

`collateralValue`
- [Market.sol#L335](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L335)
- [Market.sol#L345](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L345)
- [Market.sol#L566](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L566)

`credit`
- [Market.sol#L394](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L394)
- [Market.sol#L581](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L581)

`limit`
- [Market.sol#L461](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L461)

`feedDecimals`
- [Oracle.sol#L85](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L85)
- [Oracle.sol#L119](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L119)

`tokenDecimals`
- [Oracle.sol#L86](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L86)
- [Oracle.sol#L120](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L120)


## 17. require() or revert() statements that check input arguments should be at the top of the function

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas*) in a function that may ultimately revert in the unhappy case.

- [Fed.sol#L87-L89](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87-L89)
- [Fed.sol#L104-L107](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104-L107)

- [DBR.sol#L326-L329](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326-L329)










