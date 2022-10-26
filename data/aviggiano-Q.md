# 1. State variable change methods on `Fed` contracts should emit an event for off-chain monitoring

The following functions do not emit an event:
- `changeGov`
- `changeSupplyCeiling`
- `changeChair`
- `resign`

# 2. State variable change methods on `Fed` contracts lack input validation

The following functions should check for `address != 0` or `uint256` boundaries:
- `constructor`
- `changeGov`
- `changeSupplyCeiling`
- `changeChair`

# 3. `Fed.globalSupply` variable is redundant with `Fed.dola.totalSupply` since this is an ERC-20 token

Recommendation: Remove `Fed.globalSupply` and use `Fed.dola.totalSupply()` instead.

