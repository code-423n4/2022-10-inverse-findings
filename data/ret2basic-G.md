# Inverse Finance Contest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code audit:

1. Long `require()`/`revert()` string (4 instances)
2. Using `bool`s for storage incurs overhead (3 instances)
3. Use `!= 0` instead of `> 0` when comparing uint (19 instances)
4. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (8 instances)
5. Use `abi.encodePacked()` instead of `abi.encode()` (5 instances)
6. Use `private` instead of `public` for constants (1 instance)
7. Use custom errors instead of `revert()`/`require()` strings (60 instances)

Total 100 instances of 7 issues.

## 1. Long `require()`/`revert()` string (4 instances)

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

```solidity
src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

src/DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");

src/Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

src/Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

## 2. Using `bool`s for storage incurs overhead (3 instances)

Use `uint256(1)` and `uint256(2)` for true/false. Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

```solidity
src/Market.sol::52 => bool immutable callOnDepositCallback;

src/Market.sol::53 => bool public borrowPaused;

src/Market.sol::72 => bool _callOnDepositCallback
```

## 3. Use `!= 0` instead of `> 0` when comparing uint (19 instances)

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

```solidity
src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

src/DBR.sol::328 => require(deficit > 0, "No deficit");

src/Fed.sol::133 => if(profit > 0) {

src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

src/Market.sol::561 => require(deficit > 0, "No DBR deficit");

src/Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");

src/Market.sol::605 => if(liquidationFeeBps > 0) {

src/Oracle.sol::79 => if(fixedPrices[token] > 0) return fixedPrices[token];

src/Oracle.sol::83 => require(price > 0, "Invalid feed price");

src/Oracle.sol::96 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

src/Oracle.sol::97 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

src/Oracle.sol::113 => if(fixedPrices[token] > 0) return fixedPrices[token];

src/Oracle.sol::117 => require(price > 0, "Invalid feed price");

src/Oracle.sol::135 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

src/Oracle.sol::136 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
```

## 4. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (8 instances)

Instead of using operator && on single require check, using double `require()` checks can save more gas.

```solidity
src/DBR.sol::249 => require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

src/Market.sol::448 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

src/Market.sol::512 => require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

## 5. Use `abi.encodePacked()` instead of `abi.encode()` (5 instances)

`abi.encodePacked()` is more efficient than `abi.encode()`.

```solidity
src/DBR.sol::232 => abi.encode(

src/DBR.sol::269 => abi.encode(

src/Market.sol::104 => abi.encode(

src/Market.sol::431 => abi.encode(

src/Market.sol::495 => abi.encode(
```

## 6. Use `private` instead of `public` for constants (1 instance)

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

```solidity
src/DBR.sol::13 => uint8 public constant decimals = 18;
```

## 7. Use custom errors instead of `revert()`/`require()` strings (60 instances)

Using `require()`/`revert()` strings is expensive. Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors.

Custom errors decrease both deploy and runtime gas costs. Note that runtime gas cost is only relevant when the revert condition is met.

```solidity
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
```
