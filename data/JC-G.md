 
---

## Summary

- G-01 Multiple address mappings can be combined into a single mapping of an address to a struct |  14 instances
- G-02 State variables only set in the constructor should be declared immutable | 2 instances
- G-03 State variables can be packed into fewer storage slots | 1 instances
- G-04 <x> += <y> costs more gas than <x> = <x> + <y> for state variables | 15 instances
- G-05 Splitting require() statements that use && saves gas | 7 instances
- G-06 Public functions to external | 52 instances
- G-07 Use simple comparison in trinary logic / in if statement | 15 instances
- G-08 Amounts should be checked for 0 before calling a transfer | 6 instances

Total: 112 instances in 8 issues

---
 
## G-01 Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

14 instances in 3 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L19
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L20
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L23
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L24
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L25
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L27
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L28

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L58
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L59

Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L25
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L27


## G-02 State variables only set in the constructor should be declared immutable

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas)

2 instances in 1 file:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L11
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L12


## G-03 State variables can be packed into fewer storage slots

Solidity contracts have contiguous 32 bytes (256 bits) slots used in storage. 
By arranging the variables, it is possible to minimize the number of slots used within a contract’s storage and therefore reduce deployment costs. 
address type variables are each of 20 bytes size (way less than 32 bytes). 
However, they here take up a whole 32 bytes slot (they are contiguous). 
As bool type variables are of size 1 byte, there’s a slot here that can get saved by moving them closer to an address

2 instances in 2 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L24-L25

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L52-L53


## G-04 <x> += <y> costs more gas than <x> = <x> + <y> for state variables

15 instances in 3 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L289
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L362

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L92

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598

 

## G-05 Splitting require() statements that use && saves gas

Instead of using the && operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement (saving 3 gas per &).

7 instances in 1 file:

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512



## G-06 Public functions to external

Public functions not called by the contract should be declared external instead. 
Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so. 
https://docs.soliditylang.org/en/latest/contracts.html#function-overriding

52 instances in 8 files:

BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L120
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L133
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L223
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L120
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131

Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66

GovTokenEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L30
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L52
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L66

INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L70
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L79
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L90

SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L25
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L45
 

## G-07 Use simple comparison in trinary logic / in if statement 

The comparison operators >= and <= use more gas than >, <, or ==. 
Replacing the >= and ≤ operators with a comparison operator that has an opcode in the EVM saves gas. 

Recommended Mitigation Steps: 
Replace the comparison operator and reverse the logic to save gas using the suggestions above.

15 instances in 3 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L171
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L329
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L373

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L396
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L462
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L562
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L567
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L582
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L607

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L123



## G-08 Amounts should be checked for 0 before calling a transfer

Checking non-zero transfer values can avoid an expensive external call and save gas. 
While this is done at some places, it’s not consistently done in the solution. 

Recommended Mitigation Steps: 
I suggest adding a non-zero-value before calling a transfer.

6 instances in 2 files:

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L205
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L399
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L570

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L135

INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L63

SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L38





