## Impact
DBR.sol - `accrueDueTokens` function does not update the values if it is called twice in the same block.
When some user borrow, repay or forceReplenishment twice (or more) in the same block, inconsistencies in DBR contract are created.

Due to twice (or more) calls to `accrueDueTokens` function we could generate inconsistence mainly in `totalDueTokensAccrued` field.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L288-L290


## Steps

If some user calls to one of the three operations mentioned before, `accrueDueTokens` function is called. All the work is made correctly first time.
But when the user call twice (or more times) to one of the three operations in the same block, `accrueDueTokens` function stops at the beggining and do not update `dueTokensAccrued[user]`, `totalDueTokensAccrued` and `lastUpdated[user]`. 
Additionally, `transfer` event is not emitted. It does not look big issue.


Because of not updating those fields could cause the following:

`lastUpdated[user]` -> It does not matter because it is the same block.
`dueTokensAccrued[user]` -> The outdated value causes wrong returns values in functions like `balanceOf(user)`, `signedBalanceOf(user)` and `deficitOf(user)` in the same block. Please note that `accrued` variable will result 0 because of `block.timestamp - lastUpdated[user]` calculation.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L120-L150


`totalDueTokensAccrued` -> It has longtime impact. This field is used to calculate the `totalSupply()` returns. 

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L109-L112


The only way to fix it is to call  `accrueDueTokens` function manually for desired user or wait to the user executes one of the three functions which call it. The manual fix could be tedious because it is highly probably you do not notice it.

## Proof of Concept

Borrow, repay or forceReplenishment twice (or more) in the same block to see this data inconsistence.


## Tools Used
Manual and Foundry

## Recommended Mitigation Steps

Revert in case `block.timestamp == lastUpdated[user]` condition is met.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L286

