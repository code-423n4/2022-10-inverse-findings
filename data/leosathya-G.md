### [G-01] Use of custom errors are more efficient than require()/revert() with error message 

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

There are Multiple  instances of this issue:

#### Recommended Mitigation
require() with long error message length more than 32bytes can replace with custom error message to save gas.




### [G-02] Split require() statement that use && operator can help in save gas

There are 6 instances of this issue:
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448



### [G-03] ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
There is 4 instance of this issue:
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L495
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L431
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L232
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L269



### [G-04] Function that guaranteed to revert when called by normal user can be marked payable 

If a function modifier such as onlyOwner, onlyMarket, onlyGov and etc is used, the function will revert if a normal user tries to call the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 

There are 29 instances of this issue:
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
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L66
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59
> ** File :  src/escrows/SimpleERC20Escrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36




### [G-05] Some public function could be external for saving gas

There are 63 instances of this issue:
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
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L258
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L312
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L520
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L120
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L120
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L133
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L192
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L223
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L262
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L284
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61
> ** File :  src/Oracle.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L66
> ** File :  src/escrows/GovTokenEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L52
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L70
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L79
> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L90
> ** File :  src/escrows/SimpleERC20Escrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36
> ** File :  src/escrows/SimpleERC20Escrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L45







### [G-06] Variable should be cached to memory and then used further

State Variable "collateralFactorBps" should be cached in function getWithdrawalLimit()
> ** File :  src/Market.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L370-L380



### [G-07] <x> += <y> costs more gas than <x> = <x> + <y> for state variables 

There are 17 instances of this issue:

> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L535
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598-L600
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91-L92
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110-L111
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288-L289
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332




### [G-08] Unchecking those arithmetic operation which can't overflow or underflow can save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block: https://docs.soliditylang.org/en/v0.8.10/control-structures.html#checked-or-unchecked-arithmetic 

There are 4 instances of this issue:

> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L438
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L
> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L521
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L259




### [G-09] > 0 IS LESS EFFICIENT THAN != 0 FOR UNSIGNED INTEGERS (WITH PROOF)

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

There are 4 instances of this issue:

> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L561
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L133
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L328


### [G-10] Same require() statement used multiple times

When same require() condition is repeatedly used in multiple functions, it will more efficient to enclose that require() inside a modifier and use it with functions, This help in deployment gas saving

require(msg.sender == gov, "ONLY GOV"); used multiple times as following
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L58
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67

require(msg.sender == chair, "ONLY CHAIR"); used multiple times as following
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87
> ** File :  src/Fed.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104


### [G-11] Using Bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the  slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

There are 3 instances of this issue:
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L24
> ** File :  src/DBR.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L25
> ** File :  src/BorrowController.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L11

#### Recommended Mitigation

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


### [G-12] Divide before Multiply cause precision loss

uint invBalanceInXInv = xINV.balanceOf(address(this)) * xINV.exchangeRateStored() / 1 ether;

> ** File :  src/escrows/INVEscrow.sol**  => https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L72

#### Recommended Mitigation
should be
uint invBalanceInXInv = (xINV.balanceOf(address(this)) * xINV.exchangeRateStored()) / 1 ether;



### [G-13] State Variable that can declare as CONSTANT 

> ** File :  src/Market.sol**  =>  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L44