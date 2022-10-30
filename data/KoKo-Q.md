# QA ISSUES FOR [2022-10-INVERSE](https://github.com/code-423n4/2022-10-inverse/)

## [N-01] `public` functions not called by the contract should be declared `external` instead

./src/escrows/INVEscrow.sol

```
L44:   function initialize(IERC20 _token, address _beneficiary) public {

L59:   function pay(address recipient, uint amount) public {

L79:   function onDeposit() public {
```

./src/DBR.sol

```
L62:   function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

L70:   function claimOperator() public {

L81:   function addMinter(address minter_) public onlyOperator {

L90:   function removeMinter(address minter_) public onlyOperator {

L158:  function approve(address spender, uint256 amount) public virtual returns (bool) {

L170:  function transfer(address to, uint256 amount) public virtual returns (bool) {

L188:  function transferFrom(

L215:  function permit(

L300:  function onBorrow(address user, uint additionalDebt) public {

L313:  function onRepay(address user, uint repaidDebt) public {

L325:  function onForceReplenish(address user, uint amount) public {
```

./src/escrows/SimpleERC20Escrow.sol

```
L25:   function initialize(IERC20 _token, address) public {

L36:   function pay(address recipient, uint amount) public {

L45:   function balance() public view returns (uint) {
```

./src/Oracle.sol

```
L66:   function claimOperator() public {
```

./src/BorrowController.sol

```
L46:   function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
```

./src/escrows/GovTokenEscrow.sol

```
L30:   function initialize(IERC20 _token, address _beneficiary) public {

L43:   function pay(address recipient, uint amount) public {

L52:   function balance() public view returns (uint) {
```

./src/Market.sol

```
L172:  function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

L203:  function recall(uint amount) public {

L212:  function pauseBorrows(bool _value) public {

L370:  function getWithdrawalLimit(address user) public view returns (uint) {

L520:  function invalidateNonce() public {

L559:  function forceReplenish(address user, uint amount) public {

L575:  @notice View function for getting the amount of liquidateable debt a user holds.

L578:  function getLiquidatableDebt(address user) public view returns (uint) {

L591:  function liquidate(address user, uint repaidDebt) public {
```

./src/Fed.sol

```
L57:   function changeSupplyCeiling(uint _supplyCeiling) public {

L66:   function changeChair(address _chair) public {

L75:   function resign() public {

L86:   function expansion(IMarket market, uint amount) public {

L103:  function contraction(IMarket market, uint amount) public {
```

## [N-02] Events should have 3 `indexed` fields where possible

./src/Market.sol

```
L616:  event Withdraw(address indexed account, address indexed to, uint amount);

L618:  event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);

L619:  event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
```

./src/DBR.sol

```
L381:  event Transfer(address indexed from, address indexed to, uint256 amount);

L382:  event Approval(address indexed owner, address indexed spender, uint256 amount);
```

## [L-01] Not emiting events in some important functions

Description: Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

./src/Fed.sol

```
L48:   function changeGov(address _gov) public {

L57:   function changeSupplyCeiling(uint _supplyCeiling) public {

L75:   function resign() public {
```

./src/Market.sol

```
L149:  function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

L161:  function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

L172:  function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

L194:  function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

L212:  function pauseBorrows(bool _value) public {
```

./src/DBR.sol

```
L53:   function setPendingOperator(address newOperator_) public onlyOperator {

L62:   function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

./src/escrows/SimpleERC20Escrow.sol

```
L25:   function initialize(IERC20 _token, address) public {
```

./src/escrows/GovTokenEscrow.sol

```
L30:   function initialize(IERC20 _token, address _beneficiary) public {
```

./src/escrows/INVEscrow.sol

```
L44:   function initialize(IERC20 _token, address _beneficiary) public {
```
