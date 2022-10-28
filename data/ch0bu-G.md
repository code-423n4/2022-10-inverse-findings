## 1. `require()`/`revert()` strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 [incurs an MSTORE](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs 3 gas

```
76	require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
214	require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
63	require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
326	require(markets[msg.sender], "Only markets can call onForceReplenish");
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol


## 2. Splitting `require()` statements that use && saves gas

Instead of using the && operator in a single require statement to check multiple conditions,use multiple require statements with 1 condition per require statement.

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

```
75	require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
162	require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173	require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184	require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195	require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
448	require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
512	require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
96       uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
97       if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
135      uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
136      if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
```
249      require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol


## 3. Boolean comparisons

Comparing to a constant (`true` or `false`) is a bit more expensive than directly checking the returned boolean value.

`if (<x> == true)` => `if (<x>)`, `if (<x> == false)` => `if (!<x>)`

```
350	require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol


## 4. Usage of `uint/int` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.


## 5. Not using the named return variables when a function returns, wastes deployment gas

```
110      if(totalDueTokensAccrued > _totalSupply) return 0;
111      return _totalSupply - totalDueTokensAccrued;
123      if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
124      return balances[user] - dueTokensAccrued[user] - accrued;
136      if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
137      return dueTokensAccrued[user] + accrued - balances[user];
149      return int(balances[user]) - int(dueTokensAccrued[user]) - int(accrued);
161      return true;
177      return true;
201      return true;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
123      if(supply >= marketValue) return 0;
124      return marketValue - supply;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol
```
53       return token.balanceOf(address(this));
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol
```
73       return invBalance + invBalanceInXInv;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol
```

315      return collateralBalance * oracle.viewPrice(address(collateral), collateralFactorBps) / 1 ether;
326      return collateralBalance * oracle.getPrice(address(collateral), collateralFactorBps) / 1 ether;
336      return collateralValue * collateralFactorBps / 10000;
346      return collateralValue * collateralFactorBps / 10000;
356      if(collateralBalance == 0) return 0;
359      if(collateralFactorBps == 0) return 0;
361      if(collateralBalance <= minimumCollateral) return 0;
362      return collateralBalance - minimumCollateral;
373      if(collateralBalance == 0) return 0;
376      if(collateralFactorBps == 0) return 0;
378      if(collateralBalance <= minimumCollateral) return 0;
379      return collateralBalance - minimumCollateral;
580      if (debt == 0) return 0;
582      if(credit >= debt) return 0;
583      return debt * liquidationFactorBps / 10000;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
46       return token.balanceOf(address(this));
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol


## 6. `keccak256()`should only need to be called on a specific string literal once

It should be saved to an immutable variable, and the variable used instead. If the hash is being used as a part of a function selector, the cast to `bytes4` should also only be done once

```
105	keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
106	keccak256(bytes("DBR MARKET")),
...
432	keccak256(
433		"BorrowOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
434	),
...
497	keccak256(
498		"WithdrawOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
499	),
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
233	keccak256(
234		"Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
235	),
...
270	keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol


## 7. `public` functions not called by the contract should be declared `external` instead

Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from `external` to `public` and can save gas by doing so

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
203	function recall(uint amount) public {
212	function pauseBorrows(bool _value) public {
267	function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {
422	function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
486	function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
520	function invalidateNonce() public {
546	function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {
576	function getLiquidatableDebt(address user) public view returns (uint) {
591	function liquidate(address user, uint repaidDebt) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
53	function setPendingOperator(address newOperator_) public onlyOperator {
62	function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
70	function claimOperator() public {
71	function addMinter(address minter_) public onlyOperator {
90	function removeMinter(address minter_) public onlyOperator {
99	function addMarket(address market_) public onlyOperator {
109	function totalSupply() public view returns (uint) {
146	function signedBalanceOf(address user) public view returns (int) {
158	function approve(address spender, uint256 amount) public virtual returns (bool) {
215	function permit(
258	function invalidateNonce() public {
300	function onBorrow(address user, uint additionalDebt) public {
313	function onRepay(address user, uint repaidDebt) public {
325	function onForceReplenish(address user, uint amount) public {
340	function burn(uint amount) public {
349	function mint(address to, uint amount) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
48	function changeGov(address _gov) public {
57	function changeSupplyCeiling(uint _supplyCeiling) public {
66	function changeChair(address _chair) public {
75	function resign() public {
86	function expansion(IMarket market, uint amount) public {
103	function contraction(IMarket market, uint amount) public {
131	function takeProfit(IMarket market) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol
```
44	function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
53	function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
61	function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
66	function claimOperator() public {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
```
26	function setOperator(address _operator) public onlyOperator { operator = _operator; }
32	function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
38	function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
46	function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol
```
36	function pay(address recipient, uint amount) public {
45	function balance() public view returns (uint) {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol
```
59	function pay(address recipient, uint amount) public {
70	function balance() public view returns (uint) {
79	function onDeposit() public {
90	function delegate(address delegatee) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol
```
43	function pay(address recipient, uint amount) public {
52	function balance() public view returns (uint) {
66	function delegate(address delegatee) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol


## 8. Constructor parameters should be avoided when possible

Constructor parameters are expensive. The contract deployment will be cheaper in gas if they are hard coded instead of using constructor parameters.

```
77	gov = _gov;
78	lender = _lender;
79	pauseGuardian = _pauseGuardian;
80	escrowImplementation = _escrowImplementation;
81	dbr = _dbr;
82	collateral = _collateral;
83	oracle = _oracle;
84	collateralFactorBps = _collateralFactorBps;
85	replenishmentIncentiveBps = _replenishmentIncentiveBps;
86	liquidationIncentiveBps = _liquidationIncentiveBps;
87	callOnDepositCallback = _callOnDepositCallback;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
36	replenishmentPriceBps = _replenishmentPriceBps;
37	name = _name;
38	symbol = _symbol;
39	operator = _operator;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
37	dbr = _dbr;
38	dola = _dola;
39	gov = _gov;
40	chair = _chair;
41	supplyCeiling = _supplyCeiling;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol
```
32	operator = _operator;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
```
14	operator = _operator;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol
```
35	xINV = _xINV;
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol


## 9. Multiple `address`/`ID` mappings can be combined into a single `mapping `of an `address`/`ID` to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key’s keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

```
57	mapping (address => IEscrow) public escrows; // user => escrow
58	mapping (address => uint) public debts; // user => debt
59	mapping(address => uint256) public nonces; // user => nonce
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol
```
19	mapping(address => uint256) public balances;
20	mapping(address => mapping(address => uint256)) public allowance;
23	mapping(address => uint256) public nonces;
24	mapping (address => bool) public minters;
25	mapping (address => bool) public markets;
26	mapping (address => uint) public debts; // user => debt across all tracked markets
27	mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
28	mapping (address => uint) public lastUpdated; // user => last update timestamp
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol
```
25	mapping (address => FeedData) public feeds;
26	mapping (address => uint) public fixedPrices;
27	mapping (address => mapping(uint => uint)) public dailyLows; // token => day => price
```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

