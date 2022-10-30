## 1. Open TODOs
```solidity
escrows/INVEscrow.sol:35:        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```

## 2. Use of `Block.timestamp`
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
```solidity
Oracle.sol:89:            uint day = block.timestamp / 1 days;
Oracle.sol:124:            uint day = block.timestamp / 1 days;
Market.sol:423:        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
Market.sol:487:        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
DBR.sol:122:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
DBR.sol:135:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
DBR.sol:148:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
DBR.sol:224:        require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
DBR.sol:286:        if(lastUpdated[user] == block.timestamp) return;
DBR.sol:287:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
DBR.sol:290:        lastUpdated[user] = block.timestamp;
```


## 3. `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
```solidity
Market.sol-423-        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
Market.sol-424-        unchecked {
Market.sol-425-            address recoveredAddress = ecrecover(
Market.sol-426-                keccak256(
Market.sol:427:                    abi.encodePacked(
Market.sol-428-                        "\x19\x01",
Market.sol-429-                        DOMAIN_SEPARATOR(),
Market.sol-430-                        keccak256(
Market.sol-431-                            abi.encode(
--
Market.sol-487-        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
Market.sol-488-        unchecked {
Market.sol-489-            address recoveredAddress = ecrecover(
Market.sol-490-                keccak256(
Market.sol:491:                    abi.encodePacked(
Market.sol-492-                        "\x19\x01",
Market.sol-493-                        DOMAIN_SEPARATOR(),
Market.sol-494-                        keccak256(
Market.sol-495-                            abi.encode(
--
DBR.sol-224-        require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
DBR.sol-225-        unchecked {
DBR.sol-226-            address recoveredAddress = ecrecover(
DBR.sol-227-                keccak256(
DBR.sol:228:                    abi.encodePacked(
DBR.sol-229-                        "\x19\x01",
DBR.sol-230-                        DOMAIN_SEPARATOR(),
DBR.sol-231-                        keccak256(
DBR.sol-232-                            abi.encode(
```

## 4. Use scientific notation (e.g. `10e18`) rather than exponentiation (e.g. `10**18`)
```solidity
Oracle.sol:88:            uint normalizedPrice = price * (10 ** decimals);
Oracle.sol:122:            uint normalizedPrice = price * (10 ** decimals);
```


## 5. `require()`/`revert()` statements should have descriptive reason strings
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

