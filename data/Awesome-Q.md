## `block.timestamp` is unreliable
Using block.timestamp as part of the time checks could be slightly modified by miners/validators to favor them in contracts that contain logic heavily dependent on them.

Consider this problem and warn users that a scenario like this could occur. If the change of timestamps will not affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future.

Here are some of the instances:
[Line 423](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423)
[Line 487](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487)

## Poorly Commented Assembly Block
Assembly block is used in a multitude of contracts in Inverse Finance. Although this does not pose a security risk by itself, it is simultaneously a complicated and essential part of the system. Moreover, as this is a low-level language that is more difficult to parse for readers, consider including comprehensive documentation about the rationale behind its usage, clearly describing what every single assembly instruction does. This choice will make it easier for users to trust the code, for reviewers to verify it, and for developers to build on top of it or update it. Note that the using assembly discards several important safety features of Solidity, which may cause the code to be less safe and more error-prone.

Here are some of the instances:

[Line 229-235](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L229-L235)

[Line 296-305](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L296-L305)




