## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|
|-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 3 |
| [GAS&#x2011;2](#GAS&#x2011;2) | `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas | 12 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Using `> 0` Costs More Gas Than `!= 0` When Used On A Uint In A `require()` Statement | 11 |
| [GAS&#x2011;4](#GAS&#x2011;4) | Splitting `require()` Statements That Use `&&` Saves Gas | 8 |
| [GAS&#x2011;5](#GAS&#x2011;5) | `abi.encode()` Is Less Efficient Than `abi.encodepacked()` | 5 |
| [GAS&#x2011;6](#GAS&#x2011;6) | Don’t Compare Boolean Expressions To Boolean Literals | 1 |
| [GAS&#x2011;7](#GAS&#x2011;7) | Public Functions To External | 80 |
| [GAS&#x2011;8](#GAS&#x2011;8) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 14 |
| [GAS&#x2011;9](#GAS&#x2011;9) | Optimize names to save gas | All in-scope contracts |
| [GAS&#x2011;10](#GAS&#x2011;10) | Use `uint256(1)`/`uint256(2)` instead for `true` and `false` boolean states | 8 |
| [GAS&#x2011;11](#GAS&#x2011;11) | Can use `constant` instead of `immutable` | 1 |


Total: 244 contexts over 14 issues

## Gas Optimizations

### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```
19: mapping(address => uint256) public balances;
20: mapping(address => mapping(address => uint256)) public allowance;
23: mapping(address => uint256) public nonces;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L19-L23


### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas

#### <ins>Proof Of Concept</ins>


```
63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L63

```
301: require(markets[msg.sender], "Only markets can call onBorrow");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L301

```
314: require(markets[msg.sender], "Only markets can call onRepay");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L314

```
326: require(markets[msg.sender], "Only markets can call onForceReplenish");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L326

```
89: require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L89

```
75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
76: require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L75-L76

```
93: require(msg.sender == gov, "Only gov can call this function");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L93

```
173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L173

```
184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L184

```
214: require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L214

```
462: require(limit >= amount, "Insufficient withdrawal limit");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L462





### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement

This change saves 6 gas per instance

#### <ins>Proof Of Concept</ins>


```
63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L63

```
328: require(deficit > 0, "No deficit");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L328

```
75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L75

```
162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L162

```
173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L173

```
184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L184

```
195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L195

```
561: require(deficit > 0, "No DBR deficit");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L561

```
592: require(repaidDebt > 0, "Must repay positive debt");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L592

```
83: require(price > 0, "Invalid feed price");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L83

```
117: require(price > 0, "Invalid feed price");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L117





### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Splitting `require()` Statements That Use `&&` Saves Gas

See https://github.com/code-423n4/2022-01-xdefi-findings/issues/128 which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

#### <ins>Proof Of Concept</ins>

```
249: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L249

```
75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L75

```
162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L162

```
173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L173

```
184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L184

```
195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L195

```
448: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L448

```
512: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L512





### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> `abi.encode()` is less efficient than `abi.encodepacked()`

See for more information: https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison 

#### <ins>Proof Of Concept</ins>


```
232: abi.encode(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L232

```
269: abi.encode(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L269

```
104: abi.encode(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L104

```
431: abi.encode(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L431

```
495: abi.encode(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L495





### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> Don't compare boolean expressions to boolean literals

For cases of: `if (<x> == true)`, use `if (<x>)` instead
For cases of: `if (<x> == false)`, use `if (!<x>)` instead

#### <ins>Proof Of Concept</ins>

```
350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L350





### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```
function setOperator(address _operator) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L26

```
function allow(address allowedContract) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L32

```
function deny(address deniedContract) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L38

```
function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L46

```
function setPendingOperator(address newOperator_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L53

```
function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L62

```
function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L70

```
function addMinter(address minter_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L81

```
function removeMinter(address minter_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L90

```
function addMarket(address market_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L99

```
function totalSupply() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L109

```
function balanceOf(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L120

```
function deficitOf(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L133

```
function signedBalanceOf(address user) public view returns (int) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L146

```
function approve(address spender, uint256 amount) public virtual returns (bool) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L158

```
function transfer(address to, uint256 amount) public virtual returns (bool) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L170

```
function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual returns (bool) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L188

```
function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) public virtual {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L215

```
function invalidateNonce() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L258

```
function DOMAIN_SEPARATOR() public view virtual returns (bytes32) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L262

```
function accrueDueTokens(address user) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L284

```
function onBorrow(address user, uint additionalDebt) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L300

```
function onRepay(address user, uint repaidDebt) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L313

```
function onForceReplenish(address user, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L325

```
function burn(uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L340

```
function mint(address to, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L349

```
function changeGov(address _gov) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L48

```
function changeSupplyCeiling(uint _supplyCeiling) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L57

```
function changeChair(address _chair) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L66

```
function resign() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L75

```
function expansion(IMarket market, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L86

```
function contraction(IMarket market, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L103

```
function getProfit(IMarket market) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L120

```
function takeProfit(IMarket market) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L131

```
function DOMAIN_SEPARATOR() public view virtual returns (bytes32) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L97

```
function setOracle(IOracle _oracle) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L118

```
function setBorrowController(IBorrowController _borrowController) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L124

```
function setGov(address _gov) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L130

```
function setLender(address _lender) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L136

```
function setPauseGuardian(address _pauseGuardian) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L142

```
function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L149

```
function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L161

```
function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L172

```
function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L183

```
function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L194

```
function recall(uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L203

```
function pauseBorrows(bool _value) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L212

```
function deposit(uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L258

```
function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L267

```
function deposit(address user, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L278

```
function predictEscrow(address user) public view returns (IEscrow predicted) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L292

```
function getCollateralValue(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L312

```
function getCreditLimit(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L334

```
function getWithdrawalLimit(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L370

```
function borrow(uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L408

```
function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L422

```
function withdraw(uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L472

```
function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L486

```
function invalidateNonce() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L520

```
function repay(address user, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L531

```
function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L546

```
function forceReplenish(address user, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L559

```
function getLiquidatableDebt(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L578

```
function liquidate(address user, uint repaidDebt) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L591

```
function setPendingOperator(address newOperator_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L44

```
function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L53

```
function setFixedPrice(address token, uint price) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L61

```
function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L66

```
function initialize(IERC20 _token, address _beneficiary) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L30

```
function pay(address recipient, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L43

```
function balance() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L52

```
function delegate(address delegatee) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L9

```
function initialize(IERC20 _token, address _beneficiary) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L44

```
function pay(address recipient, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L59

```
function balance() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L70

```
function onDeposit() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L79

```
function delegate(address delegatee) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L10

```
function initialize(IERC20 _token, address) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L25

```
function pay(address recipient, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L36

```
function balance() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L45






### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```
13: uint8 public constant decimals = 18;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L13

```
20: uint8 tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L20

```
85: uint8 feedDecimals = feeds[token].feed.decimals();
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L85

```
119: uint8 feedDecimals = feeds[token].feed.decimals();
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L119

```
86: uint8 tokenDecimals = feeds[token].tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L86

```
120: uint8 tokenDecimals = feeds[token].tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L120

```
87: uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L87

```
121: uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L121




### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://github.com/enzosv/solidity-optimize-name) link for an example of how it works. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### <ins>Proof Of Concept</ins>

Relevant to all in-scope contracts






### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> Use `uint256(1)`/`uint256(2)` instead for `true` and `false` boolean states

If you don't use boolean for storage you will avoid Gwarmaccess 100 gas. In addition, state changes of boolean from `true` to `false` can cost up to ~20000 gas rather than `uint256(2)` to `uint256(1)` that would cost significantly less.

#### <ins>Proof Of Concept</ins>


```
11: mapping(address => bool) public contractAllowlist;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L11

```
32: function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L32

```
38: function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L38

```
24: mapping (address => bool) public minters;.
25: mapping (address => bool) public markets;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L24-L25

```
82: minters[minter_] = true;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L82

```
91: minters[minter_] = false;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L91

```
100: markets[market_] = true;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L100


### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> Can use `constant` instead of `immutable`

No other changes are performed on the `dola` variable, as a result this can be set from `immutable` to `constant`.
See more at: https://docs.soliditylang.org/en/v0.8.13/contracts.html#constant-and-immutable-state-variables

```
IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
```
