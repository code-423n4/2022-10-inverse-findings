
# QA

| | issue |
| ----------- | ----------- |
| 1 | [typo in comments](#1-typo-in-comments) |
| 2 | [use of floating pragma](#2-use-of-floating-pragma) |
| 3 | [event is missing indexed fields](#3-event-is-missing-indexed-fields) |
| 4 | [require()/revert() statements should have descriptive reason strings or custom error](#4-requirerevert-statements-should-have-descriptive-reason-strings-or-custom-error) |
| 5 | [constants should be defined rather than using magic numbers](#5-constants-should-be-defined-rather-than-using-magic-numbers) |
| 6 | [lines are too long](#6-lines-are-too-long) |
| 7 | [incorrect functions visibility](#7-incorrect-functions-visibility) |
| 8 | [Use Underscores for Number Literals](#8-use-underscores-for-number-literals) |
| 9 | [Duplicated require()/revert() checks should be refactored to a modifier or function](#9-duplicated-requirerevert-checks-should-be-refactored-to-a-modifier-or-function) |
| 10 | [use the modifier instead of require()](#10-use-the-modifier-instead-of-require) |
| 11 | [Code Style: non-constant should not be named in all caps](#11-code-style-non-constant-should-not-be-named-in-all-caps) |
| 12 | [abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()](#12-abiencodepacked-should-not-be-used-with-dynamic-types-when-passing-the-result-to-a-hash-function-such-as-keccak256) |
| 13 | [Mult instead div in compares](#13-mult-instead-div-in-compares) |
| 14 | [open todos](#14-open-todos) |

## 1. typo in comments

oprator --> operator
- [DBR.sol#L50](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50)

replen --> replenishmentPriceBps
- [DBR.sol#L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60)

allowe --> allowed
- [DBR.sol#L87](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87)

addres --> address
- [BorrowController.sol#L36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36)

liquidateable --> liquidatable (used as liquidatable in the code)
- [Market.sol#L575](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L575)


## 2. use of floating pragma
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

- [Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol)
- [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol)
- [BorrowController.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol)
- [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol)
- [Oracle.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol)
- [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol)
- [INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol)
- [SimpleERC20Escrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol)


## 3. event is missing indexed fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

- [DBR.sol#L381](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381)
- [DBR.sol#L382](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382)

- [Market.sol#L616](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L616)
- [Market.sol#L617](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L617)
- [Market.sol#L618](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L618)
- [Market.sol#L619](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L619)



## 4. require()/revert() statements should have descriptive reason strings or custom error

- [Fed.sol#L93](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93)


## 5. constants should be defined rather than using magic numbers 
Even assembly can benefit from using readable constants instead of hex/numeric literals

- [DBR.sol#L330](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330)

- [Market.sol#L74](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74)
- [Market.sol#L76](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76)
- [Market.sol#L75](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
- [Market.sol#L150](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150)
- [Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
- [Market.sol#L173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)
- [Market.sol#L184](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)
- [Market.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)
- [Market.sol#L336](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L336)
- [Market.sol#L346](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L346)
- [Market.sol#L360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360)
- [Market.sol#L377](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377)
- [Market.sol#L563](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563)
- [Market.sol#L564](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L564)
- [Market.sol#L583](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L583)
- [Market.sol#L595](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595)
- [Market.sol#L598](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598)
- [Market.sol#L606](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606)

- [Oracle.sol#L87](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L87)
- [Oracle.sol#L95](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L95)
- [Oracle.sol#L98](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L98)
- [Oracle.sol#L121](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L121)
- [Oracle.sol#L134](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L134)
- [Oracle.sol#L137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L137)


## 6. lines are too long
Usually lines in source code are limited to 80 characters. Its advised to keep lines lower than 120 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

- [Fed.sol#L99](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L99)

- [Oracle.sol#L12](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12)

- [GovTokenEscrow.sol#L16](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L16)


## 7. incorrect functions visibility
Whenever a function is not being called internally in the code, it can be easily declared as external. While the entire code base have explicit visibilities for every function, some of them can be changed to be external.

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


## 8. Use Underscores for Number Literals
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.


- [DBR.sol#L330](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330)

- [Market.sol#L74](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74)
- [Market.sol#L76](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76)
- [Market.sol#L75](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
- [Market.sol#L150](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150)
- [Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
- [Market.sol#L173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)
- [Market.sol#L184](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)
- [Market.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)
- [Market.sol#L336](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L336)
- [Market.sol#L346](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L346)
- [Market.sol#L360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360)
- [Market.sol#L377](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377)
- [Market.sol#L563](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563)
- [Market.sol#L564](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L564)
- [Market.sol#L583](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L583)
- [Market.sol#L595](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595)
- [Market.sol#L598](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598)
- [Market.sol#L606](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606)

- [Oracle.sol#L95](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L95)
- [Oracle.sol#L98](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L98)
- [Oracle.sol#L134](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L134)
- [Oracle.sol#L137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L137)


## 9. Duplicated require()/revert() checks should be refactored to a modifier or function

`require(msg.sender == gov)` 3 uses, first use shown below
- [Fed.sol#L49](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49)

`require(msg.sender == chair)` 3 uses, first use shown below
- [Fed.sol#L76](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76)

`require(dbr.markets(address(market))` 2 uses, first use shown below
- [Fed.sol#L88](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L88)

`require(markets[msg.sender])` 3 uses, first use shown below
- [DBR.sol#L301](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L301)

`require(balanceOf(from) >= amount)` 2 uses, first use shown below
- [DBR.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195)

`require(deadline >= block.timestamp` 2 uses, first use shown below
- [Market.sol#L423](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423)

`require(price > 011)` 2 uses, first use shown below
- [Oracle.sol#L83](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83)


## 10. use the modifier instead of require()

the modifier `onlyGov` was made for this scenario, use it instead of the require() in the rest of contract

- [Market.sol#L92](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L92)

here are the require()s
- [Market.sol#L214](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214)
- [Market.sol#L216](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L216)


## 11. Code Style: non-constant should not be named in all caps

might contain something other than constant which will be confusing if used in all caps

- [DBR.sol#L262](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L262)

- [Market.sol#L97](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97)


## 12. abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()
Use abi.encode instead 
If all the arguments are strings bytes.concat() should be used

- [DBR.sol#L228](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L228)

- [Market.sol#L427](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L427)
- [Market.sol#L491](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L491)


## 13. Mult instead div in compares

To improve algorithm precision instead of using division in comparison use multiplication in the following scenario:

Instead of `a < b / c` use `a * c < b`

- [Market.sol#L595](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595)


## 14. open todos
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

- [INVEscrow.sol#L35](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35)
