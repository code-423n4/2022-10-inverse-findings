# c4udit Report

## Files analyzed
- BorrowController.sol
- DBR.sol
- Fed.sol
- Market.sol
- Oracle.sol
- escrows/GovTokenEscrow.sol
- escrows/INVEscrow.sol
- escrows/SimpleERC20Escrow.sol

## Issues found

### Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: [G003](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g003---use--0-instead-of--0-for-unsigned-integer-comparison)

#### Findings:
```
DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
DBR.sol::328 => require(deficit > 0, "No deficit");
Fed.sol::133 => if(profit > 0) {
Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
Market.sol::561 => require(deficit > 0, "No DBR deficit");
Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");
Market.sol::605 => if(liquidationFeeBps > 0) {
Oracle.sol::79 => if(fixedPrices[token] > 0) return fixedPrices[token];
Oracle.sol::83 => require(price > 0, "Invalid feed price");
Oracle.sol::96 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
Oracle.sol::97 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
Oracle.sol::113 => if(fixedPrices[token] > 0) return fixedPrices[token];
Oracle.sol::117 => require(price > 0, "Invalid feed price");
Oracle.sol::135 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
Oracle.sol::136 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
escrows/INVEscrow.sol::81 => if(invBalance > 0) {
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: [G006](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g006---use-immutable-for-openzeppelin-accesscontrols-roles-declarations)

#### Findings:
```
DBR.sol::227 => keccak256(
DBR.sol::231 => keccak256(
DBR.sol::233 => keccak256(
DBR.sol::268 => keccak256(
DBR.sol::270 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
DBR.sol::271 => keccak256(bytes(name)),
DBR.sol::272 => keccak256("1"),
Market.sol::103 => keccak256(
Market.sol::105 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
Market.sol::106 => keccak256(bytes("DBR MARKET")),
Market.sol::107 => keccak256("1"),
Market.sol::303 => mstore(add(ptr, 0x6c), keccak256(ptr, 0x37))
Market.sol::304 => predicted := keccak256(add(ptr, 0x37), 0x55)
Market.sol::426 => keccak256(
Market.sol::430 => keccak256(
Market.sol::432 => keccak256(
Market.sol::490 => keccak256(
Market.sol::494 => keccak256(
Market.sol::496 => keccak256(
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Long Revert Strings

#### Impact
Issue Information: [G007](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g007---long-revert-strings)

#### Findings:
```
DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
DBR.sol::234 => "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
DBR.sol::270 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");
Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
Market.sol::105 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
Market.sol::433 => "BorrowOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
Market.sol::497 => "WithdrawOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)