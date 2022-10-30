# 2022-10-INVERSE

## Low Risk And Non-Critical Issues

### Events not emmited on important state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **15** instances of this issue:_

```solidity
File: src/DBR.sol

53:   function setPendingOperator(address newOperator_) public onlyOperator {

62:   function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Fed.sol

48:   function changeGov(address _gov) public {

57:   function changeSupplyCeiling(uint _supplyCeiling) public {

66:   function changeChair(address _chair) public {

75:   function resign() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol

```solidity
File: src/Market.sol

149:  function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161:  function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172:  function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183:  function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194:  function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

212:  function pauseBorrows(bool _value) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

```solidity
File: src/escrows/GovTokenEscrow.sol

30:   function initialize(IERC20 _token, address _beneficiary) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol

```solidity
File: src/escrows/INVEscrow.sol

44:   function initialize(IERC20 _token, address _beneficiary) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol

```solidity
File: src/escrows/SimpleERC20Escrow.sol

25:   function initialize(IERC20 _token, address) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol

### `public` functions not called by the contract should be declared `external` instead

_There are **48** instances of this issue:_

```solidity
File: src/BorrowController.sol

46:   function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol

```solidity
File: src/DBR.sol

53:   function setPendingOperator(address newOperator_) public onlyOperator {

62:   function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

70:   function claimOperator() public {

81:   function addMinter(address minter_) public onlyOperator {

90:   function removeMinter(address minter_) public onlyOperator {

99:   function addMarket(address market_) public onlyOperator {

109:  function totalSupply() public view returns (uint) {

146:  function signedBalanceOf(address user) public view returns (int) {

158:  function approve(address spender, uint256 amount) public virtual returns (bool) {

170:  function transfer(address to, uint256 amount) public virtual returns (bool) {

188:  function transferFrom(

215:  function permit(

258:  function invalidateNonce() public {

300:  function onBorrow(address user, uint additionalDebt) public {

313:  function onRepay(address user, uint repaidDebt) public {

325:  function onForceReplenish(address user, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Fed.sol

48:   function changeGov(address _gov) public {

57:   function changeSupplyCeiling(uint _supplyCeiling) public {

66:   function changeChair(address _chair) public {

75:   function resign() public {

86:   function expansion(IMarket market, uint amount) public {

103:  function contraction(IMarket market, uint amount) public {

131:  function takeProfit(IMarket market) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol

```solidity
File: src/Market.sol

149:  function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161:  function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172:  function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183:  function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194:  function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

203:  function recall(uint amount) public {

212:  function pauseBorrows(bool _value) public {

370:  function getWithdrawalLimit(address user) public view returns (uint) {

520:  function invalidateNonce() public {

559:  function forceReplenish(address user, uint amount) public {

575:  @notice View function for getting the amount of liquidateable debt a user holds.

578:  function getLiquidatableDebt(address user) public view returns (uint) {

591:  function liquidate(address user, uint repaidDebt) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

```solidity
File: src/Oracle.sol

66:   function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol

```solidity
File: src/escrows/GovTokenEscrow.sol

30:   function initialize(IERC20 _token, address _beneficiary) public {

43:   function pay(address recipient, uint amount) public {

52:   function balance() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol

```solidity
File: src/escrows/INVEscrow.sol

44:   function initialize(IERC20 _token, address _beneficiary) public {

59:   function pay(address recipient, uint amount) public {

70:   function balance() public view returns (uint) {

79:   function onDeposit() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol

```solidity
File: src/escrows/SimpleERC20Escrow.sol

25:   function initialize(IERC20 _token, address) public {

36:   function pay(address recipient, uint amount) public {

45:   function balance() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol

### Non-library/interface files should use fixed compiler versions, not floating ones

_There are **8** instances of this issue:_

```solidity
File: src/BorrowController.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol

```solidity
File: src/DBR.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Fed.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol

```solidity
File: src/Market.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

```solidity
File: src/Oracle.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol

```solidity
File: src/escrows/GovTokenEscrow.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol

```solidity
File: src/escrows/INVEscrow.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol

```solidity
File: src/escrows/SimpleERC20Escrow.sol

2:    pragma solidity ^0.8.13;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol

### Event is missing `indexed` fields

_There are **6** instances of this issue:_

```solidity
File: src/DBR.sol

381:  event Transfer(address indexed from, address indexed to, uint256 amount);

382:  event Approval(address indexed owner, address indexed spender, uint256 amount);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Market.sol

616:  event Withdraw(address indexed account, address indexed to, uint amount);

617:  event Repay(address indexed account, address indexed repayer, uint amount);

618:  event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);

619:  event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol
