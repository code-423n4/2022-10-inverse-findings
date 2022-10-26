


### [L01] Unsafe calls to optional ERC20 functions

#### Impact
decimals(), name() and symbol()
 are optional parts of the ERC20 specification, so there are tokens that
 do not implement them. It’s not safe to cast arbitrary token addresses 
in order to call these functions. If IERC20Metadata is to be relied on, that should be the variable type of the token variable, rather than it being address, so the compiler can verify that types correctly match, rather than this being a runtime failure. See [this]() prior instance of this issue which was marked as Low risk. Do [this](https://github.com/boringcrypto/BoringSolidity/blob/c73ed73afa9273fbce93095ef177513191782254/contracts/libraries/BoringERC20.sol#L49-L55) to resolve the issue.
#### Findings:
```
2022-10-inverse/src/Oracle.sol::5 => function decimals() external view returns (uint8);
```



### [L02] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::33 => token = _token;
2022-10-inverse/escrows/GovTokenEscrow.sol::34 => beneficiary = _beneficiary;
2022-10-inverse/escrows/INVEscrow.sol::35 => xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
2022-10-inverse/escrows/INVEscrow.sol::47 => token = _token;
2022-10-inverse/escrows/INVEscrow.sol::48 => beneficiary = _beneficiary;
2022-10-inverse/escrows/SimpleERC20Escrow.sol::28 => token = _token;
2022-10-inverse/src/BorrowController.sol::14 => operator = _operator;
2022-10-inverse/src/Fed.sol::37 => dbr = _dbr;
2022-10-inverse/src/Fed.sol::38 => dola = _dola;
2022-10-inverse/src/Fed.sol::39 => gov = _gov;
2022-10-inverse/src/Fed.sol::40 => chair = _chair;
2022-10-inverse/src/Fed.sol::41 => supplyCeiling = _supplyCeiling;
2022-10-inverse/src/Fed.sol::50 => gov = _gov;
2022-10-inverse/src/Fed.sol::59 => supplyCeiling = _supplyCeiling;
2022-10-inverse/src/Fed.sol::68 => chair = _chair;
2022-10-inverse/src/GovTokenEscrow.sol::33 => token = _token;
2022-10-inverse/src/GovTokenEscrow.sol::34 => beneficiary = _beneficiary;
2022-10-inverse/src/Oracle.sol::32 => operator = _operator;
```



### [L03] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::30 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/escrows/INVEscrow.sol::44 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/escrows/SimpleERC20Escrow.sol::25 => function initialize(IERC20 _token, address) public {
2022-10-inverse/src/GovTokenEscrow.sol::30 => function initialize(IERC20 _token, address _beneficiary) public {
```



### [L04] Return values of `transfer()`/`transferFrom()` not checked

#### Impact
Not all IERC20 implementations revert() when there’s a failure in transfer()/transferFrom(). The function signature has a boolean
 return value and they indicate errors that way instead. By not checking
 the return value, operations that should have been marked as failed may 
potentially go through without actually making a payment
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
2022-10-inverse/escrows/INVEscrow.sol::63 => token.transfer(recipient, amount);
2022-10-inverse/escrows/SimpleERC20Escrow.sol::38 => token.transfer(recipient, amount);
2022-10-inverse/src/Fed.sol::135 => dola.transfer(gov, profit);
2022-10-inverse/src/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
```



### `Non-Critical Issues`



### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
2022-10-inverse/src/Oracle.sol::101 => return normalizedPrice;
2022-10-inverse/src/Oracle.sol::140 => return normalizedPrice;
```



### [N02] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::67 => require(msg.sender == beneficiary);
2022-10-inverse/escrows/INVEscrow.sol::91 => require(msg.sender == beneficiary);
2022-10-inverse/src/Fed.sol::93 => require(globalSupply <= supplyCeiling);
2022-10-inverse/src/GovTokenEscrow.sol::67 => require(msg.sender == beneficiary);
```



### [N03] constants should be defined rather than using magic numbers


#### Findings:
```
2022-10-inverse/src/Oracle.sol::88 => uint normalizedPrice = price * (10 ** decimals);
2022-10-inverse/src/Oracle.sol::95 => uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
2022-10-inverse/src/Oracle.sol::98 => uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
2022-10-inverse/src/Oracle.sol::122 => uint normalizedPrice = price * (10 ** decimals);
2022-10-inverse/src/Oracle.sol::134 => uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
2022-10-inverse/src/Oracle.sol::137 => uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
```




### [N04] Open TODOs

#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
#### Findings:
```
2022-10-inverse/escrows/INVEscrow.sol::35 => xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```



### [N05] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::30 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/escrows/GovTokenEscrow.sol::43 => function pay(address recipient, uint amount) public {
2022-10-inverse/escrows/GovTokenEscrow.sol::52 => function balance() public view returns (uint) {
2022-10-inverse/escrows/GovTokenEscrow.sol::57 => function onDeposit() public {
2022-10-inverse/escrows/GovTokenEscrow.sol::66 => function delegate(address delegatee) public {
2022-10-inverse/escrows/INVEscrow.sol::44 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/escrows/INVEscrow.sol::59 => function pay(address recipient, uint amount) public {
2022-10-inverse/escrows/INVEscrow.sol::70 => function balance() public view returns (uint) {
2022-10-inverse/escrows/INVEscrow.sol::79 => function onDeposit() public {
2022-10-inverse/escrows/INVEscrow.sol::90 => function delegate(address delegatee) public {
2022-10-inverse/escrows/SimpleERC20Escrow.sol::25 => function initialize(IERC20 _token, address) public {
2022-10-inverse/escrows/SimpleERC20Escrow.sol::36 => function pay(address recipient, uint amount) public {
2022-10-inverse/escrows/SimpleERC20Escrow.sol::45 => function balance() public view returns (uint) {
2022-10-inverse/escrows/SimpleERC20Escrow.sol::50 => function onDeposit() public {
2022-10-inverse/src/BorrowController.sol::10 => address public operator;
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/BorrowController.sol::32 => function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
2022-10-inverse/src/BorrowController.sol::38 => function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
2022-10-inverse/src/BorrowController.sol::46 => function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
2022-10-inverse/src/Fed.sol::48 => function changeGov(address _gov) public {
2022-10-inverse/src/Fed.sol::57 => function changeSupplyCeiling(uint _supplyCeiling) public {
2022-10-inverse/src/Fed.sol::66 => function changeChair(address _chair) public {
2022-10-inverse/src/Fed.sol::86 => function expansion(IMarket market, uint amount) public {
2022-10-inverse/src/Fed.sol::103 => function contraction(IMarket market, uint amount) public {
2022-10-inverse/src/Fed.sol::120 => function getProfit(IMarket market) public view returns (uint) {
2022-10-inverse/src/Fed.sol::131 => function takeProfit(IMarket market) public {
2022-10-inverse/src/GovTokenEscrow.sol::30 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/src/GovTokenEscrow.sol::43 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/GovTokenEscrow.sol::52 => function balance() public view returns (uint) {
2022-10-inverse/src/GovTokenEscrow.sol::57 => function onDeposit() public {
2022-10-inverse/src/GovTokenEscrow.sol::66 => function delegate(address delegatee) public {
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
2022-10-inverse/src/Oracle.sol::66 => function claimOperator() public {
```



### [N06] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-10-inverse/src/Oracle.sol::27 => mapping (address => mapping(uint => uint)) public dailyLows; // token => day => price
2022-10-inverse/src/Oracle.sol::89 => uint day = block.timestamp / 1 days;
2022-10-inverse/src/Oracle.sol::91 => uint todaysLow = dailyLows[token][day];
2022-10-inverse/src/Oracle.sol::93 => uint yesterdaysLow = dailyLows[token][day - 1];
2022-10-inverse/src/Oracle.sol::96 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
2022-10-inverse/src/Oracle.sol::124 => uint day = block.timestamp / 1 days;
2022-10-inverse/src/Oracle.sol::125 => uint todaysLow = dailyLows[token][day];
2022-10-inverse/src/Oracle.sol::126 => if(todaysLow == 0 || normalizedPrice < todaysLow) {
2022-10-inverse/src/Oracle.sol::127 => dailyLows[token][day] = normalizedPrice;
2022-10-inverse/src/Oracle.sol::128 => todaysLow = normalizedPrice;
2022-10-inverse/src/Oracle.sol::132 => uint yesterdaysLow = dailyLows[token][day - 1];
2022-10-inverse/src/Oracle.sol::135 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
```



### [N07] NC-library/interface files should use fixed compiler versions, not floating ones


#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/escrows/INVEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/escrows/SimpleERC20Escrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/BorrowController.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Fed.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/GovTokenEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Oracle.sol::2 => pragma solidity ^0.8.13;
```


### [N08] Interfaces should be moved to separate files

#### Impact
Most of the other interfaces in this project are in their own file in
 the interfaces directory. The interfaces below do not follow this 
pattern
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::5 => interface IERC20 {
2022-10-inverse/escrows/INVEscrow.sol::5 => interface IERC20 {
2022-10-inverse/escrows/INVEscrow.sol::14 => interface IXINV {
2022-10-inverse/escrows/SimpleERC20Escrow.sol::5 => interface IERC20 {
2022-10-inverse/src/Fed.sol::4 => interface IMarket {
2022-10-inverse/src/Fed.sol::10 => interface IDola {
2022-10-inverse/src/Fed.sol::17 => interface IDBR {
2022-10-inverse/src/GovTokenEscrow.sol::5 => interface IERC20 {
2022-10-inverse/src/Oracle.sol::4 => interface IChainlinkFeed {
```



