# LOW


## 1. Immutable addresses lack zero-address check

Constructors should check the address written in an immutable address variable is not the zero address.

```
41	address public immutable escrowImplementation;
61	constructor (
65		address _escrowImplementation,
73	) {
80		escrowImplementation = _escrowImplementation;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol


## 2. Setters should check the input value

Setters should check the input value - ie make revert if it is the zero address or zero

```
44	function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
53	function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
61	function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
```
130	function setGov(address _gov) public onlyGov { gov = _gov; }
136	function setLender(address _lender) public onlyGov { lender = _lender; }
142	function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
149	function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
53	function setPendingOperator(address newOperator_) public onlyOperator {
62	function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
26	function setOperator(address _operator) public onlyOperator { operator = _operator; }
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol


# NON-CRITICAL


## 1. Non-library/interface files should use fixed compiler versions, not floating ones

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

```
All contracts have floating pragma directive
```


## 2. Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
35       xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol


## 3. Consider adding checks for signature malleability

Use OpenZeppelin’s `ECDSA` contract rather than calling `ecrecover()` directly

```
226      address recoveredAddress = ecrecover(
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
425      address recoveredAddress = ecrecover(
489      address recoveredAddress = ecrecover(
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol


## 4. Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

```
147	event RecordDailyLow(address indexed token, uint price);
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
```
614	event Deposit(address indexed account, uint amount);
615	event Borrow(address indexed account, uint amount);
616	event Withdraw(address indexed account, address indexed to, uint amount);
617	event Repay(address indexed account, address indexed repayer, uint amount);
618	event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);
619	event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
620	event CreateEscrow(address indexed user, address escrow);
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
140	event Expansion(IMarket indexed market, uint amount);
141	event Contraction(IMarket indexed market, uint amount);
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol
```
381	event Transfer(address indexed from, address indexed to, uint256 amount);
382	event Approval(address indexed owner, address indexed spender, uint256 amount);
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol



## 5. Missing event for critical parameter change

Events help non-contract tools to track changes, and events prevent users from being surprised by changes

```
44	function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
53	function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
61	function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
```
118	function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
124	function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
130	function setGov(address _gov) public onlyGov { gov = _gov; }
136	function setLender(address _lender) public onlyGov { lender = _lender; }
142	function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
149	function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
161	function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
172	function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
183	function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
194	function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
53	function setPendingOperator(address newOperator_) public onlyOperator {
62	function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
26	function setOperator(address _operator) public onlyOperator { operator = _operator; }
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol


## 6. Call `for`/`from` variants instead of copying and pasting code

Duplicating code can lead to errors when a change is made to only one of the locations

```
function pay(address recipient, uint amount) public {
	require(msg.sender == market, "ONLY MARKET");
	token.transfer(recipient, amount);
```
Same in:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol
```
function balance() public view returns (uint) {
	return token.balanceOf(address(this));
```
Same in:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol
```
function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
```
Same in:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
66	function claimOperator() public {
67		require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
68		operator = pendingOperator;
69		pendingOperator = address(0);
70		emit ChangeOperator(operator);
```
Same in:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol


## 7. `require()`/`revert()` statements should have descriptive reason strings

```
 91	require(msg.sender == beneficiary);
 ```
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol
```
67	require(msg.sender == beneficiary);
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol


