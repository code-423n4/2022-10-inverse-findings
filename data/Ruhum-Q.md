# Report

- [Low](#low)
  - [L-01: Oracle's fixed price feature is prone to cause issues](#l-01-oracles-fixed-price-feature-is-prone-to-cause-issues)
- [Non-Critical](#non-critical)
  - [N-01: emit events when changing a contract's configuration](#n-01-emit-events-when-changing-a-contracts-configuration)

# Low

## L-01: Oracle's fixed price feature is prone to cause issues

I couldn't come up with a scenario where a fixed price oracle would be usable. It's only usable if you mint your own token in 1:1 ratio to some other token. But, in that case, why use a oracle at all?

Instead, it provides an easy way for an attacker to liquidate all the existing loans if the operator's keys are compromised. We've seen key management issues in the past so it's not too far fetched to assume that it could happen here as well.

The attacker would set the price of all the assets to `1 wei` and thus liquidate all the loans. I'd argue that there is no clear benefit in having this feature in your Oracle contract. I recommend to remove it.

# Non-Critical

## N-01: emit events when changing a contract's configuration

It allows third-parties to track those changes off-chain, e.g. Dune.

```
./src/BorrowController.sol:26:    function setOperator(address _operator) public onlyOperator { operator = _operator; }
./src/DBR.sol:54:    function setPendingOperator(address newOperator_) public onlyOperator {
./src/DBR.sol:63:    function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
./src/Market.sol:119:    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
./src/Market.sol:125:    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
./src/Market.sol:131:    function setGov(address _gov) public onlyGov { gov = _gov; }
./src/Market.sol:137:    function setLender(address _lender) public onlyGov { lender = _lender; }
./src/Market.sol:143:    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
./src/Market.sol:150:    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
./src/Market.sol:162:    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
./src/Market.sol:173:    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
./src/Market.sol:184:    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
./src/Market.sol:195:    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
./src/Oracle.sol:44:    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
./src/Oracle.sol:53:    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
./src/Oracle.sol:59:    @param price The fixed price of the token. Remember to account for decimal precision when setting this.
./src/Oracle.sol:63:    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
./src/Fed.sol:48:    function changeGov(address _gov) public {
./src/Fed.sol:57:    function changeSupplyCeiling(uint _supplyCeiling) public {
./src/Fed.sol:66:    function changeChair(address _chair) public {
```