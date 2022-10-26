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