1:
Oracle.sol viewPrice() not set todaysLow = normalizedPrice, like getPrice()
Oracle.sol
```
    function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
....
            uint todaysLow = dailyLows[token][day];

+           if(todaysLow == 0 || normalizedPrice < todaysLow) {
+               todaysLow = normalizedPrice;
+           }            

```


2:
Market.sol constructor()/setCollateralFactorBps()  need add check collateralFactorBps>0
when equal zero , all user will can't withdraw
```
    function getWithdrawalLimitInternal(address user) internal returns (uint) {

        if(collateralFactorBps == 0) return 0;; /***collateralFactorBps ==0 return 0 ***/

```

suggest:
Market.sol
```
    constructor (
        address _gov,
        address _lender,
        address _pauseGuardian,
        address _escrowImplementation,
        IDolaBorrowingRights _dbr,
        IERC20 _collateral,
        IOracle _oracle,
        uint _collateralFactorBps,
        uint _replenishmentIncentiveBps,
        uint _liquidationIncentiveBps,
        bool _callOnDepositCallback
    ) {
-       require(_collateralFactorBps < 10000, "Invalid collateral factor");
+       require(_collateralFactorBps >0 && _collateralFactorBps < 10000, "Invalid collateral factor");



    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
-       require(_collateralFactorBps < 10000, "Invalid collateral factor");
+       require(_collateralFactorBps >0 && _collateralFactorBps < 10000, "Invalid collateral factor");
        collateralFactorBps = _collateralFactorBps;
    }
```

3:
setReplenishmentPriceBps() have check replenishmentPriceBps > 0, but the constructor() has no, suggest to add check.
DolaBorrowingRights.sol
```
    constructor(
        uint _replenishmentPriceBps,
        string memory _name,
        string memory _symbol,
        address _operator
    ) {
+       require(_replenishmentPriceBps > 0, "replenishment price must be over 0");    
        replenishmentPriceBps = _replenishmentPriceBps;
```