1. Missing zero address check
The following functions and constructors are missing zero address checks. This may lead to the contracts being redeployed or some calls in contracts functions returning default values due to address(0).
**Occurrences**:
BorrowController.constructor()
Oracle.constructor()
Fed.constructor()
Market.constructor()
BorrowController.setOperator()
Oracle.setPendingOperator()
Oracle.setFeed()
Fed.changeChair()
Fed.changeGov()
Fed.changeSupplyCeiling()
Fed.resign()
Market.setOracle()
Market.setBorrowController()
Market.setGov()
Market.setLender()
Market.setPauseGuardian()
.



2. Missing zero value check
The following are missing zero value checks. This may lead to unfavourable values or reverts.

Oracle.setFeed() - price
Market.collateralFactorBps()


3. Missing events and emit for critical operations
The following are missing events and corresponding emits for contract critical operations. This may lead to inadequate visibility in terms of monitoring.
**Occurrences**

BorrowController.setOperator()
BorrowController.allow()
BorrowController.deny()
Fed.changeChair()
Fed.changeGov()
Fed.changeSupplyCeiling()
Fed.resign()
Fed.takeProfit()
Market.setOracle()
Market.setBorrowController()
Market.setGov()
Market.setLender()
Market.setPauseGuardian()