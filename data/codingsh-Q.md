
### Low Issues

```solidity
DolaBorrowingRights.setReplenishmentPriceBps(uint256) (src/DBR.sol#62-65) should emit an event for: 
	- replenishmentPriceBps = newReplenishmentPriceBps_ (src/DBR.sol#64) 
 ```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-arithmetic

```solidity
DolaBorrowingRights.constructor(uint256,string,string,address)._operator (src/DBR.sol#34) lacks a zero-check on :
		- operator = _operator (src/DBR.sol#39)
DolaBorrowingRights.setPendingOperator(address).newOperator_ (src/DBR.sol#53) lacks a zero-check on :
		- pendingOperator = newOperator_ (src/DBR.sol#54)
 ```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

```solidity
DolaBorrowingRights.totalSupply() (src/DBR.sol#109-112) uses timestamp for comparisons
	Dangerous comparisons:
	- totalDueTokensAccrued > _totalSupply (src/DBR.sol#110)
DolaBorrowingRights.balanceOf(address) (src/DBR.sol#120-125) uses timestamp for comparisons
	Dangerous comparisons:
	- dueTokensAccrued[user] + accrued > balances[user] (src/DBR.sol#123)
DolaBorrowingRights.deficitOf(address) (src/DBR.sol#133-138) uses timestamp for comparisons
	Dangerous comparisons:
	- dueTokensAccrued[user] + accrued < balances[user] (src/DBR.sol#136)
DolaBorrowingRights.transfer(address,uint256) (src/DBR.sol#170-178) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(balanceOf(msg.sender) >= amount,Insufficient balance) (src/DBR.sol#171)
DolaBorrowingRights.transferFrom(address,address,uint256) (src/DBR.sol#188-202) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(balanceOf(from) >= amount,Insufficient balance) (src/DBR.sol#195)
DolaBorrowingRights.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (src/DBR.sol#215-253) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deadline >= block.timestamp,PERMIT_DEADLINE_EXPIRED) (src/DBR.sol#224)
DolaBorrowingRights.accrueDueTokens(address) (src/DBR.sol#284-292) uses timestamp for comparisons
	Dangerous comparisons:
	- lastUpdated[user] == block.timestamp (src/DBR.sol#286)
DolaBorrowingRights.onBorrow(address,uint256) (src/DBR.sol#300-305) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deficitOf(user) == 0,DBR Deficit) (src/DBR.sol#303)
DolaBorrowingRights.onForceReplenish(address,uint256) (src/DBR.sol#325-334) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deficit > 0,No deficit) (src/DBR.sol#328)
	- require(bool,string)(deficit >= amount,Amount > deficit) (src/DBR.sol#329)
DolaBorrowingRights._burn(address,uint256) (src/DBR.sol#372-379) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(balanceOf(from) >= amount,Insufficient balance) (src/DBR.sol#373)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp

```solidity
DolaBorrowingRights.mint(address,uint256) (src/DBR.sol#349-352) compares to a boolean constant:
	-require(bool,string)(minters[msg.sender] == true || msg.sender == operator,ONLY MINTERS OR OPERATOR) (src/DBR.sol#350)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#boolean-equality

```solidity
Pragma version^0.8.13 (src/DBR.sol#2) allows old versions
solc-0.8.13 is not recommended for deployment
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

```solidity
Function DolaBorrowingRights.DOMAIN_SEPARATOR() (src/DBR.sol#262-264) is not in mixedCase
Variable DolaBorrowingRights._totalSupply (src/DBR.sol#14) is not in mixedCase
Variable DolaBorrowingRights.INITIAL_CHAIN_ID (src/DBR.sol#21) is not in mixedCase
Variable DolaBorrowingRights.INITIAL_DOMAIN_SEPARATOR (src/DBR.sol#22) is not in mixedCase
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions


```solidity
BorrowController.setOperator(address) (src/BorrowController.sol#26) should emit an event for: 
	- operator = _operator (src/BorrowController.sol#26) 
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-access-control

```solidity
BorrowController.constructor(address)._operator (src/BorrowController.sol#13) lacks a zero-check on :
		- operator = _operator (src/BorrowController.sol#14)
BorrowController.setOperator(address)._operator (src/BorrowController.sol#26) lacks a zero-check on :
		- operator = _operator (src/BorrowController.sol#26)
   ```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

```solidity
Pragma version^0.8.13 (src/BorrowController.sol#2) allows old versions
solc-0.8.13 is not recommended for deployment
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

```solidity
Parameter BorrowController.setOperator(address)._operator (src/BorrowController.sol#26) is not in mixedCase
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions

```solidity
Fed.constructor(IDBR,IDola,address,address,uint256)._gov (src/Fed.sol#36) lacks a zero-check on :
		- gov = _gov (src/Fed.sol#39)
Fed.constructor(IDBR,IDola,address,address,uint256)._chair (src/Fed.sol#36) lacks a zero-check on :
		- chair = _chair (src/Fed.sol#40)
Fed.changeGov(address)._gov (src/Fed.sol#48) lacks a zero-check on :
		- gov = _gov (src/Fed.sol#50)
Fed.changeChair(address)._chair (src/Fed.sol#66) lacks a zero-check on :
		- chair = _chair (src/Fed.sol#68)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

```solidity
Reentrancy in Fed.contraction(IMarket,uint256) (src/Fed.sol#103-113):
	External calls:
	- market.recall(amount) (src/Fed.sol#108)
	- dola.burn(amount) (src/Fed.sol#109)
	State variables written after the call(s):
	- globalSupply -= amount (src/Fed.sol#111)
Reentrancy in Fed.expansion(IMarket,uint256) (src/Fed.sol#86-95):
	External calls:
	- dola.mint(address(market),amount) (src/Fed.sol#90)
	State variables written after the call(s):
	- globalSupply += amount (src/Fed.sol#92)
	- supplies[market] += amount (src/Fed.sol#91)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2

```solidity
Reentrancy in Fed.contraction(IMarket,uint256) (src/Fed.sol#103-113):
	External calls:
	- market.recall(amount) (src/Fed.sol#108)
	- dola.burn(amount) (src/Fed.sol#109)
	Event emitted after the call(s):
	- Contraction(market,amount) (src/Fed.sol#112)
Reentrancy in Fed.expansion(IMarket,uint256) (src/Fed.sol#86-95):
	External calls:
	- dola.mint(address(market),amount) (src/Fed.sol#90)
	Event emitted after the call(s):
	- Expansion(market,amount) (src/Fed.sol#94)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3

```solidity
Fed.expansion(IMarket,uint256) (src/Fed.sol#86-95) compares to a boolean constant:
	-require(bool,string)(market.borrowPaused() != true,CANNOT EXPAND PAUSED MARKETS) (src/Fed.sol#89)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#boolean-equality

```solidity
Pragma version^0.8.13 (src/Fed.sol#2) allows old versions
solc-0.8.13 is not recommended for deployment
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

```solidity
Parameter Fed.changeGov(address)._gov (src/Fed.sol#48) is not in mixedCase
Parameter Fed.changeSupplyCeiling(uint256)._supplyCeiling (src/Fed.sol#57) is not in mixedCase
Parameter Fed.changeChair(address)._chair (src/Fed.sol#66) is not in mixedCase
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions

```solidity
Oracle.constructor(address)._operator (src/Oracle.sol#30) lacks a zero-check on :
		- operator = _operator (src/Oracle.sol#32)
Oracle.setPendingOperator(address).newOperator_ (src/Oracle.sol#44) lacks a zero-check on :
		- pendingOperator = newOperator_ (src/Oracle.sol#44)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

```solidity
Pragma version^0.8.13 (src/Oracle.sol#2) allows old versions
solc-0.8.13 is not recommended for deployment
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

```solidity
Market.setGov(address) (src/Market.sol#130) should emit an event for: 
	- gov = _gov (src/Market.sol#130) 
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-access-control

```
Market.setCollateralFactorBps(uint256) (src/Market.sol#149-152) should emit an event for: 
	- collateralFactorBps = _collateralFactorBps (src/Market.sol#151) 
Market.setLiquidationFactorBps(uint256) (src/Market.sol#161-164) should emit an event for: 
	- liquidationFactorBps = _liquidationFactorBps (src/Market.sol#163) 
Market.setReplenismentIncentiveBps(uint256) (src/Market.sol#172-175) should emit an event for: 
	- replenishmentIncentiveBps = _replenishmentIncentiveBps (src/Market.sol#174) 
Market.setLiquidationIncentiveBps(uint256) (src/Market.sol#183-186) should emit an event for: 
	- liquidationIncentiveBps = _liquidationIncentiveBps (src/Market.sol#185) 
Market.setLiquidationFeeBps(uint256) (src/Market.sol#194-197) should emit an event for: 
	- liquidationFeeBps = _liquidationFeeBps (src/Market.sol#196) 
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-arithmetic

```
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
```


Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

```solidity
Reentrancy in Market.borrowInternal(address,address,uint256) (src/Market.sol#389-401):
	External calls:
	- require(bool,string)(borrowController.borrowAllowed(msg.sender,borrower,amount),Denied by borrow controller) (src/Market.sol#392)
	- credit = getCreditLimitInternal(borrower) (src/Market.sol#394)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
	State variables written after the call(s):
	- debts[borrower] += amount (src/Market.sol#395)
	- totalDebt += amount (src/Market.sol#397)
Reentrancy in Market.forceReplenish(address,uint256) (src/Market.sol#559-572):
	External calls:
	- collateralValue = getCollateralValueInternal(user) (src/Market.sol#566)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
	State variables written after the call(s):
	- totalDebt += replenishmentCost (src/Market.sol#568)
Reentrancy in Market.liquidate(address,uint256) (src/Market.sol#591-612):
	External calls:
	- require(bool,string)(getCreditLimitInternal(user) < debt,User debt is healthy) (src/Market.sol#594)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
	- price = oracle.getPrice(address(collateral),collateralFactorBps) (src/Market.sol#596)
	State variables written after the call(s):
	- totalDebt -= repaidDebt (src/Market.sol#600)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2

```solidity
Reentrancy in Market.borrowInternal(address,address,uint256) (src/Market.sol#389-401):
	External calls:
	- require(bool,string)(borrowController.borrowAllowed(msg.sender,borrower,amount),Denied by borrow controller) (src/Market.sol#392)
	- credit = getCreditLimitInternal(borrower) (src/Market.sol#394)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
	- dbr.onBorrow(borrower,amount) (src/Market.sol#398)
	- dola.transfer(to,amount) (src/Market.sol#399)
	Event emitted after the call(s):
	- Borrow(borrower,amount) (src/Market.sol#400)
Reentrancy in Market.deposit(address,uint256) (src/Market.sol#278-285):
	External calls:
	- escrow = getEscrow(user) (src/Market.sol#279)
		- escrow.initialize(collateral,user) (src/Market.sol#248)
	- collateral.transferFrom(msg.sender,address(escrow),amount) (src/Market.sol#280)
	- escrow.onDeposit() (src/Market.sol#282)
	Event emitted after the call(s):
	- Deposit(user,amount) (src/Market.sol#284)
Reentrancy in Market.depositAndBorrow(uint256,uint256) (src/Market.sol#267-270):
	External calls:
	- deposit(amountDeposit) (src/Market.sol#268)
		- collateral.transferFrom(msg.sender,address(escrow),amount) (src/Market.sol#280)
		- escrow.onDeposit() (src/Market.sol#282)
		- escrow.initialize(collateral,user) (src/Market.sol#248)
	- borrow(amountBorrow) (src/Market.sol#269)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
		- require(bool,string)(borrowController.borrowAllowed(msg.sender,borrower,amount),Denied by borrow controller) (src/Market.sol#392)
		- dbr.onBorrow(borrower,amount) (src/Market.sol#398)
		- dola.transfer(to,amount) (src/Market.sol#399)
	Event emitted after the call(s):
	- Borrow(borrower,amount) (src/Market.sol#400)
		- borrow(amountBorrow) (src/Market.sol#269)
Reentrancy in Market.forceReplenish(address,uint256) (src/Market.sol#559-572):
	External calls:
	- collateralValue = getCollateralValueInternal(user) (src/Market.sol#566)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
	- dbr.onForceReplenish(user,amount) (src/Market.sol#569)
	- dola.transfer(msg.sender,replenisherReward) (src/Market.sol#570)
	Event emitted after the call(s):
	- ForceReplenish(user,msg.sender,amount,replenishmentCost,replenisherReward) (src/Market.sol#571)
Reentrancy in Market.liquidate(address,uint256) (src/Market.sol#591-612):
	External calls:
	- require(bool,string)(getCreditLimitInternal(user) < debt,User debt is healthy) (src/Market.sol#594)
		- collateralBalance * oracle.getPrice(address(collateral),collateralFactorBps) / 1000000000000000000 (src/Market.sol#326)
	- price = oracle.getPrice(address(collateral),collateralFactorBps) (src/Market.sol#596)
	- dbr.onRepay(user,repaidDebt) (src/Market.sol#601)
	- dola.transferFrom(msg.sender,address(this),repaidDebt) (src/Market.sol#602)
	- escrow.pay(msg.sender,liquidatorReward) (src/Market.sol#604)
	- escrow.pay(gov,liquidationFee) (src/Market.sol#608)
	Event emitted after the call(s):
	- Liquidate(user,msg.sender,repaidDebt,liquidatorReward) (src/Market.sol#611)
Reentrancy in Market.repay(address,uint256) (src/Market.sol#531-539):
	External calls:
	- dbr.onRepay(user,amount) (src/Market.sol#536)
	- dola.transferFrom(msg.sender,address(this),amount) (src/Market.sol#537)
	Event emitted after the call(s):
	- Repay(user,msg.sender,amount) (src/Market.sol#538)
Reentrancy in Market.repayAndWithdraw(uint256,uint256) (src/Market.sol#546-549):
	External calls:
	- repay(msg.sender,repayAmount) (src/Market.sol#547)
		- dbr.onRepay(user,amount) (src/Market.sol#536)
		- dola.transferFrom(msg.sender,address(this),amount) (src/Market.sol#537)
	- withdraw(withdrawAmount) (src/Market.sol#548)
		- escrow.pay(to,amount) (src/Market.sol#464)
		- escrow.initialize(collateral,user) (src/Market.sol#248)
		- minimumCollateral = debt * 1000000000000000000 / oracle.getPrice(address(collateral),collateralFactorBps) * 10000 / collateralFactorBps (src/Market.sol#360)
	Event emitted after the call(s):
	- CreateEscrow(user,address(instance)) (src/Market.sol#237)
		- withdraw(withdrawAmount) (src/Market.sol#548)
	- Withdraw(from,to,amount) (src/Market.sol#465)
		- withdraw(withdrawAmount) (src/Market.sol#548)
Reentrancy in Market.withdrawInternal(address,address,uint256) (src/Market.sol#460-466):
	External calls:
	- limit = getWithdrawalLimitInternal(from) (src/Market.sol#461)
		- minimumCollateral = debt * 1000000000000000000 / oracle.getPrice(address(collateral),collateralFactorBps) * 10000 / collateralFactorBps (src/Market.sol#360)
	- escrow = getEscrow(from) (src/Market.sol#463)
		- escrow.initialize(collateral,user) (src/Market.sol#248)
	Event emitted after the call(s):
	- CreateEscrow(user,address(instance)) (src/Market.sol#237)
		- escrow = getEscrow(from) (src/Market.sol#463)
Reentrancy in Market.withdrawInternal(address,address,uint256) (src/Market.sol#460-466):
	External calls:
	- limit = getWithdrawalLimitInternal(from) (src/Market.sol#461)
		- minimumCollateral = debt * 1000000000000000000 / oracle.getPrice(address(collateral),collateralFactorBps) * 10000 / collateralFactorBps (src/Market.sol#360)
	- escrow = getEscrow(from) (src/Market.sol#463)
		- escrow.initialize(collateral,user) (src/Market.sol#248)
	- escrow.pay(to,amount) (src/Market.sol#464)
	Event emitted after the call(s):
	- Withdraw(from,to,amount) (src/Market.sol#465)

```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3

```solidity
Market.borrowOnBehalf(address,uint256,uint256,uint8,bytes32,bytes32) (src/Market.sol#422-451) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deadline >= block.timestamp,DEADLINE_EXPIRED) (src/Market.sol#423)
Market.withdrawOnBehalf(address,uint256,uint256,uint8,bytes32,bytes32) (src/Market.sol#486-515) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deadline >= block.timestamp,DEADLINE_EXPIRED) (src/Market.sol#487)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp

Market.createEscrow(address) (src/Market.sol#226-238) uses assembly
	- INLINE ASM (src/Market.sol#229-235)
Market.predictEscrow(address) (src/Market.sol#292-306) uses assembly
	- INLINE ASM (src/Market.sol#296-305)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#assembly-usage

Pragma version^0.8.13 (src/Market.sol#2) allows old versions
solc-0.8.13 is not recommended for deployment
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

```solidity
Function Market.DOMAIN_SEPARATOR() (src/Market.sol#97-99) is not in mixedCase
Parameter Market.setOracle(IOracle)._oracle (src/Market.sol#118) is not in mixedCase
Parameter Market.setBorrowController(IBorrowController)._borrowController (src/Market.sol#124) is not in mixedCase
Parameter Market.setGov(address)._gov (src/Market.sol#130) is not in mixedCase
Parameter Market.setLender(address)._lender (src/Market.sol#136) is not in mixedCase
Parameter Market.setPauseGuardian(address)._pauseGuardian (src/Market.sol#142) is not in mixedCase
Parameter Market.setCollateralFactorBps(uint256)._collateralFactorBps (src/Market.sol#149) is not in mixedCase
Parameter Market.setLiquidationFactorBps(uint256)._liquidationFactorBps (src/Market.sol#161) is not in mixedCase
Parameter Market.setReplenismentIncentiveBps(uint256)._replenishmentIncentiveBps (src/Market.sol#172) is not in mixedCase
Parameter Market.setLiquidationIncentiveBps(uint256)._liquidationIncentiveBps (src/Market.sol#183) is not in mixedCase
Parameter Market.setLiquidationFeeBps(uint256)._liquidationFeeBps (src/Market.sol#194) is not in mixedCase
Parameter Market.pauseBorrows(bool)._value (src/Market.sol#212) is not in mixedCase
Variable Market.INITIAL_CHAIN_ID (src/Market.sol#55) is not in mixedCase
Variable Market.INITIAL_DOMAIN_SEPARATOR (src/Market.sol#56) is not in mixedCase
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions

```solidity
Market.createEscrow(address) (src/Market.sol#226-238) uses literals with too many digits:
	- mstore(uint256,uint256)(ptr_createEscrow_asm_0,0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000000000000000000000) (src/Market.sol#231)
Market.createEscrow(address) (src/Market.sol#226-238) uses literals with too many digits:
	- mstore(uint256,uint256)(ptr_createEscrow_asm_0 + 0x28,0x5af43d82803e903d91602b57fd5bf30000000000000000000000000000000000) (src/Market.sol#233)
Market.predictEscrow(address) (src/Market.sol#292-306) uses literals with too many digits:
	- mstore(uint256,uint256)(ptr_predictEscrow_asm_0,0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000000000000000000000) (src/Market.sol#298)
Market.predictEscrow(address) (src/Market.sol#292-306) uses literals with too many digits:
	- mstore(uint256,uint256)(ptr_predictEscrow_asm_0 + 0x28,0x5af43d82803e903d91602b57fd5bf3ff00000000000000000000000000000000) (src/Market.sol#300)
```
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#too-many-digits