## `public` functions not called the contract should be declared `external` instead

contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from `external`to `public`.

```
    function setOperator(address _operator) public onlyOperator { operator = _operator; }
```
>File: src/BorrowController.sol [(line 26)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)
```
    function setPendingOperator(address newOperator_) public onlyOperator {
```
>File: src/DBR.sol [(line 53)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53)
```
    function changeGov(address _gov) public {
```
>File: /src/Fed.sol [(line 48)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48)
```
    function claimOperator() public {
```
>File: /src/Oracle.sol [(line 66)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66)
```
    function balance() public view returns (uint) {
```
>File: src/escrows/GovTokenEscrow.sol [(line 52)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L52)
```
    function pay(address recipient, uint amount) public {
```
>File: src/escrows/INVEscrow.sol [(line 59)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59)
```
    function pay(address recipient, uint amount) public {
```
>File: src/escrows/SimpleERC20Escrow.sol [(line 36)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36
)
```
    Some Instances of this issue are:
```
* [(BorrowController.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32)
* [(BorrowController.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38)
* [(BorrowController.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46)
* [(DBR.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70)
* [(DBR.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158)
* [(DBR.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188)
* [(DBR.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300)
* [(DBR.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313)
* [(DBR.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325)
* [(Fed.sol)](hthttps://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57)
* [(Fed.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66)
* [(Fed.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L74)
* [(Fed.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131)
* [(Oracle.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61)
* [(Oracle.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53)
* [(Oracle.sol)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44)

## Caching `storage` variables in `memory` to save Gas

Anytime you are reading from storage more than once, it is cheaper in gas cost to cache the variable in memory: a SLOAD cost 100gas, while MLOAD and MSTORE cost 3 gas.

```
        ///@audit: totalDueTokensAccrued is used more than one time
        if(totalDueTokensAccrued > _totalSupply) return 0;
        return _totalSupply - totalDueTokensAccrued;
```
>File: /src/DBR.sol [(line 110-111)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L110-L111)
```
        ///@audit: pendingOperator is used more than one time
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
        operator = pendingOperator;
```
>File: /src/Oracle.sol [(line 67-68)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67-L68)
```
        ///@audit: collateralFactorBps is used more than one time
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
```
>File: /src/Market.sol [(line 359-360)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L359-L360)
```
        ///@audit: collateralFactorBps is used more than one time
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
```
>File: /src/Market.sol [(line 376-377)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L376-L377)
```
        ///@audit: liquidationFeeBps is used more than one time
        if(liquidationFeeBps > 0) {
            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
```
>File: /src/Market.sol [(line 605-606)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605-L606)

## Use `calldata` instead of `memory`

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.
 
```
        string memory _name,
        string memory _symbol,
```
>File: /src/DBR.sol [(line 32-33)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L32-L33)

##  Multiple `address` mappings can be combined into a single mapping of an address to a struct, where appropriate.

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot

```
    mapping (address => FeedData) public feeds;
    mapping (address => uint) public fixedPrices;
    mapping (address => mapping(uint => uint)) public dailyLows; /
```
>File: /src/Oracle.sol [(line 25-27)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L25-L27)
```
    mapping(address => uint256) public nonces;
    mapping (address => bool) public minters;
    mapping (address => bool) public markets;
    mapping (address => uint) public debts; // user => debt across all tracked markets
    mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
    mapping (address => uint) public lastUpdated; // user => last update timestamp
```
>File: /src/DBR.sol [(line 23-28)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L23-L28)
```
    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowance;
```
>File: /src/DBR.sol [(line 19-20)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L19-L20)
```
    mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce
```
>File: /src/Market.sol [(line 57-59)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57-L59)

## call `emit` from `storage` is more expensive

Emmitting an event using storage data is more expensive

```
        emit ChangeOperator(operator);
```
>File: /src/DBR.sol [(line 74)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L74)
```
        emit ChangeOperator(operator);
```
>File: /src/Oracle.sol [(line 70)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L70)

## Splitting `require()` statements that use `&&` saves gas (even a single &&)

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

```
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
```
>File: /src/Market.sol [(line 75)]( https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
```
        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
```
>File: /src/Market.sol [(line 162)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
```
        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
```
>File: /src/Market.sol [(line 173)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)
```
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
```
>File: /src/Market.sol [(line 184)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)
```
        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
```
>File: /src/Market.sol [(line 195)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)
```
        require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
>File: /src/Market.sol [(line 448)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)
```
        require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
>File: /src/Market.sol [(line 512)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512)

## `internal` functions only called once can be `inlined` to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
        function getWithdrawalLimitInternal(address user) internal returns (uint) {
```
>File: /src/Market.sol [(line 353)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353)
