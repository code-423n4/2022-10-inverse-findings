## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Missing Checks for Address(0x0)  | 33 |
| [LOW&#x2011;2](#LOW&#x2011;2) | IERC20 `approve()` Is Deprecated | 1 |
| [LOW&#x2011;3](#LOW&#x2011;3) | The Contract Should `approve(0)` First | 1 |
| [LOW&#x2011;4](#LOW&#x2011;4) | Missing parameter validation in `constructor` | 15 |

Total: 50 contexts over 4 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Critical Changes Should Use Two-step Procedure | 17 |
| [NC&#x2011;2](#NC&#x2011;2) | Public Functions Not Called By The Contract Should Be Declared External Instead | 40 |
| [NC&#x2011;3](#NC&#x2011;3) | Missing event for critical parameter change | 19 |
| [NC&#x2011;4](#NC&#x2011;4) | `require()` / `revert()` Statements Should Have Descriptive Reason Strings | 3 |
| [NC&#x2011;5](#NC&#x2011;5) | Implementation contract may not be initialized | 4 |
| [NC&#x2011;6](#NC&#x2011;6) | Avoid Floating Pragmas: The Version Should Be Locked | All in-scope contracts |
| [NC&#x2011;7](#NC&#x2011;7) | Use of `ecrecover` is susceptible to signature malleability | 3 |

Total: 94 contexts over 7 issues

## Low Risk Issues

### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Missing Checks for Address(0x0) 

Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

#### <ins>Proof Of Concept</ins>


```
26: function setOperator: address _operator
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L26

```
32: function allow: address allowedContract
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L32

```
38: function deny: address deniedContract
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L38

```
53: function setPendingOperator: address newOperator_
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L53

```
81: function addMinter: address minter_
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L81

```
90: function removeMinter: address minter_
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L90

```
99: function addMarket: address market_
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L99

```
284: function accrueDueTokens: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L284

```
313: function onRepay: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L313

```
325: function onForceReplenish: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L325

```
349: function mint: address to
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L349

```
48: function changeGov: address _gov
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L48

```
66: function changeChair: address _chair
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L66

```
130: function setGov: address _gov
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L130

```
136: function setLender: address _lender
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L136

```
142: function setPauseGuardian: address _pauseGuardian
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L142

```
278: function deposit: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L278

```
422: function borrowOnBehalf: address from
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L422

```
486: function withdrawOnBehalf: address from
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L486

```
531: function repay: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L531

```
559: function forceReplenish: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L559

```
591: function liquidate: address user
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L591

```
44: function setPendingOperator: address newOperator_
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L44

```
53: function setFeed: address token
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L53

```
61: function setFixedPrice: address token
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L61

```
112: function getPrice: address token
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L112

```
30: function initialize: address _beneficiary
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L30

```
43: function pay: address recipient
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L43

```
66: function delegate: address delegatee
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L66

```
44: function initialize: address _beneficiary
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L44

```
59: function pay: address recipient
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L59

```
90: function delegate: address delegatee
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L90

```
36: function pay: address recipient
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L36



#### <ins>Recommended Mitigation Steps</ins>

Consider adding explicit zero-address validation on input parameters of address type.




### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> IERC20 `approve()` Is Deprecated

`approve()` is subject to a known front-running attack. It is deprecated in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead.

https://docs.openzeppelin.com/contracts/3.x/api/token/erc20#IERC20-approve-address-uint256-

#### <ins>Proof Of Concept</ins>

```
50: _token.approve(address(xINV), type(uint).max);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L50



#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeIncreaseAllowance()` / `safeDecreaseAllowance()` instead.



### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> The Contract Should `approve(0)` First

Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

#### <ins>Proof Of Concept</ins>


```
50: _token.approve(address(xINV), type(uint).max);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L50



#### <ins>Recommended Mitigation Steps</ins>

Approve with a zero amount first before setting the actual amount.



### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Missing parameter validation in `constructor`

Some parameters of constructors are not checked for invalid values.

#### <ins>Proof Of Concept</ins>

```
14: operator = _operator;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L14

```
36: replenishmentPriceBps = _replenishmentPriceBps;
37: name = _name;
38: symbol = _symbol;
39: operator = _operator;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L36-L39

```
39: gov = _gov;
40: chair = _chair;
41: supplyCeiling = _supplyCeiling;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L39-L41

```
77: gov = _gov;
78: lender = _lender;
79: pauseGuardian = _pauseGuardian;
80: escrowImplementation = _escrowImplementation;
84: collateralFactorBps = _collateralFactorBps;
86: liquidationIncentiveBps = _liquidationIncentiveBps;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L77-L86

```
32: operator = _operator;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L32



#### <ins>Recommended Mitigation Steps</ins>

Validate the parameters.

## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```
26: function setOperator(address _operator) public onlyOperator { operator = _operator; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L26

```
62: function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L62

```
48: function changeGov(address _gov) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L48

```
57: function changeSupplyCeiling(uint _supplyCeiling) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L57

```
66: function changeChair(address _chair) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L66

```
118: function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L118

```
124: function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L124

```
130: function setGov(address _gov) public onlyGov { gov = _gov; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L130

```
136: function setLender(address _lender) public onlyGov { lender = _lender; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L136

```
142: function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L142

```
149: function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L149

```
161: function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L161

```
172: function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L172

```
183: function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L183

```
194: function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L194

```
53: function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L53

```
61: function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L61



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.



### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Public Functions Not Called By The Contract Should Be Declared External Instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public.

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
function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L62

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
function accrueDueTokens(address user) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L284

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
function takeProfit(IMarket market) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L131

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
function liquidate(address user, uint repaidDebt) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L591

```
function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L53

```
function setFixedPrice(address token, uint price) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L61





### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Missing event for critical parameter change

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

#### <ins>Proof Of Concept</ins>


```
26: function setOperator(address _operator) public onlyOperator { operator = _operator; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L26

```
53: function setPendingOperator(address newOperator_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L53

```
62: function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L62

```
48: function changeGov(address _gov) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L48

```
57: function changeSupplyCeiling(uint _supplyCeiling) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L57

```
66: function changeChair(address _chair) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L66

```
118: function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L118

```
124: function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L124

```
130: function setGov(address _gov) public onlyGov { gov = _gov; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L130

```
136: function setLender(address _lender) public onlyGov { lender = _lender; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L136

```
142: function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L142

```
149: function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L149

```
161: function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L161

```
172: function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L172

```
183: function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L183

```
194: function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L194

```
44: function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L44

```
53: function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L53

```
61: function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L61





### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> `require()` / `revert()` Statements Should Have Descriptive Reason Strings

#### <ins>Proof Of Concept</ins>


```
93: require(globalSupply <= supplyCeiling);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L93

```
67: require(msg.sender == beneficiary);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L67

```
91: require(msg.sender == beneficiary);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L91






### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```
13: constructor(address _operator) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L13

```
30: constructor(
31:      uint _replenishmentPriceBps,
32:      string memory _name,
33:      string memory _symbol,
34:      address _operator
35:  ) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L30-L35

```
29: constructor(
30:       address _operator
31: ) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L29-L31

```
34: constructor(IXINV _xINV) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L34





### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Avoid Floating Pragmas: The Version Should Be Locked

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### <ins>Proof Of Concept</ins>

All in-scope contracts use floating pragmas ^0.8.13 of Solidity 
```
pragma solidity ^0.8.13;
```




### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Use of `ecrecover` is susceptible to signature malleability

The built-in EVM precompile `ecrecover` is susceptible to signature malleability, which could lead to replay attacks.
References:  https://swcregistry.io/docs/SWC-117,  https://swcregistry.io/docs/SWC-121, and  https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57.
While this is not immediately exploitable, this may become a vulnerability if used elsewhere.

#### <ins>Proof Of Concept</ins>


```
226: address recoveredAddress = ecrecover(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L226

```
425: address recoveredAddress = ecrecover(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L425

```
489: address recoveredAddress = ecrecover(
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L489



#### Recommended Mitigation Steps
Consider using OpenZeppelin’s ECDSA library (which prevents this malleability) instead of the built-in function.
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/cryptography/ECDSA.sol



