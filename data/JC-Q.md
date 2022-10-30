
---

## summary

- N-01 public functions not called by the contract should be declared external instead | 52 instances
- N-02 Missing event for critical parameter change | 12 instances
- N-03 Use a more recent version of solidity | 2 instances
- N-04 NatSpec is incomplete | 2 instances
- N-05 Event is missing indexed fields | 6 instances
- N-06 Open TODO | 1 instance
- N-07 The visibility for constructor is ignored | 6 instances
- N-08 Use of ecrecover is susceptible to signature malleability | 1 instance
- N-09 Large multiples of ten should use scientific notation rather than decimal literals for readability | 3 instances

Sub- Total: 84 instances in 8 issues

- L-01 Missing checks for address(0x0) when assigning values to address state variables | 8 instances 
- L-02 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256() | 3 instances
- L-03 FeeOnTransfer Tokens not Supported | 2 instances
- L-04 Unspecific Compiler Version Pragma | 8 instances

Sub-total: 21 instances in 4 issues

Total: 105 instances in 12 issues


---

## N-01 public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public. 
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
 

## N-02 Missing event for critical parameter change 

12 instances in 4 files:

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L6
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L17
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L19

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L11
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L12
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L14

GovTokenEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L6
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L7
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L9
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L10

SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L6
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L7


## N-03 Use a more recent version of solidity

Use a solidity version of at least 0.8.14 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>) 

2 instances:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L2
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L2


## N-04 NatSpec is incomplete

2 instances in 1 file:

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L96
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L100


## N-05 Event is missing indexed fields
Each event should use three indexed fields if there are three or more fields

6 instances in 2 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L616
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L617
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L618
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L619


## N-06 Open TODO 

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35


## N-07 The visibility for constructor is ignored

6 instances in 6 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L30-L42

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L61-L90

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L36-L42

Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L29-L33

BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L13-L15

INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L34-L36



## N-08 Use of ecrecover is susceptible to signature malleability

The built-in EVM precompile ecrecover is susceptible to signature malleability, which could lead to replay attacks 

References: 
- https://swcregistry.io/docs/SWC-117
- https://swcregistry.io/docs/SWC-121
- https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57). 

Recommend considering using OpenZeppelin’s ECDSA library (which prevents this malleability) instead of the built-in function.

1 instance in 1 file:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L226


## N-09 Large multiples of ten should use scientific notation rather than decimal literals for readability 

3 instances in 1 file:

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76



## L-01 Missing checks for address(0x0) when assigning values to address state variables 

8 instances in 4 files:

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L78
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L79
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L40

Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32

BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14


## L-02 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions 
(e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). 
“Unless there is a compelling reason, abi.encode should be preferred”. 
If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

3 instances in 2 files:

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L228

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L427
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L491


## L-03 FeeOnTransfer Tokens not Supported

There exist ERC20 tokens that charge a fee for every transfer() or transferFrom(). 
If this tokens are unsupported, ensure there is proper documentation about it...

2 instances in 2 file:

INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L63

SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L38


## L-04 Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version, e.g. 'pragma solidity ^0.8.0;' -> 'pragma solidity 0.8.4;"

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L2

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L2



