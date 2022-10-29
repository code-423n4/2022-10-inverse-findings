## 1. Lack of zero address check

Zero-address checks as input validation on address parameters is always a best practice. This is especially true for critical addresses that are immutable and set in the constructor because they cannot be changed later. Accidentally using zero addresses here will lead to failing logic or force contract redeployment and increased gas costs.

Recommendation : https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

### Proof of Concept
```
Oracle.constructor(address)._operator (src/Oracle.sol#30) lacks a zero-check on :
		- operator = _operator (src/Oracle.sol#32)
Oracle.setPendingOperator(address).newOperator_ (src/Oracle.sol#44) lacks a zero-check on :
		- pendingOperator = newOperator_ (src/Oracle.sol#44)
DolaBorrowingRights.constructor(uint256,string,string,address)._operator (src/DBR.sol#34) lacks a zero-check on :
		- operator = _operator (src/DBR.sol#39)
DolaBorrowingRights.setPendingOperator(address).newOperator_ (src/DBR.sol#53) lacks a zero-check on :
		- pendingOperator = newOperator_ (src/DBR.sol#54)
Market.constructor(address,address,address,address,IDolaBorrowingRights,IERC20,IOracle,uint256,uint256,uint256,bool)._gov (src/Market.sol#62) lacks a zero-check on :
		- gov = _gov (src/Market.sol#77)
Market.constructor(address,address,address,address,IDolaBorrowingRights,IERC20,IOracle,uint256,uint256,uint256,bool)._lender (src/Market.sol#63) lacks a zero-check on :
		- lender = _lender (src/Market.sol#78)
Market.constructor(address,address,address,address,IDolaBorrowingRights,IERC20,IOracle,uint256,uint256,uint256,bool)._pauseGuardian (src/Market.sol#64) lacks a zero-check on :
		- pauseGuardian = _pauseGuardian (src/Market.sol#79)
Market.constructor(address,address,address,address,IDolaBorrowingRights,IERC20,IOracle,uint256,uint256,uint256,bool)._escrowImplementation (src/Market.sol#65) lacks a zero-check on :
		- escrowImplementation = _escrowImplementation (src/Market.sol#80)
Market.setGov(address)._gov (src/Market.sol#130) lacks a zero-check on :
		- gov = _gov (src/Market.sol#130)
Market.setLender(address)._lender (src/Market.sol#136) lacks a zero-check on :
		- lender = _lender (src/Market.sol#136)
Market.setPauseGuardian(address)._pauseGuardian (src/Market.sol#142) lacks a zero-check on :
		- pauseGuardian = _pauseGuardian (src/Market.sol#142)
BorrowController.constructor(address)._operator (src/BorrowController.sol#13) lacks a zero-check on :
		- operator = _operator (src/BorrowController.sol#14)
BorrowController.setOperator(address)._operator (src/BorrowController.sol#26) lacks a zero-check on :
		- operator = _operator (src/BorrowController.sol#26)
```

## 2.  Missing Events for important functions 

There are missing events calls for important function calls like updating address, or setting a value.

Recommendation : https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-arithmetic
### Proof of Concept
```
DolaBorrowingRights.setReplenishmentPriceBps(uint256) (src/DBR.sol#62-65) should emit an event for: 
	- replenishmentPriceBps = newReplenishmentPriceBps_ (src/DBR.sol#64)
Market.setGov(address) (src/Market.sol#130) should emit an event for: 
	- gov = _gov (src/Market.sol#130) 
BorrowController.setOperator(address) (src/BorrowController.sol#26) should emit an event for: 
	- operator = _operator (src/BorrowController.sol#26)
```