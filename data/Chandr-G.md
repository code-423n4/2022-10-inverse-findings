# Require instead &&
### IMPACT
Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249
require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512
require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

### Mitigation:
Replace: 
require (A && B); 
With:
require(A);
require(B);

# PREFIX INCREMENTS
### IMPACT
Prefix increments are cheaper than postfix increments.

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L239
nonces[owner]++,

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L259
nonces[msg.sender]++;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L438
nonces[from]++,

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L502
nonces[from]++,

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L521
nonces[msg.sender]++;

### Mitigation:
replace foo++ to ++foo


# CUSTOM ERRORS
### IMPACT:
Custom errors from Solidity 0.8.4 are cheaper than require/revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information, as explained [here](https://blog.soliditylang.org/2021/04/21/custom-errors/)

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L45
require(msg.sender == operator, "ONLY OPERATOR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L71
require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L171
require(balanceOf(msg.sender) >= amount, "Insufficient balance");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195
require(balanceOf(from) >= amount, "Insufficient balance");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224
require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249
require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L301
require(markets[msg.sender], "Only markets can call onBorrow");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L303
require(deficitOf(user) == 0, "DBR Deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L314
require(markets[msg.sender], "Only markets can call onRepay");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326
require(markets[msg.sender], "Only markets can call onForceReplenish");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L328
require(deficit > 0, "No deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L329
require(deficit >= amount, "Amount > deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L350
require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L373
require(balanceOf(from) >= amount, "Insufficient balance");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L18
require(msg.sender == operator, "Only operator");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49
require(msg.sender == gov, "ONLY GOV");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L58
require(msg.sender == gov, "ONLY GOV");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67
require(msg.sender == gov, "ONLY GOV");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76
require(msg.sender == chair, "ONLY CHAIR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87
require(msg.sender == chair, "ONLY CHAIR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L88
require(dbr.markets(address(market)), "UNSUPPORTED MARKET");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L89
require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93
require(globalSupply <= supplyCeiling);

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104
require(msg.sender == chair, "ONLY CHAIR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L105
require(dbr.markets(address(market)), "UNSUPPORTED MARKET");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L107
require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L136
require(success, "Transfer failed.");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L36
require(msg.sender == operator, "ONLY OPERATOR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67
require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83
require(price > 0, "Invalid feed price");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L104
revert("Price not found");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L117
require(price > 0, "Invalid feed price");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L143
revert("Price not found");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74
require(_collateralFactorBps < 10000, "Invalid collateral factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76
require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L93
require(msg.sender == gov, "Only gov can call this function");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150
require(_collateralFactorBps < 10000, "Invalid collateral factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L204
require(msg.sender == lender, "Only lender can recall");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214
require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L216
require(msg.sender == gov, "Only governance can unpause");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L236
require(instance != IEscrow(address(0)), "ERC1167: create2 failed");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L390
require(!borrowPaused, "Borrowing is paused");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L392
require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L396
require(credit >= debts[borrower], "Exceeded credit limit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423
require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L462
require(limit >= amount, "Insufficient withdrawal limit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487
require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512
require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533
require(debt >= amount, "Insufficient debt");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L561
require(deficit > 0, "No DBR deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L562
require(deficit >= amount, "Amount > deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L567
require(collateralValue >= debts[user], "Exceeded collateral value");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L592
require(repaidDebt > 0, "Must repay positive debt");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L594
require(getCreditLimitInternal(user) < debt, "User debt is healthy");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595
require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

### Mitigation:
Replace require and revert statements with custom errors.


# COMPARISON OPERATORS
### IMPACT
In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =.
Using strict comparison operators hence saves gas.

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L171
require(balanceOf(msg.sender) >= amount, "Insufficient balance");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195
require(balanceOf(from) >= amount, "Insufficient balance");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224
require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L329
require(deficit >= amount, "Amount > deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L373
require(balanceOf(from) >= amount, "Insufficient balance");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93
require(globalSupply <= supplyCeiling);

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L107
require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L123
if(supply >= marketValue) return 0;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L361
if(collateralBalance <= minimumCollateral) return 0;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L378
if(collateralBalance <= minimumCollateral) return 0;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L396
require(credit >= debts[borrower], "Exceeded credit limit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423
require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L462
require(limit >= amount, "Insufficient withdrawal limit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487
require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533
require(debt >= amount, "Insufficient debt");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L562
require(deficit >= amount, "Amount > deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L567
require(collateralValue >= debts[user], "Exceeded collateral value");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L582
if(credit >= debt) return 0;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595
require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L607
if(escrow.balance() >= liquidationFee) {

### Mitigation:
Replace <= with <, and >= with >. Do not forget to increment/decrement the compared variable


# COMPARISON WITH ZERO
### IMPACT
>0 is less gas efficient than !0 if you enable the optimizer at 10k AND you’re in a require statement. Detailed explanation with the opcodes [here](https://twitter.com/gzeon/status/1485428085885640706)

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L328
require(deficit > 0, "No deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L133
if(profit > 0) {

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L79
if(fixedPrices[token] > 0) return fixedPrices[token];

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83
require(price > 0, "Invalid feed price");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L96
uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L97
if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L113
if(fixedPrices[token] > 0) return fixedPrices[token];

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L117
require(price > 0, "Invalid feed price");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L135
uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L136
if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation 
incentive");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L561
require(deficit > 0, "No DBR deficit");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L592
require(repaidDebt > 0, "Must repay positive debt");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605
if(liquidationFeeBps > 0) {

### Mitigation:
Replace >0 with !0


# Increment/decrement operations
### IMPACT
X = X + Y IS CHEAPER THAN X += Y
X = X- Y IS CHEAPER THAN X -= Y

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172
balances[msg.sender] -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174
balances[to] += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196
balances[from] -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198
balances[to] += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288
dueTokensAccrued[user] += accrued;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L289
totalDueTokensAccrued += accrued;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304
debts[user] += additionalDebt;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316
debts[user] -= repaidDebt;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332
debts[user] += replenishmentCost;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360
_totalSupply += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L362
balances[to] += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374
balances[from] -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L376
_totalSupply -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91
supplies[market] += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L92
globalSupply += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110
supplies[market] -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L111
globalSupply -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395
debts[borrower] += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397
totalDebt += amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534
debts[user] -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L535
totalDebt -= amount;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565
debts[user] += replenishmentCost;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568
totalDebt += replenishmentCost;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598
liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L599
debts[user] -= repaidDebt;

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L600
totalDebt -= repaidDebt;

# Mitigation:
X += Y replace with X = X + Y
X -= Y replace with X = X - Y


# LONG REQUIRE()/REVERT() 
### IMPACT
REQUIRE()/REVERT() Strings longer than 32 bytes cost extra gas

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326
require(markets[msg.sender], "Only markets can call onForceReplenish");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76
require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214
require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

# Mitigation:
Replace with more compact strings.

