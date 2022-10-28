#### Precompute constant hashes

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L432
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L496
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L105
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L106
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L107
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L233

##### Description:

As listed in above context, there are several `keccak256` operations performed on constant values. Instead of performing them on each invocation, these hashes should be precomputed and used directly to save gas.

#### Use `unchecked` blocks when safe to do so

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L111
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L124
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L137

##### Description:

As listed in above context, there are several places in which math operations are executed in which we can be certain that an overflow/underflow is impossible. In these cases, we should place this logic within `unchecked` blocks to save gas.

#### Avoid recomputing logic which can be passed as parameters

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L325
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L559

##### Description:

In `Market.forceReplenish`, the values `deficit` and `replenishmentCost` are computed. Despite the fact that `Market.forceReplenish` is the only possible caller of `DBR.onForceReplenish`, these values are re-computed, when they can be more efficiently passed as parameters.

#### Avoid revert strings greater than 32 bytes

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L76

##### Description:

The revert string listed above in context is greater than 32 bytes long, costing additional gas on revert by requiring the management of multiple words in memory.

#### Can use uint256 instead of some smaller sized uints

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L119

##### Description:

As listed above in context, occasionally there are instances of uints of <256 bits used when not necessary. Usage of smaller sized uints generally requires more gas as the values have to be converted to 256 bits to execute operations on them.

#### Use `msg.sender` directly instead of passing as param

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L46

#### Description:

In `BorrowController.borrowAllowed`, `msg.sender` gets passed as a param when it would be more efficient to simply use `msg.sender` directly.
