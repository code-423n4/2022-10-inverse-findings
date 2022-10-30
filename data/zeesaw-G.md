## Issues found

### Use != 0 instead of > 0 for Unsigned Integer Comparison

```
src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
src/DBR.sol::328 => require(deficit > 0, "No deficit");
src/Fed.sol::133 => if(profit > 0) {
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
src/escrows/INVEscrow.sol::81 => if(invBalance > 0) {
```

### Long Revert Strings

```
src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
src/DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");
src/Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
src/Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```