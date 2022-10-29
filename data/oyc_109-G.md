## [G-01] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
2022-10-inverse/src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
2022-10-inverse/src/DBR.sol::328 => require(deficit > 0, "No deficit");
2022-10-inverse/src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
2022-10-inverse/src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
2022-10-inverse/src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
2022-10-inverse/src/Market.sol::561 => require(deficit > 0, "No DBR deficit");
2022-10-inverse/src/Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");
2022-10-inverse/src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
```

## [G-02] Long Revert Strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

```
2022-10-inverse/src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
2022-10-inverse/src/DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");
2022-10-inverse/src/Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
2022-10-inverse/src/Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

## [G-03] Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```
2022-10-inverse/src/Fed.sol::28 => IDBR public immutable dbr;
2022-10-inverse/src/Fed.sol::29 => IDola public immutable dola;
2022-10-inverse/src/Market.sol::41 => address public immutable escrowImplementation;
2022-10-inverse/src/Market.sol::42 => IDolaBorrowingRights public immutable dbr;
2022-10-inverse/src/Market.sol::44 => IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
2022-10-inverse/src/Market.sol::45 => IERC20 public immutable collateral;
2022-10-inverse/src/escrows/INVEscrow.sol::32 => IXINV public immutable xINV;
```

## [G-04] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/BorrowController.sol::32 => function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
2022-10-inverse/src/BorrowController.sol::38 => function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
2022-10-inverse/src/DBR.sol::53 => function setPendingOperator(address newOperator_) public onlyOperator {
2022-10-inverse/src/DBR.sol::62 => function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
2022-10-inverse/src/DBR.sol::81 => function addMinter(address minter_) public onlyOperator {
2022-10-inverse/src/DBR.sol::90 => function removeMinter(address minter_) public onlyOperator {
2022-10-inverse/src/DBR.sol::99 => function addMarket(address market_) public onlyOperator {
2022-10-inverse/src/Market.sol::118 => function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
2022-10-inverse/src/Market.sol::124 => function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
2022-10-inverse/src/Market.sol::130 => function setGov(address _gov) public onlyGov { gov = _gov; }
2022-10-inverse/src/Market.sol::136 => function setLender(address _lender) public onlyGov { lender = _lender; }
2022-10-inverse/src/Market.sol::142 => function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
2022-10-inverse/src/Market.sol::149 => function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::161 => function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::172 => function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::183 => function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::194 => function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

## [G-05] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-10-inverse/src/DBR.sol::13 => uint8 public constant decimals = 18;
2022-10-inverse/src/Oracle.sol::20 => uint8 tokenDecimals;
```

## [G-06] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
2022-10-inverse/src/BorrowController.sol::11 => mapping(address => bool) public contractAllowlist;
2022-10-inverse/src/DBR.sol::24 => mapping (address => bool) public minters;
2022-10-inverse/src/DBR.sol::25 => mapping (address => bool) public markets;
2022-10-inverse/src/Market.sol::53 => bool public borrowPaused;
```

## [G-07] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-10-inverse/src/DBR.sol::239 => nonces[owner]++,
2022-10-inverse/src/DBR.sol::259 => nonces[msg.sender]++;
2022-10-inverse/src/Market.sol::438 => nonces[from]++,
2022-10-inverse/src/Market.sol::502 => nonces[from]++,
2022-10-inverse/src/Market.sol::521 => nonces[msg.sender]++;
```

## [G-08] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-10-inverse/src/DBR.sol::172 => balances[msg.sender] -= amount;
2022-10-inverse/src/DBR.sol::174 => balances[to] += amount;
2022-10-inverse/src/DBR.sol::196 => balances[from] -= amount;
2022-10-inverse/src/DBR.sol::198 => balances[to] += amount;
2022-10-inverse/src/DBR.sol::288 => dueTokensAccrued[user] += accrued;
2022-10-inverse/src/DBR.sol::289 => totalDueTokensAccrued += accrued;
2022-10-inverse/src/DBR.sol::304 => debts[user] += additionalDebt;
2022-10-inverse/src/DBR.sol::316 => debts[user] -= repaidDebt;
2022-10-inverse/src/DBR.sol::332 => debts[user] += replenishmentCost;
2022-10-inverse/src/DBR.sol::360 => _totalSupply += amount;
2022-10-inverse/src/DBR.sol::362 => balances[to] += amount;
2022-10-inverse/src/DBR.sol::374 => balances[from] -= amount;
2022-10-inverse/src/DBR.sol::376 => _totalSupply -= amount;
2022-10-inverse/src/Fed.sol::91 => supplies[market] += amount;
2022-10-inverse/src/Fed.sol::92 => globalSupply += amount;
2022-10-inverse/src/Fed.sol::110 => supplies[market] -= amount;
2022-10-inverse/src/Fed.sol::111 => globalSupply -= amount;
2022-10-inverse/src/Market.sol::395 => debts[borrower] += amount;
2022-10-inverse/src/Market.sol::397 => totalDebt += amount;
2022-10-inverse/src/Market.sol::534 => debts[user] -= amount;
2022-10-inverse/src/Market.sol::535 => totalDebt -= amount;
2022-10-inverse/src/Market.sol::565 => debts[user] += replenishmentCost;
2022-10-inverse/src/Market.sol::568 => totalDebt += replenishmentCost;
2022-10-inverse/src/Market.sol::598 => liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
2022-10-inverse/src/Market.sol::599 => debts[user] -= repaidDebt;
2022-10-inverse/src/Market.sol::600 => totalDebt -= repaidDebt;
```

## [G-09] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
2022-10-inverse/src/DBR.sol::232 => abi.encode(
2022-10-inverse/src/DBR.sol::269 => abi.encode(
2022-10-inverse/src/Market.sol::104 => abi.encode(
2022-10-inverse/src/Market.sol::431 => abi.encode(
2022-10-inverse/src/Market.sol::495 => abi.encode(
```

## [G-10] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

```
2022-10-inverse/src/BorrowController.sol::18 => require(msg.sender == operator, "Only operator");
2022-10-inverse/src/DBR.sol::45 => require(msg.sender == operator, "ONLY OPERATOR");
2022-10-inverse/src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
2022-10-inverse/src/DBR.sol::71 => require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
2022-10-inverse/src/DBR.sol::171 => require(balanceOf(msg.sender) >= amount, "Insufficient balance");
2022-10-inverse/src/DBR.sol::195 => require(balanceOf(from) >= amount, "Insufficient balance");
2022-10-inverse/src/DBR.sol::224 => require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
2022-10-inverse/src/DBR.sol::249 => require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
2022-10-inverse/src/DBR.sol::301 => require(markets[msg.sender], "Only markets can call onBorrow");
2022-10-inverse/src/DBR.sol::303 => require(deficitOf(user) == 0, "DBR Deficit");
2022-10-inverse/src/DBR.sol::314 => require(markets[msg.sender], "Only markets can call onRepay");
2022-10-inverse/src/DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");
2022-10-inverse/src/DBR.sol::328 => require(deficit > 0, "No deficit");
2022-10-inverse/src/DBR.sol::329 => require(deficit >= amount, "Amount > deficit");
2022-10-inverse/src/DBR.sol::350 => require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
2022-10-inverse/src/DBR.sol::373 => require(balanceOf(from) >= amount, "Insufficient balance");
2022-10-inverse/src/Fed.sol::49 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::58 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::67 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::76 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::87 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::88 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
2022-10-inverse/src/Fed.sol::89 => require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
2022-10-inverse/src/Fed.sol::104 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::105 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
2022-10-inverse/src/Fed.sol::107 => require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
2022-10-inverse/src/Market.sol::74 => require(_collateralFactorBps < 10000, "Invalid collateral factor");
2022-10-inverse/src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
2022-10-inverse/src/Market.sol::93 => require(msg.sender == gov, "Only gov can call this function");
2022-10-inverse/src/Market.sol::150 => require(_collateralFactorBps < 10000, "Invalid collateral factor");
2022-10-inverse/src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
2022-10-inverse/src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
2022-10-inverse/src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
2022-10-inverse/src/Market.sol::204 => require(msg.sender == lender, "Only lender can recall");
2022-10-inverse/src/Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
2022-10-inverse/src/Market.sol::216 => require(msg.sender == gov, "Only governance can unpause");
2022-10-inverse/src/Market.sol::236 => require(instance != IEscrow(address(0)), "ERC1167: create2 failed");
2022-10-inverse/src/Market.sol::390 => require(!borrowPaused, "Borrowing is paused");
2022-10-inverse/src/Market.sol::392 => require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
2022-10-inverse/src/Market.sol::396 => require(credit >= debts[borrower], "Exceeded credit limit");
2022-10-inverse/src/Market.sol::423 => require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
2022-10-inverse/src/Market.sol::448 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
2022-10-inverse/src/Market.sol::462 => require(limit >= amount, "Insufficient withdrawal limit");
2022-10-inverse/src/Market.sol::487 => require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
2022-10-inverse/src/Market.sol::512 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
2022-10-inverse/src/Market.sol::533 => require(debt >= amount, "Insufficient debt");
2022-10-inverse/src/Market.sol::561 => require(deficit > 0, "No DBR deficit");
2022-10-inverse/src/Market.sol::562 => require(deficit >= amount, "Amount > deficit");
2022-10-inverse/src/Market.sol::567 => require(collateralValue >= debts[user], "Exceeded collateral value");
2022-10-inverse/src/Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");
2022-10-inverse/src/Market.sol::594 => require(getCreditLimitInternal(user) < debt, "User debt is healthy");
2022-10-inverse/src/Market.sol::595 => require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
2022-10-inverse/src/Oracle.sol::36 => require(msg.sender == operator, "ONLY OPERATOR");
2022-10-inverse/src/Oracle.sol::67 => require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
2022-10-inverse/src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/escrows/GovTokenEscrow.sol::31 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/src/escrows/GovTokenEscrow.sol::44 => require(msg.sender == market, "ONLY MARKET");
2022-10-inverse/src/escrows/INVEscrow.sol::45 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/src/escrows/INVEscrow.sol::60 => require(msg.sender == market, "ONLY MARKET");
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::26 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::37 => require(msg.sender == market, "ONLY MARKET");
```

## [G-11] Don't compare boolean expressions to boolean literals

The extran comparison wastes gas
if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)

```
2022-10-inverse/src/DBR.sol::350 => require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

## [G-12] Splitting require() statements that use && saves gas

Saves 16 gas per instance.
If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:

```
2022-10-inverse/src/DBR.sol::249 => require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
2022-10-inverse/src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
2022-10-inverse/src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
2022-10-inverse/src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
2022-10-inverse/src/Market.sol::448 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
2022-10-inverse/src/Market.sol::512 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

## [G-13] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/BorrowController.sol::32 => function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
2022-10-inverse/src/BorrowController.sol::38 => function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
2022-10-inverse/src/BorrowController.sol::46 => function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
2022-10-inverse/src/DBR.sol::53 => function setPendingOperator(address newOperator_) public onlyOperator {
2022-10-inverse/src/DBR.sol::62 => function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
2022-10-inverse/src/DBR.sol::70 => function claimOperator() public {
2022-10-inverse/src/DBR.sol::81 => function addMinter(address minter_) public onlyOperator {
2022-10-inverse/src/DBR.sol::90 => function removeMinter(address minter_) public onlyOperator {
2022-10-inverse/src/DBR.sol::99 => function addMarket(address market_) public onlyOperator {
2022-10-inverse/src/DBR.sol::109 => function totalSupply() public view returns (uint) {
2022-10-inverse/src/DBR.sol::146 => function signedBalanceOf(address user) public view returns (int) {
2022-10-inverse/src/DBR.sol::158 => function approve(address spender, uint256 amount) public virtual returns (bool) {
2022-10-inverse/src/DBR.sol::170 => function transfer(address to, uint256 amount) public virtual returns (bool) {
2022-10-inverse/src/DBR.sol::258 => function invalidateNonce() public {
2022-10-inverse/src/DBR.sol::300 => function onBorrow(address user, uint additionalDebt) public {
2022-10-inverse/src/DBR.sol::313 => function onRepay(address user, uint repaidDebt) public {
2022-10-inverse/src/DBR.sol::325 => function onForceReplenish(address user, uint amount) public {
2022-10-inverse/src/Fed.sol::48 => function changeGov(address _gov) public {
2022-10-inverse/src/Fed.sol::57 => function changeSupplyCeiling(uint _supplyCeiling) public {
2022-10-inverse/src/Fed.sol::66 => function changeChair(address _chair) public {
2022-10-inverse/src/Fed.sol::75 => function resign() public {
2022-10-inverse/src/Fed.sol::86 => function expansion(IMarket market, uint amount) public {
2022-10-inverse/src/Fed.sol::103 => function contraction(IMarket market, uint amount) public {
2022-10-inverse/src/Fed.sol::131 => function takeProfit(IMarket market) public {
2022-10-inverse/src/Market.sol::118 => function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
2022-10-inverse/src/Market.sol::124 => function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
2022-10-inverse/src/Market.sol::130 => function setGov(address _gov) public onlyGov { gov = _gov; }
2022-10-inverse/src/Market.sol::136 => function setLender(address _lender) public onlyGov { lender = _lender; }
2022-10-inverse/src/Market.sol::142 => function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
2022-10-inverse/src/Market.sol::149 => function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::161 => function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::172 => function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::183 => function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::194 => function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
2022-10-inverse/src/Market.sol::203 => function recall(uint amount) public {
2022-10-inverse/src/Market.sol::212 => function pauseBorrows(bool _value) public {
2022-10-inverse/src/Market.sol::267 => function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {
2022-10-inverse/src/Market.sol::370 => function getWithdrawalLimit(address user) public view returns (uint) {
2022-10-inverse/src/Market.sol::422 => function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
2022-10-inverse/src/Market.sol::486 => function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
2022-10-inverse/src/Market.sol::520 => function invalidateNonce() public {
2022-10-inverse/src/Market.sol::546 => function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {
2022-10-inverse/src/Market.sol::559 => function forceReplenish(address user, uint amount) public {
2022-10-inverse/src/Market.sol::578 => function getLiquidatableDebt(address user) public view returns (uint) {
2022-10-inverse/src/Market.sol::591 => function liquidate(address user, uint repaidDebt) public {
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
2022-10-inverse/src/Oracle.sol::66 => function claimOperator() public {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::30 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::43 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::52 => function balance() public view returns (uint) {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::57 => function onDeposit() public {
2022-10-inverse/src/escrows/INVEscrow.sol::44 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/src/escrows/INVEscrow.sol::59 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/escrows/INVEscrow.sol::70 => function balance() public view returns (uint) {
2022-10-inverse/src/escrows/INVEscrow.sol::79 => function onDeposit() public {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::25 => function initialize(IERC20 _token, address) public {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::36 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::45 => function balance() public view returns (uint) {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::50 => function onDeposit() public {
```

## [G-14] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
2022-10-inverse/src/DBR.sol::19 => mapping(address => uint256) public balances;
2022-10-inverse/src/DBR.sol::20 => mapping(address => mapping(address => uint256)) public allowance;
2022-10-inverse/src/DBR.sol::23 => mapping(address => uint256) public nonces;
```

## [G-15] Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

instances:

```
2022-10-inverse/src/DBR.sol::249 => require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
2022-10-inverse/src/Market.sol::448 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
2022-10-inverse/src/Market.sol::512 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

## [G-16] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-10-inverse/src/DBR.sol::372 => function _burn(address from, uint256 amount) internal virtual {
2022-10-inverse/src/Market.sol::226 => function createEscrow(address user) internal returns (IEscrow instance) {
2022-10-inverse/src/Market.sol::353 => function getWithdrawalLimitInternal(address user) internal returns (uint) {
```

## [G-17] State variables only set in the constructor should be declared immutable

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

```
2022-10-inverse/src/BorrowController.sol::18 => require(msg.sender == operator, "Only operator");
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/DBR.sol::13 => uint8 public constant decimals = 18;
2022-10-inverse/src/DBR.sol::37 => name = _name;
2022-10-inverse/src/DBR.sol::38 => symbol = _symbol;
2022-10-inverse/src/DBR.sol::45 => require(msg.sender == operator, "ONLY OPERATOR");
```