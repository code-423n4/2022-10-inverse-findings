## CAPITALIZATION OF CONSTANTS
As documented in the link below:

https://docs.soliditylang.org/en/v0.8.17/style-guide.html?highlight=capitalized

Constants should be named with all capital letters with underscores separating words. There is one instance found.

Line[13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)

## USE OF BLOCK.TIMESTAMP
Some contract code uses the block.timestamp as part of the calculations and time checks. Nevertheless, timestamps can be slightly altered by miners/validators to favor them in contracts that have logics that depend strongly on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future. Here are eleven instances found.

[Line 423](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423)
[Line 487](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487)
[Line 122](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L122)
[Line 135](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L135)
[Line 148](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L148)
[Line 224](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224)
[Lines 286 - 290](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286-L290)
[Line 89](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L89)
[Line 124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L124)

## COMMENT AND CODE LINE LENGTH
Try limit the length of comments and/or code lines to 80 - 100 character long for readability sake. Here is one instance found.

[Line 12](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12)

## MISSING EVENTS ON CRITICAL OPERATIONS
Several critical operations do not trigger events, which will make it difficult to review the correct behavior of the contracts once deployed. Users and blockchain monitoring systems will not be able to easily detect suspicious behaviors without events. There are numerous instances found.

[Lines 118 - 197](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118-L197)
[Lines 53 - 61](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53-L61)
[Line 26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)
[Lines 48 - 69](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L69)
[Lines 62 - 65](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62-L65)

## TIMELOCK FOR CRITICAL PARAMETER CHANGE
It is a good practice giving time to users to react and adjust to critical changes with a mandatory time window between the changes. The first step is simply broadcasting to users with a specific change that is coming whilst the second step commits that change after an appropriate period of waiting. This would allow time for users opposing to the change to withdraw within the set time frame. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of the owner making a malicious act). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes. Here are the instances found.

[Lines 118 - 197](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118-L197)
[Lines 53 - 61](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53-L61)
[Line 26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)
[Lines 48 - 69](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L69)
[Lines 62 - 65](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62-L65)
[Lines 81 - 102](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81-L102)

Logically, this should also apply to the two step take over functions, `setPendingOperator()` and `claimOperator()`. 

[line 44](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44)
[Lines 66 - 71](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66-L71)
[Lines 53 - 55](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53-L55)
[Lines 70 - 75}(https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70-L75)

## UNCOMMENTED ASSEMBLY BLOCK
Assembly is a low-level language that is harder to parse by readers, consider including extensive documentation regarding the rationale behind its use, clearly explaining what every single assembly instruction does. This will make it easier for users to trust the code, for reviewers to verify it, and for developers to build on top of it or update it. Note that the use of assembly discards several important safety features of Solidity, which may render the code unsafer and more error-prone. Here are some instances found. There are two instances found.

[Lines 229 - 235](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L229-L235)
[Lines 296 - 305](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L296-L305)

## COMMENTED CODE LINES
Codebase with lines of code commented out with `//` can lead to confusion and is detrimental to overall code readability. Consider removing commented out lines of code that are no longer needed. There are two instances found.

[Lines 56 - 60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L56-L60)
[Lines 49 - 53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L49-L53)

## COMMENT AND CODE MISMATCH
```
    @dev Must be set between 1 and 10000.
```
[Line 158](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L158)
[Line 169](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L169)

If that was what the comment meant, the require statements should be refactored to the following code lines.

[Line 162](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
[Line 173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)

```
Line 162        require(_liquidationFactorBps > 1 && _liquidationFactorBps < 10000, "Invalid liquidation factor");

Line 173        require(_replenishmentIncentiveBps > 1 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
```
## THRESHOLD LIMIT CHECKS
Certain parameters of the contracts can be configured to invalid values, causing a variety of issues and breaking expected interactions within/between contracts. There are two instances found pertaining to critical parameter that lacks sanity/threshold/limit checks.

[Line 41](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L41)
[Line 59](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L59)

## CONSTRUCTOR SANITY CHECKS
Zero address, zero value, and empty string checks implemented at the constructor could avoid human errors leading to non-functional calls associated with the mistakes. This is especially so when the incidents entail immutable state variables that could end up having had to redeploy the contract. There are five contract instances found.

[Lines 13 -15](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L13-L15)
[Lines 30 - 42](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L30-L42)
[Lines 36 - 42](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L36-L42)
[Lines 61 - 90](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L61-L90)
[Lines 29 - 33](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L29-L33)