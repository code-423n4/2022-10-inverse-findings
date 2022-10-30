## 1. Use `!= 0` instead of `> 0`
```solidity
escrows/INVEscrow.sol:81:        if(invBalance > 0) {
Oracle.sol:79:        if(fixedPrices[token] > 0) return fixedPrices[token];
Oracle.sol:83:            require(price > 0, "Invalid feed price");
Oracle.sol:96:            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
Oracle.sol:97:            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
Oracle.sol:113:        if(fixedPrices[token] > 0) return fixedPrices[token];
Oracle.sol:117:            require(price > 0, "Invalid feed price");
Oracle.sol:135:            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
Oracle.sol:136:            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
Fed.sol:133:        if(profit > 0) {
Market.sol:75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
Market.sol:162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol:173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
Market.sol:184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
Market.sol:195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
Market.sol:561:        require(deficit > 0, "No DBR deficit");
Market.sol:592:        require(repaidDebt > 0, "Must repay positive debt");
Market.sol:605:        if(liquidationFeeBps > 0) {
DBR.sol:63:        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
DBR.sol:328:        require(deficit > 0, "No deficit");
```

## 2. `<x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
Fed.sol:91:        supplies[market] += amount;
Fed.sol:92:        globalSupply += amount;
Fed.sol:110:        supplies[market] -= amount;
Fed.sol:111:        globalSupply -= amount;
Market.sol:395:        debts[borrower] += amount;
Market.sol:397:        totalDebt += amount;
Market.sol:534:        debts[user] -= amount;
Market.sol:535:        totalDebt -= amount;
Market.sol:565:        debts[user] += replenishmentCost;
Market.sol:568:        totalDebt += replenishmentCost;
Market.sol:598:        liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
Market.sol:599:        debts[user] -= repaidDebt;
Market.sol:600:        totalDebt -= repaidDebt;
DBR.sol:172:        balances[msg.sender] -= amount;
DBR.sol:174:            balances[to] += amount;
DBR.sol:196:        balances[from] -= amount;
DBR.sol:198:            balances[to] += amount;
DBR.sol:288:        dueTokensAccrued[user] += accrued;
DBR.sol:289:        totalDueTokensAccrued += accrued;
DBR.sol:304:        debts[user] += additionalDebt;
DBR.sol:316:        debts[user] -= repaidDebt;
DBR.sol:332:        debts[user] += replenishmentCost;
DBR.sol:360:        _totalSupply += amount;
DBR.sol:362:            balances[to] += amount;
DBR.sol:374:        balances[from] -= amount;
DBR.sol:376:            _totalSupply -= amount;
```

## 3. Use calldata instead of memory when used only for reading
```solidity
DBR.sol:32:        string memory _name,
DBR.sol:33:        string memory _symbol,
```

## 4. Use `variable == false|0` -> `!variable` or `variable ==  true` -> `variable`
```solidity
Oracle.sol:126:            if(todaysLow == 0 || normalizedPrice < todaysLow) {
Market.sol:356:        if(collateralBalance == 0) return 0;
Market.sol:358:        if(debt == 0) return collateralBalance;
Market.sol:359:        if(collateralFactorBps == 0) return 0;
Market.sol:373:        if(collateralBalance == 0) return 0;
Market.sol:375:        if(debt == 0) return collateralBalance;
Market.sol:376:        if(collateralFactorBps == 0) return 0;
Market.sol:580:        if (debt == 0) return 0;
DBR.sol:303:        require(deficitOf(user) == 0, "DBR Deficit");
DBR.sol:350:        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```


## 5. `10eX` use less gas then `10**X`
```solidity
Oracle.sol:88:            uint normalizedPrice = price * (10 ** decimals);
Oracle.sol:122:            uint normalizedPrice = price * (10 ** decimals);
```

## 6. ` >=` COSTS LESS GAS THAN `>`
The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde 
```solidity
escrows/INVEscrow.sol:81:        if(invBalance > 0) {
Oracle.sol:79:        if(fixedPrices[token] > 0) return fixedPrices[token];
Oracle.sol:83:            require(price > 0, "Invalid feed price");
Oracle.sol:96:            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
Oracle.sol:97:            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
Oracle.sol:113:        if(fixedPrices[token] > 0) return fixedPrices[token];
Oracle.sol:117:            require(price > 0, "Invalid feed price");
Oracle.sol:135:            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
Oracle.sol:136:            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
Fed.sol:133:        if(profit > 0) {
Market.sol:75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
Market.sol:162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol:173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
Market.sol:184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
Market.sol:195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
Market.sol:561:        require(deficit > 0, "No DBR deficit");
Market.sol:562:        require(deficit >= amount, "Amount > deficit");
Market.sol:592:        require(repaidDebt > 0, "Must repay positive debt");
Market.sol:605:        if(liquidationFeeBps > 0) {
DBR.sol:63:        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
DBR.sol:110:        if(totalDueTokensAccrued > _totalSupply) return 0;
DBR.sol:123:        if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
DBR.sol:328:        require(deficit > 0, "No deficit");
DBR.sol:329:        require(deficit >= amount, "Amount > deficit");
```

## 7. Use `abi.encodePacked` for gas optimization 
Changing the abi.encode function to abi.encodePacked  can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient.
```solidity
Market.sol:104:                abi.encode(
Market.sol:431:                            abi.encode(
Market.sol:495:                            abi.encode(
DBR.sol:232:                            abi.encode(
DBR.sol:269:                abi.encode(
```

## 8. Use `private` for `constants`
```solidity
DBR.sol:13:    uint8 public constant decimals = 18;
```

## 9. Multiple address mappings can be combined into a single mapping of an `address` to a `struct`, where appropriate
check mannulay and only use of addresses is used as a key

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
```solidity
Market.sol:59:    mapping(address => uint256) public nonces; // user => nonce
DBR.sol:19:    mapping(address => uint256) public balances;
DBR.sol:20:    mapping(address => mapping(address => uint256)) public allowance;
DBR.sol:23:    mapping(address => uint256) public nonces;
BorrowController.sol:11:    mapping(address => bool) public contractAllowlist;
```

## 10. Use custom errors to save deployment and runtime costs in case of revert
Instead of using strings for error messages (e.g., require(msg.sender == owner, “unauthorized”)), you can use custom errors to reduce both deployment and runtime gas costs. In addition, they are very convenient as you can easily pass dynamic information to them.
```solidity
escrows/GovTokenEscrow.sol:31:        require(market == address(0), "ALREADY INITIALIZED");
escrows/GovTokenEscrow.sol:44:        require(msg.sender == market, "ONLY MARKET");
escrows/GovTokenEscrow.sol:67:        require(msg.sender == beneficiary);
escrows/SimpleERC20Escrow.sol:26:        require(market == address(0), "ALREADY INITIALIZED");
escrows/SimpleERC20Escrow.sol:37:        require(msg.sender == market, "ONLY MARKET");
escrows/INVEscrow.sol:45:        require(market == address(0), "ALREADY INITIALIZED");
escrows/INVEscrow.sol:60:        require(msg.sender == market, "ONLY MARKET");
escrows/INVEscrow.sol:91:        require(msg.sender == beneficiary);
Oracle.sol:36:        require(msg.sender == operator, "ONLY OPERATOR");
Oracle.sol:67:        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
Oracle.sol:83:            require(price > 0, "Invalid feed price");
Oracle.sol:117:            require(price > 0, "Invalid feed price");
Fed.sol:49:        require(msg.sender == gov, "ONLY GOV");
Fed.sol:58:        require(msg.sender == gov, "ONLY GOV");
Fed.sol:67:        require(msg.sender == gov, "ONLY GOV");
Fed.sol:76:        require(msg.sender == chair, "ONLY CHAIR");
Fed.sol:87:        require(msg.sender == chair, "ONLY CHAIR");
Fed.sol:88:        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
Fed.sol:89:        require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
Fed.sol:93:        require(globalSupply <= supplyCeiling);
Fed.sol:104:        require(msg.sender == chair, "ONLY CHAIR");
Fed.sol:105:        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
Fed.sol:107:        require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
Market.sol:74:        require(_collateralFactorBps < 10000, "Invalid collateral factor");
Market.sol:75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
Market.sol:76:        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
Market.sol:93:        require(msg.sender == gov, "Only gov can call this function");
Market.sol:150:        require(_collateralFactorBps < 10000, "Invalid collateral factor");
Market.sol:162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol:173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
Market.sol:184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
Market.sol:195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
Market.sol:204:        require(msg.sender == lender, "Only lender can recall");
Market.sol:214:            require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
Market.sol:216:            require(msg.sender == gov, "Only governance can unpause");
Market.sol:236:        require(instance != IEscrow(address(0)), "ERC1167: create2 failed");
Market.sol:390:        require(!borrowPaused, "Borrowing is paused");
Market.sol:392:            require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
Market.sol:396:        require(credit >= debts[borrower], "Exceeded credit limit");
Market.sol:423:        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
Market.sol:448:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
Market.sol:462:        require(limit >= amount, "Insufficient withdrawal limit");
Market.sol:487:        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
Market.sol:512:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
Market.sol:533:        require(debt >= amount, "Insufficient debt");
Market.sol:561:        require(deficit > 0, "No DBR deficit");
Market.sol:562:        require(deficit >= amount, "Amount > deficit");
Market.sol:567:        require(collateralValue >= debts[user], "Exceeded collateral value");
Market.sol:592:        require(repaidDebt > 0, "Must repay positive debt");
Market.sol:594:        require(getCreditLimitInternal(user) < debt, "User debt is healthy");
Market.sol:595:        require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
DBR.sol:45:        require(msg.sender == operator, "ONLY OPERATOR");
DBR.sol:63:        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
DBR.sol:71:        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
DBR.sol:171:        require(balanceOf(msg.sender) >= amount, "Insufficient balance");
DBR.sol:195:        require(balanceOf(from) >= amount, "Insufficient balance");
DBR.sol:224:        require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
DBR.sol:249:            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
DBR.sol:301:        require(markets[msg.sender], "Only markets can call onBorrow");
DBR.sol:303:        require(deficitOf(user) == 0, "DBR Deficit");
DBR.sol:314:        require(markets[msg.sender], "Only markets can call onRepay");
DBR.sol:326:        require(markets[msg.sender], "Only markets can call onForceReplenish");
DBR.sol:328:        require(deficit > 0, "No deficit");
DBR.sol:329:        require(deficit >= amount, "Amount > deficit");
DBR.sol:350:        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
DBR.sol:373:        require(balanceOf(from) >= amount, "Insufficient balance");
BorrowController.sol:18:        require(msg.sender == operator, "Only operator");
```
