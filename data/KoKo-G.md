# GAS ISSUES FOR [2022-10-INVERSE](https://github.com/code-423n4/2022-10-inverse)

## [G-01] There is no need to compare boolean variable to boolean literals

Description: if (var == true) => if (var)

./src/DBR.sol

```
L350:  require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

## [G-02] Re-arrange storage variable so they use less contract storage slots and use less gas this way

Description: Currently storage variables in Market.sol are packed in 15 slots, but could fit in 14

## [G-03] A < B + 1 is cheaper than A <= B

./src/DBR.sol

```
L171:  require(balanceOf(msg.sender) >= amount, "Insufficient balance");

L224:  require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

L329:  require(deficit >= amount, "Amount > deficit");

L373:  require(balanceOf(from) >= amount, "Insufficient balance");
```

./src/Market.sol

```
L162:  require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

L361:  if(collateralBalance <= minimumCollateral) return 0;

L396:  require(credit >= debts[borrower], "Exceeded credit limit");

L423:  require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

L462:  require(limit >= amount, "Insufficient withdrawal limit");

L487:  require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

L562:  require(deficit >= amount, "Amount > deficit");

L582:  if(credit >= debt) return 0;

L607:  if(escrow.balance() >= liquidationFee) {
```

./src/Fed.sol

```
L93:   require(globalSupply <= supplyCeiling);

L107:  require(amount <= supply, "AMOUNT TOO BIG");

L123:  if(supply >= marketValue) return 0;
```

## [G-04] Consider shortening some string to avoid extra gas

./src/DBR.sol

```
L63:   require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

L326:  require(markets[msg.sender], "Only markets can call onForceReplenish");
```

./src/Market.sol

```
L76:   require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

L214:  require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

## [G-05] `!= 0` comparison is cheaper than `> 0`

./src/Market.sol

```
L162:  require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

L173:  require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

L195:  require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

L561:  require(deficit > 0, "No DBR deficit");

L592:  require(repaidDebt > 0, "Must repay positive debt");
```

./src/Fed.sol

```
L133:  if(profit > 0) {
```

./src/Oracle.sol

```
L83:   require(price > 0, "Invalid feed price");

L97:   if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

L117:  require(price > 0, "Invalid feed price");

L135:  uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

L136:  if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
```

./src/escrows/INVEscrow.sol

```
L81:   if(invBalance > 0) {
```

./src/DBR.sol

```
L63:   require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

L328:  require(deficit > 0, "No deficit");
```

## [G-06] Use `uint265` for boolean values (e.g. true => 1 and false => 2)

./src/Market.sol

```
L52:   bool immutable callOnDepositCallback;

L53:   bool public borrowPaused;
```

## [G-07] Cache variables that are read multiple times in a single fuction or loop

./src/Market.sol

```
L560:  uint deficit = dbr.deficitOf(user);
L563:  uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
L569:  dbr.onForceReplenish(user, amount);
```

./src/Oracle.sol

```
L82:   uint price = feeds[token].feed.latestAnswer();
L85:   uint8 feedDecimals = feeds[token].feed.decimals();
L86:   uint8 tokenDecimals = feeds[token].tokenDecimals;

L116:  uint price = feeds[token].feed.latestAnswer();
L119:  uint8 feedDecimals = feeds[token].feed.decimals();
L120:  uint8 tokenDecimals = feeds[token].tokenDecimals;
```

## [G-08] If a function is not open to any user, it can be marked as `payable` to save gas

Description: Under the hood the compiler checks if a function is payable. The check is skipped if it is marked as such by the developer

./src/escrows/INVEscrow.sol

```
L59:   function pay(address recipient, uint amount) public {

L90:   function delegate(address delegatee) public {
```

./src/Oracle.sol

```
L66:   function claimOperator() public {
```

./src/escrows/SimpleERC20Escrow.sol

```
L36:   function pay(address recipient, uint amount) public {
```

./src/Market.sol

```
L203:  function recall(uint amount) public {

L212:  function pauseBorrows(bool _value) public {
```

./src/escrows/GovTokenEscrow.sol

```
L43:   function pay(address recipient, uint amount) public {

L66:   function delegate(address delegatee) public {
```

./src/Fed.sol

```
L48:   function changeGov(address _gov) public {

L66:   function changeChair(address _chair) public {

L75:   function resign() public {

L86:   function expansion(IMarket market, uint amount) public {

L103:  function contraction(IMarket market, uint amount) public {
```

./src/DBR.sol

```
L70:   function claimOperator() public {
```

## [G-09] Spread the `require()` conditions to multiple `require()` statements to save gas from `&&`

./src/DBR.sol

```
L249:  require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

./src/Market.sol

```
L75:   require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

L162:  require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

L173:  require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

L448:  require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

L512:  require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

## [G-10] Use custom errors to save gas

./src/escrows/SimpleERC20Escrow.sol

```
L26:   require(market == address(0), "ALREADY INITIALIZED");

L37:   require(msg.sender == market, "ONLY MARKET");
```

./src/Market.sol

```
L74:   require(_collateralFactorBps < 10000, "Invalid collateral factor");

L75:   require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

L76:   require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

L93:   require(msg.sender == gov, "Only gov can call this function");

L150:  require(_collateralFactorBps < 10000, "Invalid collateral factor");

L162:  require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

L173:  require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

L204:  require(msg.sender == lender, "Only lender can recall");

L214:  require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

L236:  require(instance != IEscrow(address(0)), "ERC1167: create2 failed");

L390:  require(!borrowPaused, "Borrowing is paused");

L396:  require(credit >= debts[borrower], "Exceeded credit limit");

L487:  require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

L512:  require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

L533:  require(debt >= amount, "Insufficient debt");

L561:  require(deficit > 0, "No DBR deficit");

L562:  require(deficit >= amount, "Amount > deficit");

L594:  require(getCreditLimitInternal(user) < debt, "User debt is healthy");

L595:  require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
```

./src/BorrowController.sol

```
L18:   require(msg.sender == operator, "Only operator");
```

./src/escrows/GovTokenEscrow.sol

```
L31:   require(market == address(0), "ALREADY INITIALIZED");

L44:   require(msg.sender == market, "ONLY MARKET");
```

./src/escrows/INVEscrow.sol

```
L45:   require(market == address(0), "ALREADY INITIALIZED");

L60:   require(msg.sender == market, "ONLY MARKET");
```

./src/Fed.sol

```
L58:   require(msg.sender == gov, "ONLY GOV");

L67:   require(msg.sender == gov, "ONLY GOV");

L76:   require(msg.sender == chair, "ONLY CHAIR");

L87:   require(msg.sender == chair, "ONLY CHAIR");

L88:   require(dbr.markets(address(market)), "UNSUPPORTED MARKET");

L89:   require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");

L107:  require(amount <= supply, "AMOUNT TOO BIG");
```

./src/Oracle.sol

```
L36:   require(msg.sender == operator, "ONLY OPERATOR");

L83:   require(price > 0, "Invalid feed price");

L104:  revert("Price not found");

L117:  require(price > 0, "Invalid feed price");

L143:  revert("Price not found");
```

./src/DBR.sol

```
L63:   require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

L71:   require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");

L171:  require(balanceOf(msg.sender) >= amount, "Insufficient balance");

L195:  require(balanceOf(from) >= amount, "Insufficient balance");

L249:  require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

L301:  require(markets[msg.sender], "Only markets can call onBorrow");

L303:  require(deficitOf(user) == 0, "DBR Deficit");

L314:  require(markets[msg.sender], "Only markets can call onRepay");

L328:  require(deficit > 0, "No deficit");

L329:  require(deficit >= amount, "Amount > deficit");

L373:  require(balanceOf(from) >= amount, "Insufficient balance");
```

## [G-11] A=A+B costs less gas than A+=B if A and B are located in storage

./src/Market.sol

```
L397:  totalDebt += amount;

L535:  totalDebt -= amount;

L600:  totalDebt -= repaidDebt;

L395:  debts[borrower] += amount;

L534:  debts[user] -= amount;

L599:  debts[user] -= repaidDebt;
```

./src/Fed.sol

```
L92:   globalSupply += amount;

L111:  globalSupply -= amount;

L110:  supplies[market] -= amount;
```

./src/DBR.sol

```
L360:  _totalSupply += amount;

L376:  _totalSupply -= amount;

L289:  totalDueTokensAccrued += accrued;

L172:  balances[msg.sender] -= amount;

L198:  balances[to] += amount;

L374:  balances[from] -= amount;

L304:  debts[user] += additionalDebt;

L316:  debts[user] -= repaidDebt;

L288:  dueTokensAccrued[user] += accrued;
```
