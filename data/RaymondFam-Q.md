## Zero Address Checks
Zero address checks should be implemented at the constructor to avoid human error(s) that could result in non-functional calls associated with them particularly when the incidents involve immutable variables that could lead to contract redeployment at its worst. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L83

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

