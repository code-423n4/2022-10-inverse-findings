## [NAZ-L1] Signature Malleability of EVM's `ecrecover()`
**Severity**: Low
**Context**: [`Market.sol#L425`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L425), [`Market.sol#L489`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L489), [`DBR.sol#L226`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L226)

**Description**:
The function calls the Solidity `ecrecover()` function directly to verify the given signatures. However, the `ecrecover()` EVM opcode allows malleable (non-unique) signatures and thus is susceptible to replay attacks.

Although a replay attack seems not possible here since the nonce is increased each time, ensuring the signatures are not malleable is considered a best practice.

**Recommendation**:
Use the ecrecover function from [OpenZeppelin's ECDSA library](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/cryptography/ECDSA.sol) for signature verification. (Ensure using a version `> 4.7.3` for there was a critical bug `>= 4.1.0 < 4.7.3`).


## [NAZ-L2] Lack of Event Emission For Critical Functions
**Severity**: Low
**Context**: [`Market.sol#L118`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118), [`Market.sol#L124`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124), [`Market.sol#L130`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130), [`Market.sol#L136`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136), [`Market.sol#L142`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142), [`Market.sol#L149`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149), [`Market.sol#L161`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161), [`Market.sol#L172`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172), [`Market.sol#L183`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183), [`Market.sol#L194`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194), [`DBR.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53), [`DBR.sol#L62`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62), [`BorrowController.sol#L26`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26), [`BorrowController.sol#L32`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32), [`BorrowController.sol#L38`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38), [`Oracle.sol#L44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44), [`Oracle.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53), [`Oracle.sol#L61`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61), [`Fed.sol#L48`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48), [`Fed.sol#L57`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57), [`Fed.sol#L66`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66), [`Fed.sol#L75`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75), [`SimpleERC20Escrow.sol#L36`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36), [`INVEscrow.sol#L59`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59), [`GovTokenEscrow.sol#L43`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43) 

**Description**:
Several functions update critical parameters that are missing event emission. These should be performed to ensure tracking of changes of such critical parameters.

**Recommendation**:
Consider adding events to functions that change critical parameters.


## [NAZ-L3] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`Market.sol#L118`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118), [`Market.sol#L124`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124), [`Market.sol#L130`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130), [`Market.sol#L136`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136), [`Market.sol#L142`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142), [`Market.sol#L149`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149), [`Market.sol#L161`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161), [`Market.sol#L172`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172), [`Market.sol#L183`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183), [`Market.sol#L194`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194), [`DBR.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53), [`DBR.sol#L62`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62), [`DBR.sol#L81`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81), [`DBR.sol#L90`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90), [`DBR.sol#L99`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99), [`BorrowController.sol#L26`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26), [`BorrowController.sol#L32`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32), [`BorrowController.sol#L38`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38), [`Oracle.sol#L44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44), [`Oracle.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53), [`Oracle.sol#L61`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61), [`Fed.sol#L48`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48), [`Fed.sol#L57`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57), [`Fed.sol#L66`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L4] Missing Zero-address Validation
**Severity**: Low
**Context**: [`Market.sol#L118`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118), [`Market.sol#L124`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124), [`Market.sol#L130`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130), [`Market.sol#L136`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136), [`Market.sol#L142`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142), [`DBR.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53), [`DBR.sol#L81`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81), [`DBR.sol#L90`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90), [`DBR.sol#L99`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99), [`BorrowController.sol#L26`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26), [`BorrowController.sol#L32`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32), [`BorrowController.sol#L38`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38), [`Oracle.sol#L44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44), [`Oracle.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53), [`Oracle.sol#L61`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61), [`Fed.sol#L48`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48), [`Fed.sol#L66`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66), [`Fed.sol#L75`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75), [`SimpleERC20Escrow.sol#L36`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36), [`INVEscrow.sol#L59`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59), [`GovTokenEscrow.sol#L43`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L5] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`SimpleERC20Escrow.sol#L25`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L25), [`INVEscrow.sol#L44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L44), [`GovTokenEscrow.sol#L30`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L30)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N1] Missing Visibility of State Variables
**Severity**: Informational
**Context**: [`Market.sol#L52`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L52)

**Description**:
Some state varaibles are missing visibility.

**Recommendation**:
Consider adding visibility to all state variables


## [NAZ-N2] Line Length
**Severity**: Informational
**Context**: [`Market.sol#L124`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124), [`Market.sol#L133`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L133), [`Market.sol#L145`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L145), [`Market.sol#L168`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L168), [`Market.sol#L173`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173), [`Market.sol#L179`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L179), [`Market.sol#L184`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184), [`Market.sol#L195`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195), [`Market.sol#L209`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L209), [`Market.sol#L350`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L350), [`Market.sol#L360`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360), [`Market.sol#L377`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377), [`Market.sol#L433`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L433), [`Market.sol#L497`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L497), [`Market.sol#L526`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L526), [`Market.sol#L555`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L555), [`Market.sol#L587`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L587), [`Market.sol#L618`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L618), [`DBR.sol#L50`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50), [`DBR.sol#L58`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L58), [`DBR.sol#L116`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L116), [`DBR.sol#L129`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L129), [`DBR.sol#L181`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L181), [`DBR.sol#L320-L321`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L320-L321), [`Oracle.sol#L12-L14`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12-L14), [`Oracle.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53), [`Fed.sol#L99`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L99), [`SimpleERC20Escrow.sol#L49`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L49), [`INVEscrow.sol#L25`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L25), [`INVEscrow.sol#L62`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62), [`GovTokenEscrow.sol#L16`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L16), [`GovTokenEscrow.sol#L56`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L56)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N3] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`Market.sol#L55-L56`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L55-L56), [`Market.sol#L97`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97), [`Market.sol#L101`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101), [`Market.sol#L226`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226), [`Market.sol#L245`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245), [`Market.sol#L323`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L323), [`Market.sol#L344`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L344), [`Market.sol#L353`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353), [`Market.sol#L389`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389), [`Market.sol#L460`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L460), [`DBR.sol#L13-L14`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13-L14), [`DBR.sol#L21-L22`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L21-L22), [`DBR.sol#L262`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L262), [`DBR.sol#L266`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L266)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N4] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`Market.sol#L92`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L92), [`DBR.sol#L44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L44), [`BorrowController.sol#L17`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L17), [`Oracle.sol#L35`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L35), [`Fed.sol#L14`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L14), [`Fed.sol#L131`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131), [`INVEscrow.sol#L10`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L10), [`INVEscrow.sol#L17`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L17), [`INVEscrow.sol#L79`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L79), [`GovTokenEscrow.sol#L9`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L9), [`GovTokenEscrow.sol#L66`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L66)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N5] Use Underscores for Number Literals
**Severity**: Informational
**Context**: [`Market.sol#L51`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L51), [`Market.sol#L74-L76`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74-L76), [`Market.sol#L150`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150), [`Market.sol#L162`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162), [`Market.sol#L173`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173), [`Market.sol#L184`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184), [`Market.sol#L195`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195), [`Market.sol#L336`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L336), [`Market.sol#L346`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L346), [`Market.sol#L360`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360), [`Market.sol#L377`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377), [`Market.sol#L563-L564`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563-L564), [`Market.sol#L583`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L583), [`Market.sol#L595`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595), [`Market.sol#L598`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598), [`Market.sol#L606`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606), [`DBR.sol#L330`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330), [`Oracle.sol#L95`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L95), [`Oracle.sol#L98`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L98), [`Oracle.sol#L134`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L134), [`Oracle.sol#L137`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L137)

**Description**:
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

**Recommendation**:
Consider using underscores for number literals to improve its readability.


## [NAZ-N6] TODOs Left In The Code
**Severity**: Informational
**Context**: [`INVEscrow.sol#L35`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35)

**Description**:
There should never be any TODOs in the code when deploying.

**Recommendation**:
Consider finishing the TODOs before deploying.


## [NAZ-N7] Spelling Errors
**Severity**: Informational
**Context**: [`Market.sol#L172 (setReplenismentIncentiveBps => setReplenishmentIncentiveBps)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172), [`Market.sol#L181 (liqudation => liquidation)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L181), [`Market.sol#L578 (liquidateable => liquidatable)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578), [`DBR.sol#L50 (oprator => operator)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50), [`DBR.sol#L60 (replen => replenishment)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60), [`DBR.sol#L87 (allowe => allow)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87), [`DBR.sol#L96 (unrepayable => non-repayable)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L96), [`BorrowController.sol#L36 (addres => address)`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.


## [NAZ-N8] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-inverse/tree/main/src)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N9] Floating Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-inverse/tree/main/src)

**Description**:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

**Recommendation**: 
Consider locking the pragma version.


## [NAZ-N10] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-inverse/tree/main/src)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.
