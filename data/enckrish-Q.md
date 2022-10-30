# Low Severity
## Should use two-step transfer process for `BorrowController.operator`
Accidently transferring operator rights to an unowned address, will freeze the `allowList` in its then current state. Use a two step process as in `DBR`.

## Incorrect combination of `Market.escrowImplementation` and `Market.callOnDepositCallback` can cause deposits to revert
If `callOnDepositCallback` is set to true, then `escrow.onDeposit()` will be called, but only `INVEscrow` implements this function and if the implementation points to any other escrow, then `deposit` will revert.

# Non-Critical
## Missing events for state changes in `BorrowController.allow` and `BorrowController.deny`

## `DBR.addMinter` and `DBR.removeMinter` can emit incorrect event in some cases
`addMinter` can emit `AddMinter` event even if the address is already a minter. Same for `removeMinter`, which emits `RemoveMinter` for an address, even if it wasn't a minter before.

## `DBR.transferFrom` doesn't return useful error for insufficient allowance
In case, allowance is less than the amount to transfer, `DBR.transferFrom` reverts due to underflow error. An end-user can not determine the reason without requiring substantial effort.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L194

## `Market.dola` can be marked as `constant` since it is not assigned to in the constructor

