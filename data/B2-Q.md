## Missing checks for `address(0x0)` when assigning values to `address` state variables

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

```
        operator = _operator;
```
>File: src/BorrowController.sol [(line 14)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14)
```
        operator = _operator;
```
>File: src/DBR.sol [(line 39)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39)
```
        gov = _gov;
        chair = _chair;
        supplyCeiling = _supplyCeiling;
```
>File: src/Fed.sol [(line 39-41)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39-L41)
```
        gov = _gov;
        lender = _lender;
        pauseGuardian = _pauseGuardian;
        escrowImplementation = _escrowImplementation;
```
>File: src/Market.sol [(line 77-80)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L80)
```
        operator = _operator;
```
>File: src/Oracle.sol [(line 32)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32)

## `require()` statement should have descriptive reason strings
```
        require(globalSupply <= supplyCeiling);
```
>File: src/Fed.sol [(line 93)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93)
```
        require(msg.sender == beneficiary);
```
>File: src/escrows/GovTokenEscrow.sol [(line 67)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67)
```
        require(msg.sender == beneficiary);
```
>File: src/escrows/INVEscrow.sol [(line 91)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L91)


## Use `msg.sender` instead of `tx.origin`
```
        if(msgSender == tx.origin) return true;

```
* File: src/BorrowController.sol [(line 47)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L47)

## `Cross-Chain Replay` attack

Storing the `block.chainid` is not safe. 

```
        INITIAL_CHAIN_ID = block.chainid;
```
* File: src/DBR.sol [(line 40)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L40)
```
        INITIAL_CHAIN_ID = block.chainid;
```
* File: src/Market.sol [(line 88)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L88)
```
        return block.chainid == INITIAL_CHAIN_ID ? INITIAL_DOMAIN_SEPARATOR : computeDomainSeparator();
```
* File: src/DBR.sol [(line 264)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L263)
```
         block.chainid,
```
* File: src/DBR.sol [(line 273)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L273)
```
        return block.chainid == INITIAL_CHAIN_ID ? INITIAL_DOMAIN_SEPARATOR : computeDomainSeparator();
```
* File: src/Market.sol [(line 98)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L98)
```
         block.chainid,
```
* File: src/Market.sol [(line 108)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L108)

## Direct  usage of  `ecrecover()` allows signature malleability

Use the recover function from OpenZeppelin’s ECDSA library for signature verification.

```
         address recoveredAddress = ecrecover(
```
* File: src/DBR.sol [(line 226)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L226)
```
         address recoveredAddress = ecrecover(
```
* File: src/Market.sol [(line 425)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L425)
```
         address recoveredAddress = ecrecover(
```
* File: src/Market.sol [(line 489)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L489)

##  `initialize` functions can be Front-run

This function can be called by everyone. So an attacker may watch the mempool for this  function and may frontrun it by supplying more gas.
This may result in unintended behaviour.

```
        function initialize(IERC20 _token, address _beneficiary) public {
```
* File: src/escrows/GovTokenEscrow.sol [(line 30)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L30)
```
        function initialize(IERC20 _token, address _beneficiary) public {
```
* File: src/escrows/INVEscrow.sol [(line 44)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L44)
```
        function initialize(IERC20 _token, address _beneficiary) public {
```
* File: /src/escrows/SimpleERC20Escrow.sol [(line 25)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L25)

## Consider ordering multiplication first.

Solidity could truncate the results, performing multiplication before division will prevent rounding/truncation in solidity math.

```
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```
* File: /src/DBR.sol [(line 122)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L122)
```
        return collateralValue * collateralFactorBps / 10000;
```
* File: /src/Market.sol [(line 336)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L336)
```
         uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
```
* File: /src/Oracle.sol [(line 336)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L95)
```
         Other instances of this issue are
```
* File: /src/Oracle.sol [(line 98)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L98)
* File: /src/Oracle.sol [(line 134)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L134)
* File: /src/Oracle.sol [(line 137)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L137)
* File: /src/Market.sol [(line 346)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L346)
* File: /src/Market.sol [(line 360)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360)
* File: /src/Market.sol [(line 377)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377)
* File: /src/Market.sol [(line 564)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L564)
* File: /src/Market.sol [(line 583)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L583)
* File: /src/Market.sol [(line 595)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595)
* File: /src/Market.sol [(line 597)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L597)
* File: /src/Market.sol [(line 598)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598)
* File: /src/Market.sol [(line 606)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606)
* File: /src/DBR.sol [(line 135)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L135)
* File: /src/DBR.sol [(line 148)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L148)
* File: /src/DBR.sol [(line 287)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287)
* File: /src/DBR.sol [(line 330)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330)

## open `TODO` comments

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.

```
        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```
* File: /src/escrows/INVEscrow.sol [(line 35)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35)

## TYPOS
```
     ///@audit: addres 
    @param deniedContract The addres of the denied contract
```
* File: /src/BorrowController.sol [(line 36)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36)
```
    ///@audit oprator
    @notice Sets pending operator of the contract. Operator role must be claimed by the new oprator. Only callable by Operator.
```
* File: /src/DBR.sol [(line 50)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50)
```
    ///@audit allowe
    @notice Removes a minter from the set of addresses allowe to mint DBR tokens. Only callable by Operator.
```
* File: /src/DBR.sol [(line 87)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87)
```
    ///@audit th 
    @notice Get the DBR deficit of an address. Will return 0 if th user has zero DBR or more.
```
* File: /src/DBR.sol [(line 128)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128)
```
    ///@audit mus 
    @dev Collateral factor mus be set below 100%
```
* File: /src/Market.sol [(line 146)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L146)
```
    ///@audit liqudation 
    @param _liquidationIncentiveBps The new liqudation incentive set in basis points. 1 = 0.01% 
```
* File: /src/Market.sol [(line 181)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L181)

## Use of `block.timestamp`

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
      uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```
* File: /src/DBR.sol [(line 122)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L122)
```
      Other instances of this issue are :
```
* File: /src/DBR.sol [(line 135)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L135)
* File: /src/DBR.sol [(line 148)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L148)
* File: /src/DBR.sol [(line 224)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224)
* File: /src/DBR.sol [(line 286)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286)
* File: /src/DBR.sol [(line 287)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287)
* File: /src/DBR.sol [(line 290)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L290)
* File: /src/Market.sol [(line 423)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423)
* File: /src/Market.sol [(line 487)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487)
* File: /src/Oracle.sol [(line 89)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L89)
* File: /src/Oracle.sol [(line 124)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L124)

## Event is missing `indexed` fields
Each event should use three indexed fields if there are three or more fields.
```
    event Transfer(address indexed from, address indexed to, uint256 amount);
```
* File: /src/DBR.sol [(line 382)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382)
```
    event Transfer(address indexed from, address indexed to, uint256 amount);
```
* File: /src/DBR.sol [(line 381)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381)
```
    event Withdraw(address indexed account, address indexed to, uint amount);
    event Repay(address indexed account, address indexed repayer, uint amount);
    event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);
    event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
```
* File: /src/Market.sol [(line 616-619)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L616-L619)

## `NatSpec` is incomplete
```
    /// @audit Missing: '@return'
    /**
    @notice Internal function for creating an escrow for users to deposit collateral in.
    @dev Uses create2 and minimal proxies to create the escrow at a deterministic address
    @param user The address of the user to create an escrow for.
    */
    function createEscrow(address user) internal returns (IEscrow instance) {
```
* File: /src/Market.sol [(line 221-226)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L221-L226)
```
    /// @audit Missing: '@return'
    /**
    @notice Internal function for getting the escrow of a user.
    @dev If the escrow doesn't exist, an escrow contract is deployed.
    @param user The address of the user owning the escrow.
    */
    function getEscrow(address user) internal returns (IEscrow) {
```
* File: /src/Market.sol [(line 240-245)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L240-L245)
```
    /// @audit Missing: '@return'
    /**
    @notice View function for predicting the deterministic escrow address of a user.
    @dev Only use deposit() function for deposits and NOT the predicted escrow address unless you know what you're doing
    @param user Address of the user owning the escrow.
    */
    function predictEscrow(address user) public view returns (IEscrow predicted) {
```
* File: /src/Market.sol [(line 287-292)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L287-L292)
```
    /// @audit Missing: '@return'
    /**
    @notice View function for getting the dollar value of the user's collateral in escrow for the market.
    @param user Address of the user.
    */
    function getCollateralValue(address user) public view returns (uint) {
```
* File: /src/Market.sol [(line 308-312](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L308-L312)
```
    /// @audit Missing: '@return'
    /**
    @notice Internal function for getting the dollar value of the user's collateral in escrow for the market.
    @dev Updates the lowest price comparisons of the pessimistic oracle
    @param user Address of the user.
    */
    function getCollateralValueInternal(address user) internal returns (uint) {
```
* File: /src/Market.sol [(line 318-323)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L318-L323)
```
   /// @audit Missing: '@return'
    /**
    @notice View function for getting the credit limit of a user.
    @dev To calculate the available credit, subtract user debt from credit limit.
    @param user Address of the user.
    */
    function getCreditLimit(address user) public view returns (uint) {
```
* File: /src/Market.sol [(line 329-334)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L329-L334)
```
   /// @audit Missing: '@return'
    /**
    @notice Internal function for getting the credit limit of a user.
    @dev To calculate the available credit, subtract user debt from credit limit. Updates the pessimistic oracle.
    @param user Address of the user.
    */
    function getCreditLimitInternal(address user) internal returns (uint) {
```
* File: /src/Market.sol [(line 339-344)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L339-L344)
```
  /// @audit Missing: '@return'
   /**
    @notice Internal function for getting the withdrawal limit of a user.
     The withdrawal limit is how much collateral a user can withdraw before their loan would be underwater. Updates the pessimistic oracle.
    @param user Address of the user.
    */
    function getWithdrawalLimitInternal(address user) internal returns (uint) {
```
* File: /src/Market.sol [(line 348-353)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L348-L353)
```
    /// @audit Missing: '@return'
    /**
    @notice View function for getting the withdrawal limit of a user.
     The withdrawal limit is how much collateral a user can withdraw before their loan would be underwater.
    @param user Address of the user.
    */
    function getWithdrawalLimit(address user) public view returns (uint) {
```
* File: /src/Market.sol [(line 365-370)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L365-L370)
```
    /// @audit Missing: '@return'
    /**
    @notice View function for getting the amount of liquidateable debt a user holds.
    @param user The address of the user.
    */
    function getLiquidatableDebt(address user) public view returns (uint) {
```
* File: /src/Market.sol [(line 574-578)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L574-L578)
