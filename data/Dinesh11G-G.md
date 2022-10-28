### Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
2022-10-inverse/src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
2022-10-inverse/src/DBR.sol::328 => require(deficit > 0, "No deficit");
2022-10-inverse/src/Fed.sol::133 => if(profit > 0) {
2022-10-inverse/src/Market.sol::75 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::162 => require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
2022-10-inverse/src/Market.sol::173 => require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
2022-10-inverse/src/Market.sol::184 => require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
2022-10-inverse/src/Market.sol::195 => require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
2022-10-inverse/src/Market.sol::561 => require(deficit > 0, "No DBR deficit");
2022-10-inverse/src/Market.sol::592 => require(repaidDebt > 0, "Must repay positive debt");
2022-10-inverse/src/Market.sol::605 => if(liquidationFeeBps > 0) {
2022-10-inverse/src/Oracle.sol::79 => if(fixedPrices[token] > 0) return fixedPrices[token];
2022-10-inverse/src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::96 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
2022-10-inverse/src/Oracle.sol::97 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
2022-10-inverse/src/Oracle.sol::113 => if(fixedPrices[token] > 0) return fixedPrices[token];
2022-10-inverse/src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::135 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
2022-10-inverse/src/Oracle.sol::136 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
2022-10-inverse/src/escrows/INVEscrow.sol::81 => if(invBalance > 0) {
```
#### Tools used
Manual





-----------------------------------------------------------------------------------------------------------------------------------------------------------------



### Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: Only valid for solidity versions <0.6.12 ⚡️

Access roles marked as constant results in computing the keccak256 operation each time the variable is used because assigned operations for constant variables are re-evaluated every time.

Changing the variables to immutable results in computing the hash only once on deployment, leading to gas savings.

Example
🤦 Bad:

bytes32 public constant GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");
🚀 Good:

bytes32 public immutable GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");

#### Findings:
```

2022-10-inverse/src/DBR.sol::227 => keccak256(
2022-10-inverse/src/DBR.sol::231 => keccak256(
2022-10-inverse/src/DBR.sol::233 => keccak256(
2022-10-inverse/src/DBR.sol::268 => keccak256(
2022-10-inverse/src/DBR.sol::270 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
2022-10-inverse/src/DBR.sol::271 => keccak256(bytes(name)),
2022-10-inverse/src/DBR.sol::272 => keccak256("1"),
2022-10-inverse/src/Market.sol::103 => keccak256(
2022-10-inverse/src/Market.sol::105 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
2022-10-inverse/src/Market.sol::106 => keccak256(bytes("DBR MARKET")),
2022-10-inverse/src/Market.sol::107 => keccak256("1"),
2022-10-inverse/src/Market.sol::303 => mstore(add(ptr, 0x6c), keccak256(ptr, 0x37))
2022-10-inverse/src/Market.sol::304 => predicted := keccak256(add(ptr, 0x37), 0x55)
2022-10-inverse/src/Market.sol::426 => keccak256(
2022-10-inverse/src/Market.sol::430 => keccak256(
2022-10-inverse/src/Market.sol::432 => keccak256(
2022-10-inverse/src/Market.sol::490 => keccak256(
2022-10-inverse/src/Market.sol::494 => keccak256(
2022-10-inverse/src/Market.sol::496 => keccak256(
```
#### Tools used
Manual




-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### Long Revert Strings

#### Impact
Issue Information: Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Example
🤦 Bad:

require(condition, "UniswapV3: The reentrancy guard. A transaction cannot re-enter the pool mid-swap");
🚀 Good (with shorter string):

// TODO: Provide link to a reference of error codes
require(condition, "LOK");
#### Findings:
```
2022-10-inverse/src/DBR.sol::63 => require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
2022-10-inverse/src/DBR.sol::234 => "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
2022-10-inverse/src/DBR.sol::270 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
2022-10-inverse/src/DBR.sol::326 => require(markets[msg.sender], "Only markets can call onForceReplenish");
2022-10-inverse/src/Market.sol::76 => require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
2022-10-inverse/src/Market.sol::105 => keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
2022-10-inverse/src/Market.sol::214 => require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
2022-10-inverse/src/Market.sol::433 => "BorrowOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
2022-10-inverse/src/Market.sol::497 => "WithdrawOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
```
#### Tools used
Manual

-----------------------------------------------------------------------------------------------------------------------------------------------------------------


### Improper liquidationFactorBps was set

### Impact:
It leads to an increase in the Gas value when <= 10000 is used in https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162

### Remidation

It is fixed by using < 10000 which lowers the Gas fee

### Findings
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162


### Tools:
Manual