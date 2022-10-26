### [L-01] ERC20 transfer() AND transferFrom() FUNCTIONS USED FOR TOKEN TRANSFER AND ITS RETURN VALUE IS IGNORED

Not all IERC20 implementations revert() when there’s a failure in   transfer()/transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

There are 9 instances of this issue:
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L205
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L280
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L399
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L537
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L570
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L602
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L135
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L63
> ** File :  src/escrows/SimpleERC20Escrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L38

#### Recommended Mitigation
There atleast should be require() condition to check return value of transfer()/transferFrom() function.
Or, use safeERC20 library from Openzeppelin. Use safeTransfer()/safeTransferFrom() or at least implement a return value check for ERC20 transfer() function



### [L-02] FLOATING PRAGMA IS USED

There are 8 instances of this issue:
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L02
> ** File :  src/Fed.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L02
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L02
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L02
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L02
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L02
> ** File :  src/escrows/SimpleERC20Escrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L02
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L02

#### Recommended Mitigation
its recommended to lock pragma. Contract should deploy with that compiler version with whom it was tested



### [L-03] ABSENCE OF ZERO ADDRESS CHECK FOR RECEIVER ADDRESS IN transfer() and transferFrom() FUNCTION

Due to absence of zero address check for receiver address in transfer() and transferFrom() functions, this may lead to token loss.
As there are special functions present for token burning in contract, so i consider this as a small bug.

There are 2 instances of this issue:
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188-202
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170-178

#### Recommended Mitigation
There should be require() that validate a perticular address is zero address or not



### [L-04] Absence of Zero address check for critical state variable addresses

There are 7 instances of this issue:
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L80
> ** File :  src/Fed.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39-L40
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L34
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L48


#### Recommended Mitigation
There should be require() that validate a perticular address is zero address or not


### [L-05] CONSIDER ADDINGS CHECKS FOR SIGNATURE MALLEABILITY

The ecrecover() function returns an address of zero when the signature does not match. This can cause problems if address zero is ever the owner of assets, and someone uses the permit function on address zero. If that happens, any invalid signature will pass the checks, and the assets will be stealable. In this case, the asset of concern is the vault’s ERC20 token, and fortunately OpenZeppelin’s implementation does a good job of making sure that address zero is never able to have a positive balance. If this contract ever changes to another ERC20 implementation that is laxer in its checks in favor of saving gas, this code may become a problem.

There are 4 instances of this issue:
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L104
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L425
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L489
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L226

#### Recommended Mitigation
Use OpenZeppelin’s ECDSA contract rather than calling ecrecover() directly


### [L-06] MISSING EVENTS FOR ONLY FUNCTIONS THAT CHANGE CRITICAL PARAMETERS

The functions that change critical parameters should emit events. Events allow capturing the changed parameters so that off-chain tools/interfaces can register such changes with timelocks that allow users to evaluate them and consider if they would like to engage/exit based on how they perceive the changes as affecting the trustworthiness of the protocol or profitability of the implemented financial services. The alternative of directly querying on-chain contract state for such changes is not considered practical for most users/usages.

There are multiple instances of this issue:
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194

#### Recommended Mitigation
Recommended to emit events on critical variable change



### [N-01] Some require statements don't have any error message

There are 1 instances of this issue:
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93
  
#### Recommended Mitigation
There should be a error message which signifies revert reason.

