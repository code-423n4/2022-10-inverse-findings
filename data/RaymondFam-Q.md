## Zero Address Checks
Zero address checks should be implemented at the constructor to avoid human error(s) that could result in non-functional calls associated with them particularly when the incidents involve immutable variables that could lead to contract redeployment at its worst. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L83
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L37-L40

Better yet, incorporate complementary codehash checks just to make sure the address inputs are the matching ones if these have not been deemed an overkill from the developer team's perspective.

## `block.timestamp` Unreliable
The use of `block.timestamp` as part of the time checks can be slightly altered by miners/validators to favor them in contracts that have logic strongly dependent on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487

## Inadequately Commented Assembly Block
Assembly block is used in numerous contracts of Inverse Finance. While this does not pose a security risk per se, it is at the same time a complicated and critical part of the system. Moreover, as this is a low-level language that is harder to parse by readers, consider including extensive documentation regarding the rationale behind its use, clearly explaining what every single assembly instruction does. This will make it easier for users to trust the code, for reviewers to verify it, and for developers to build on top of it or update it. Note that the use of assembly discards several important safety features of Solidity, which may render the code less safe and more error-prone.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L229-L235
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L296-L305

## `constant` Variables Should be Capitalized
Constants should be named with all capital letters with underscores separating words if need be. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13

## Use of `uint` Instead of `uint256`
Across the codebase, there are numerous instances of uint, as opposed to uint256. In favor of explicitness, consider replacing all instances of uint with uint256. 

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L6-L33
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L47-L54
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L58
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L69-L71

## Lack of Events for Critical Operations
Critical operations not triggering events will make it difficult to review the correct behavior of the deployed contracts. Users and blockchain monitoring systems will not be able to detect suspicious behaviors at ease without events. Consider adding events where appropriate for all critical operations for better support of off-chain logging API. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61

## Add a Timelock to Critical Parameter Change
It is a good practice to give time for users to react and adjust to critical changes with a mandatory time window between them. The first step merely broadcasts to users that a particular change is coming, and the second step commits that change after a suitable waiting period. This allows users that do not accept the change to withdraw within the grace period. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious Owner making any malicious or ulterior intention). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61

## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L11-L112

## Events Associated With Setter Functions
Consider having events associated with setter functions emit both the new and old values instead of just the new value. Here are two of the instances entailed which may have the functions unanimously refactored as follows:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70-L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66-L71

```
    event ChangeOperator(address indexed oldOperator, address indexed newOperator);

    function claimOperator() public {
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
        emit ChangeOperator(operator, msg.sender);
        operator = pendingOperator;
        pendingOperator = address(0);
    }
```
## Sanity/Threshold/Limit checks on Setter Functions
Devoid of sanity/threshold/limit checks, certain critical parameters of the contracts can be configured to invalid values, causing a variety of issues and breaking expected interactions between contracts. Consider adding proper validation checks in the setter function logic. If the validation procedure is unclear or too complex to implement on-chain, document the potential issues that could produce invalid values. Here are some of the instances entailed that would need a boundary limit in place:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L41
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L59

## Add a Constructor Initializer
As per Openzeppelin's recommendation:

https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/6

The guidelines are now to prevent front-running of `initialize()` on an implementation contract, by adding an empty constructor with the initializer modifier. Hence, the implementation contract gets initialized atomically upon deployment.

This feature is readily incorporated in the Solidity Wizard since the UUPS vulnerability discovery. You would just need to check UPGRADEABILITY to have the following constructor code block added to the contract:

```
    /// @custom:oz-upgrades-unsafe-allow constructor
    constructor() {
        _disableInitializers();
    }
```
Here are some of the contract instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L25
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L30

## No Storage Gap for Upgradeable Contracts
Consider adding a storage gap at the end of an upgradeable contract, just in case it would entail some child contracts in the future. This would ensure no shifting down of storage in the inheritance chain. Here are some of the contract instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol

```
uint256[50] private __gap;
```
## Commented Code
Throughout the codebase `SimpleERC20Escrow.sol` and `GovTokenEscrow.sol`, there are lines of code that have been commented out with //. This can lead to confusion and is detrimental to overall code readability. Consider removing commented out lines of code that are no longer needed.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L49-L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L56-L60

## Open TODOs
Open TODOs can point to architecture or programming issues that still need to be resolved. Consider resolving them before deploying.

Here is one of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35

## Missing Zero Value Checks
`_replenishmentIncentiveBps` at the constructor should include a zero value check to be consistent with its setter function:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173

That said, a similar check for `_liquidationIncentiveBps` should also be included unless there is a good reason for not doing so:
 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150

## Comment and Code Mistmatch
The comment said `_replenishmentIncentiveBps` must be set between 1 and 10000, but the require statement implemented a threshold check between 0 and 10000:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L169
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173

## Immutable Boolean
`callOnDepositCallback` is an immutable state boolean in `Market.sol` that will have its literal state or value determined at the constructor upon contract deployment. It does not make much sense implementing it in deposit() involving the following code lines:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L281-L283

```
        if(callOnDepositCallback) {
            escrow.onDeposit();
        }
```
If it has been set to true, the if condition can be removed since it is going to always run `escrow.onDeposit()`.

If it has been set to false, lines 281 - 283 can be removed since it is going to always skip `escrow.onDeposit()`.

Based on the code logic, it would make better sense setting it to true considering the collateral amount has been transferred from msg.sender to the escrow on line 280.