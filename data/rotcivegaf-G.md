# Gas report

| G-N    |Issue|Instances|
|:------:|:----|:-------:|
| [G&#x2011;01] | `public` functions to `external` functions | 61 |

## [G-01] `public` functions to `external` functions

The following functions could be set external to save gas and improve code quality
`external` call cost is less expensive than of `public` functions

```solidity
File: src/BorrowController.sol

26    function setOperator(address _operator) public onlyOperator { operator = _operator; }

32    function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

38    function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }

46    function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
```

```solidity
File: src/DBR.sol

 53    function setPendingOperator(address newOperator_) public onlyOperator {

 62    function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

 70    function claimOperator() public {

 81    function addMinter(address minter_) public onlyOperator {

 90    function removeMinter(address minter_) public onlyOperator {

 99    function addMarket(address market_) public onlyOperator {

146    function signedBalanceOf(address user) public view returns (int) {

258    function invalidateNonce() public {

300    function onBorrow(address user, uint additionalDebt) public {

313    function onRepay(address user, uint repaidDebt) public {

325    function onForceReplenish(address user, uint amount) public {

340    function burn(uint amount) public {

349    function mint(address to, uint amount) public {
```

```solidity
File: src/Fed.sol

 48    function changeGov(address _gov) public {

 57    function changeSupplyCeiling(uint _supplyCeiling) public {

 66    function changeChair(address _chair) public {

 75    function resign() public {

 86    function expansion(IMarket market, uint amount) public {

103    function contraction(IMarket market, uint amount) public {

131    function takeProfit(IMarket market) public {
```

```solidity
File: src/Market.sol

118    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

124    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

130    function setGov(address _gov) public onlyGov { gov = _gov; }

136    function setLender(address _lender) public onlyGov { lender = _lender; }

142    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }

149    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

203    function recall(uint amount) public {

212    function pauseBorrows(bool _value) public {

267    function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {

370    function getWithdrawalLimit(address user) public view returns (uint) {

422    function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

486    function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

520    function invalidateNonce() public {

546    function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {

559    function forceReplenish(address user, uint amount) public {

578    function getLiquidatableDebt(address user) public view returns (uint) {

591    function liquidate(address user, uint repaidDebt) public {
```

```solidity
File: src/Oracle.sol

44    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

53    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

61    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }

66    function claimOperator() public {
```

```solidity
File: GovTokenEscrow.sol

30    function initialize(IERC20 _token, address _beneficiary) public {

43    function pay(address recipient, uint amount) public {

52    function balance() public view returns (uint) {

66    function delegate(address delegatee) public {
```

```solidity
File: INVEscrow.sol

44    function initialize(IERC20 _token, address _beneficiary) public {

59    function pay(address recipient, uint amount) public {

70    function balance() public view returns (uint) {

79    function onDeposit() public {

90    function delegate(address delegatee) public {
```

```solidity
File: SimpleERC20Escrow.sol

25    function initialize(IERC20 _token, address) public {

36    function pay(address recipient, uint amount) public {

45    function balance() public view returns (uint) {
```

