## [G001] >= costs less gas than >

he compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas

#### Instances:
```
src/Fed.sol::133 => if(profit > 0) {
src/Market.sol::582 => if(credit >= debt) return 0;
src/Market.sol::605 => if(liquidationFeeBps > 0) {
src/Oracle.sol::79 => if(fixedPrices[token] > 0) return fixedPrices[token];
src/Oracle.sol::97 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
src/Oracle.sol::113 => if(fixedPrices[token] > 0) return fixedPrices[token];
src/Oracle.sol::136 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
src/escrows/INVEscrow.sol::81 => if(invBalance > 0) {
```
## [G002] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

#### Instances:
```
src/DBR.sol::172 => balances[msg.sender] -= amount;
src/DBR.sol::174 => balances[to] += amount;
src/DBR.sol::196 => balances[from] -= amount;
src/DBR.sol::198 => balances[to] += amount;
src/DBR.sol::288 => dueTokensAccrued[user] += accrued;
src/DBR.sol::289 => totalDueTokensAccrued += accrued;
src/DBR.sol::304 => debts[user] += additionalDebt;
src/DBR.sol::316 => debts[user] -= repaidDebt;
src/DBR.sol::332 => debts[user] += replenishmentCost;
src/DBR.sol::360 => _totalSupply += amount;
src/DBR.sol::362 => balances[to] += amount;
src/DBR.sol::374 => balances[from] -= amount;
src/DBR.sol::376 => _totalSupply -= amount;
src/Fed.sol::91 => supplies[market] += amount;
src/Fed.sol::92 => globalSupply += amount;
src/Fed.sol::110 => supplies[market] -= amount;
src/Fed.sol::111 => globalSupply -= amount;
src/Market.sol::395 => debts[borrower] += amount;
src/Market.sol::397 => totalDebt += amount;
src/Market.sol::534 => debts[user] -= amount;
src/Market.sol::535 => totalDebt -= amount;
src/Market.sol::565 => debts[user] += replenishmentCost;
src/Market.sol::568 => totalDebt += replenishmentCost;
src/Market.sol::598 => liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
src/Market.sol::599 => debts[user] -= repaidDebt;
src/Market.sol::600 => totalDebt -= repaidDebt;
```
## [G003] Using `bools` for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.Refer https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 . Use uint256(1) and uint256(2) for true/false

#### Instances:
```
src/BorrowController.sol::11 => mapping(address => bool) public contractAllowlist;
src/DBR.sol::24 => mapping (address => bool) public minters;
src/DBR.sol::25 => mapping (address => bool) public markets;
src/Market.sol::52 => bool immutable callOnDepositCallback;
src/Market.sol::53 => bool public borrowPaused;
```

## [G004] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement

This change saves 6 gas per instance

#### Instances:
```
src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
src/DBR.sol::328 => require(deficit > 0, "No deficit");
src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
src/Market.sol::561 => require(deficit > 0, "No DBR deficit");
src/Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");
src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
```
## [G005] Splitting `require()` statements that use `&&` saves gas

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

#### Instances:
```
src/DBR.sol::249 => require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
src/Market.sol::448 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
src/Market.sol::512 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
## [G006] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.\[layout_in_storage.html]{https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html}\Use a larger size then downcast where needed

#### Instances:
```
src/DBR.sol::13 => uint8 public constant decimals = 18;
src/Oracle.sol::85 => uint8 feedDecimals = feeds[token].feed.decimals();
src/Oracle.sol::86 => uint8 tokenDecimals = feeds[token].tokenDecimals;
src/Oracle.sol::87 => uint8 decimals = 36 - feedDecimals - tokenDecimals;
src/Oracle.sol::119 => uint8 feedDecimals = feeds[token].feed.decimals();
src/Oracle.sol::120 => uint8 tokenDecimals = feeds[token].tokenDecimals;
src/Oracle.sol::121 => uint8 decimals = 36 - feedDecimals - tokenDecimals;
```
## [G007] abi.encode() is less efficient than abi.encodePacked()

#### Instances:
```
src/DBR.sol::232 => abi.encode(
src/DBR.sol::269 => abi.encode(
src/Market.sol::104 => abi.encode(
src/Market.sol::431 => abi.encode(
src/Market.sol::495 => abi.encode(
```
## [G008] Don't compare boolean expressions to boolean literals

`if (<x> == true)` => `if (<x>)`, `if (<x> == false)` => `if (!<x>)`

#### Instances:
```
src/DBR.sol::350 => require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

## [G009] Functions with access control cheaper if payable

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

#### Instances:
```
codearena/2022-10-inverse/src/Market.sol::149 => function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
src/Market.sol::161 => function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
src/Market.sol::172 => function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
src/Market.sol::183 => function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
src/Market.sol::194 => function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```
## [G010] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version

#### Instances:
```
src/BorrowController.sol::18 => require(msg.sender == operator, "Only operator");
src/DBR.sol::45 => require(msg.sender == operator, "ONLY OPERATOR");
src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
src/DBR.sol::71 => require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
src/DBR.sol::171 => require(balanceOf(msg.sender) >= amount, "Insufficient balance");
src/DBR.sol::195 => require(balanceOf(from) >= amount, "Insufficient balance");
src/DBR.sol::224 => require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
src/DBR.sol::249 => require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
src/DBR.sol::301 => require(markets[msg.sender], "Only markets can call onBorrow");
src/DBR.sol::303 => require(deficitOf(user) == 0, "DBR Deficit");
src/DBR.sol::314 => require(markets[msg.sender], "Only markets can call onRepay");
src/DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");
src/DBR.sol::328 => require(deficit > 0, "No deficit");
src/DBR.sol::329 => require(deficit >= amount, "Amount > deficit");
src/DBR.sol::350 => require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
src/DBR.sol::373 => require(balanceOf(from) >= amount, "Insufficient balance");
src/Fed.sol::49 => require(msg.sender == gov, "ONLY GOV");
src/Fed.sol::58 => require(msg.sender == gov, "ONLY GOV");
src/Fed.sol::67 => require(msg.sender == gov, "ONLY GOV");
src/Fed.sol::76 => require(msg.sender == chair, "ONLY CHAIR");
src/Fed.sol::87 => require(msg.sender == chair, "ONLY CHAIR");
src/Fed.sol::88 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
src/Fed.sol::89 => require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
src/Fed.sol::104 => require(msg.sender == chair, "ONLY CHAIR");
src/Fed.sol::105 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
src/Fed.sol::107 => require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
src/Market.sol::74 => require(_collateralFactorBps < 10000, "Invalid collateral factor");
src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
src/Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
src/Market.sol::93 => require(msg.sender == gov, "Only gov can call this function");
src/Market.sol::150 => require(_collateralFactorBps < 10000, "Invalid collateral factor");
src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
src/Market.sol::204 => require(msg.sender == lender, "Only lender can recall");
src/Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
src/Market.sol::216 => require(msg.sender == gov, "Only governance can unpause");
src/Market.sol::236 => require(instance != IEscrow(address(0)), "ERC1167: create2 failed");
src/Market.sol::390 => require(!borrowPaused, "Borrowing is paused");
src/Market.sol::392 => require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
src/Market.sol::396 => require(credit >= debts[borrower], "Exceeded credit limit");
src/Market.sol::423 => require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
src/Market.sol::448 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
src/Market.sol::462 => require(limit >= amount, "Insufficient withdrawal limit");
src/Market.sol::487 => require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
src/Market.sol::512 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
src/Market.sol::533 => require(debt >= amount, "Insufficient debt");
src/Market.sol::561 => require(deficit > 0, "No DBR deficit");
src/Market.sol::562 => require(deficit >= amount, "Amount > deficit");
src/Market.sol::567 => require(collateralValue >= debts[user], "Exceeded collateral value");
src/Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");
src/Market.sol::594 => require(getCreditLimitInternal(user) < debt, "User debt is healthy");
src/Market.sol::595 => require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
src/Oracle.sol::36 => require(msg.sender == operator, "ONLY OPERATOR");
src/Oracle.sol::67 => require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
src/Oracle.sol::104 => revert("Price not found");
src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
src/Oracle.sol::143 => revert("Price not found");
src/escrows/GovTokenEscrow.sol::31 => require(market == address(0), "ALREADY INITIALIZED");
src/escrows/GovTokenEscrow.sol::44 => require(msg.sender == market, "ONLY MARKET");
src/escrows/INVEscrow.sol::45 => require(market == address(0), "ALREADY INITIALIZED");
src/escrows/INVEscrow.sol::60 => require(msg.sender == market, "ONLY MARKET");
src/escrows/SimpleERC20Escrow.sol::26 => require(market == address(0), "ALREADY INITIALIZED");
src/escrows/SimpleERC20Escrow.sol::37 => require(msg.sender == market, "ONLY MARKET");
```
