### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
src/Market.sol:L104                abi.encode(

src/Market.sol:L431                            abi.encode(

src/Market.sol:L495                            abi.encode(

src/DBR.sol:L232                            abi.encode(

src/DBR.sol:L269                abi.encode(

```

### [G-02] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
src/Market.sol:L118    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

src/Market.sol:L124    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

src/Market.sol:L130    function setGov(address _gov) public onlyGov { gov = _gov; }

src/Market.sol:L136    function setLender(address _lender) public onlyGov { lender = _lender; }

src/Market.sol:L142    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }

src/Market.sol:L149    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

src/Market.sol:L161    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

src/Market.sol:L172    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

src/Market.sol:L183    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

src/Market.sol:L194    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

src/Oracle.sol:L44    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

src/Oracle.sol:L53    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

src/Oracle.sol:L61    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }

src/DBR.sol:L53    function setPendingOperator(address newOperator_) public onlyOperator {

src/DBR.sol:L62    function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

src/DBR.sol:L81    function addMinter(address minter_) public onlyOperator {

src/DBR.sol:L90    function removeMinter(address minter_) public onlyOperator {

src/DBR.sol:L99    function addMarket(address market_) public onlyOperator {

src/BorrowController.sol:L26    function setOperator(address _operator) public onlyOperator { operator = _operator; }

src/BorrowController.sol:L32    function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

src/BorrowController.sol:L38    function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }

```

### [G-03] ```X += Y``` costs more gas than ```X = X + Y``` for state variables.


#### Impact
Consider changing ```X += Y``` to ```X = X + Y``` to save gas.


#### Findings:
```
src/Market.sol:L397        totalDebt += amount;

src/Market.sol:L568        totalDebt += replenishmentCost;

src/Fed.sol:L92        globalSupply += amount;

src/DBR.sol:L289        totalDueTokensAccrued += accrued;

src/DBR.sol:L360        _totalSupply += amount;

```

### [G-04] Revert message greater than 32 bytes


#### Impact
Keep revert message lower than or equal to 32 bytes to save gas.


#### Findings:
```
src/Market.sol:L76        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

src/Market.sol:L214            require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

src/DBR.sol:L63        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

src/DBR.sol:L326        require(markets[msg.sender], "Only markets can call onForceReplenish");

```

### [G-05] Splitting ```require()``` statements that use && saves gas.


#### Impact
Consider splitting the ```require()``` statements to save gas.


#### Findings:
```
src/Market.sol:L75        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

src/Market.sol:L162        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

src/Market.sol:L173        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

src/Market.sol:L184        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

src/Market.sol:L195        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

src/Market.sol:L448            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

src/Market.sol:L512            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

src/DBR.sol:L249            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

```