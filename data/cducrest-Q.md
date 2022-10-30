## C4 audit contest: 2022-10-inverse

### Low Risk: dbr approve does not check initial allowance

Description: If there is an existing allowance from Alice to Bob of x, and Alice wants to update it to y, she will call
`approve(Bob, y)`. If before transaction execution (e.g. via front-running) Bob calls `transferFrom(Alice, Bob, x)`, he will
be able to withdraw `x` before the approval and `y` again after the execution of Alice's `approve`. The same reasoning applies
to the `permit` function.

Suggestion: Add a `require(allowance[msg.sender][spender] == 0)` in `approve` and `require(allowance[owner][spender] == 0)`
in `permit`.

Reference: https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L159
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224

### Non-Critical: dbr.replenishmentPriceBps must be >= 100% 

Description: The protocol makes no sense and is vulnerable if `dbr.replenishmentPriceBps` is lower than `10_000`.
It would mean the replenishment costs are lower than the amount minted to users on replenishment (i.e. the market operates at a loss).

Suggestion: Add `require(replenishmentPriceBps >= 10_000)` in constructor and `setReplenishmentPriceBps` of `DolaBorrowingRights`

Reference: https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330

### Non-Critical: Inconsistencies in between constructor and setter functions

Description: In market.sol `setLiquidationFactorBps` and `setReplenismentIncentiveBps` cannot set the `liquidationFactorBps` and `replenishmentIncentiveBps`
to `0` while the constructor can. 

Suggestion: Use similar requirements in the constructor and setters function

Reference: https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
