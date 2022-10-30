## [L-01] DIFFERENT RESTRICTIONS FOR SOME STATE VARIABLES IN `constructor` AND SETTERS
Some state variables have different restrictions when setting them in `constructor` and the corresponding setter functions. It is possible that the values, which can be set in the `constructor`, cannot be set in the setter functions or vice versa. These inconsistencies can confuse users and developers. For better user experience and maintainability, please consider updating the restrictions for the following state variables to be the same in `constructor` and the corresponding setter functions.

`replenishmentPriceBps`'s setter function requires that `newReplenishmentPriceBps_ > 0` but `constructor` does not require that at all.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L30-L42
```solidity
    constructor(
        uint _replenishmentPriceBps,
        string memory _name,
        string memory _symbol,
        address _operator
    ) {
        replenishmentPriceBps = _replenishmentPriceBps;
        ...
    }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62-L65
```solidity
    function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
        replenishmentPriceBps = newReplenishmentPriceBps_;
    }
```

`replenishmentIncentiveBps`'s setter function requires that `_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000` but `constructor` only requires `_replenishmentIncentiveBps < 10000`.

`liquidationIncentiveBps`'s setter function requires that `_liquidationIncentiveBps + liquidationFeeBps < 10000` but `constructor` only requires that `_liquidationIncentiveBps < 10000`.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L61-L90
```solidity
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
        require(_collateralFactorBps < 10000, "Invalid collateral factor");
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
        ...
        collateralFactorBps = _collateralFactorBps;
        replenishmentIncentiveBps = _replenishmentIncentiveBps;
        liquidationIncentiveBps = _liquidationIncentiveBps;
        ...
    }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172-L175
```solidity
    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
        replenishmentIncentiveBps = _replenishmentIncentiveBps;
    }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183-L186
```solidity
    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
        liquidationIncentiveBps = _liquidationIncentiveBps;
    }
```

## [L-02] MISSING TWO-STEP PROCEDURES FOR SETTING `gov`, `lender`, and `pauseGuardian`
The following `setGov`, `setLender`, and `setPauseGuardian` functions can be called to set `gov`, `lender`, and `pauseGuardian`. Because these functions do not include a two-step procedure, it is possible to assign these roles to wrong addresses. When this occurs, the functions that are only callable by these roles can become no-ops or be maliciously called. To reduce the potential attack surface, please consider implementing a two-step procedure for assigning each of these roles.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
```solidity
    function setGov(address _gov) public onlyGov { gov = _gov; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
```solidity
    function setLender(address _lender) public onlyGov { lender = _lender; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
```solidity
    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
```

## [L-03] MISSING `address(0)` CHECKS FOR CRITICAL ADDRESS INPUTS
To prevent unintended behaviors, critical addresses inputs should be checked against `address(0)`.

Please consider checking `_operator` in the following `constructor`.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L30-L42
```solidity
    constructor(
        uint _replenishmentPriceBps,
        string memory _name,
        string memory _symbol,
        address _operator
    ) {
        ...
        operator = _operator;
        ...
    }
```

Please consider checking `_gov`, `_lender`, `_pauseGuardian`, and `_escrowImplementation` in the following `constructor`.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L61-L90
```solidity
    constructor (
        address _gov,
        address _lender,
        address _pauseGuardian,
        address _escrowImplementation,
        ...
    ) {
        ...
        gov = _gov;
        lender = _lender;
        pauseGuardian = _pauseGuardian;
        escrowImplementation = _escrowImplementation;
        ...
    }
```

Please consider checking `_operator` in the following `constructor`.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L29-L33
```solidity
    constructor(
        address _operator
    ) {
        operator = _operator;
    }
```

## [L-04] UNRESOLVED TODO COMMENT
A comment regarding todo indicates that there is an unresolved action item for implementation, which needs to be addressed before the protocol deployment. Please review the todo comment in the following code.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L34-L36
```solidity
    constructor(IXINV _xINV) {
        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
    }
```

## [N-01] TYPOS IN COMMENTS
The phrase in the following comment should be "the user" instead of "th user".
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128
```solidity
    @notice Get the DBR deficit of an address. Will return 0 if th user has zero DBR or more.
```

The word in the following comment should be "debt" instead of "deb".
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L129
```solidity
    @dev The deficit of a user is calculated as the difference between the user's accrued DBR deb + due DBR debt and their balance.
```

The phrase in the following comment should be "The amount" instead of "Th amount".
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L589
```solidity
    @param repaidDebt Th amount of user user debt to liquidate.
```

## [N-02] CONSTANTS CAN BE USED INSTEAD OF MAGIC NUMBERS
To improve readability and maintainability, constants can be used instead of magic numbers. Please consider replacing the magic numbers, such as `10000`, in the following code with constants.

```solidity
src\DBR.sol
  122: uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
  135: uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
  330: uint replenishmentCost = amount * replenishmentPriceBps / 10000;    

src\Market.sol
  75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive"); 
  150: require(_collateralFactorBps < 10000, "Invalid collateral factor"); 
  162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
  173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
  184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
  195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
  377: uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
  563: uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;  
  564: uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;  
  583: return debt * liquidationFactorBps / 10000; 

src\Oracle.sol
  87: uint8 decimals = 36 - feedDecimals - tokenDecimals;
  89: uint day = block.timestamp / 1 days; 
  95: uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000; 
  98: uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps; 
  121: uint8 decimals = 36 - feedDecimals - tokenDecimals; 
  124: uint day = block.timestamp / 1 days; 
  134: uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000; 
  137: uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps; 
```

## [N-03] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. @param and/or @return comments are missing for the following functions. Please consider completing NatSpec comments for them.

```solidity
src\Market.sol
  226: function createEscrow(address user) internal returns (IEscrow instance) {   
  245: function getEscrow(address user) internal returns (IEscrow) {   
  292: function predictEscrow(address user) public view returns (IEscrow predicted) {
  312: function getCollateralValue(address user) public view returns (uint) {  
  323: function getCollateralValueInternal(address user) internal returns (uint) {  
  334: function getCreditLimit(address user) public view returns (uint) {  
  344: function getCreditLimitInternal(address user) internal returns (uint) {  
  353: function getWithdrawalLimitInternal(address user) internal returns (uint) { 
  370: function getWithdrawalLimit(address user) public view returns (uint) { 

src\Oracle.sol
  78: function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {  
```

## [N-04] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. NatSpec comments are missing for the following functions. Please consider adding them.
```solidity
src\DBR.sol
  262: function DOMAIN_SEPARATOR() public view virtual returns (bytes32) { 
  266: function computeDomainSeparator() internal view virtual returns (bytes32) { 

src\Market.sol
  97: function DOMAIN_SEPARATOR() public view virtual returns (bytes32) { 
  101: function computeDomainSeparator() internal view virtual returns (bytes32) { 
```