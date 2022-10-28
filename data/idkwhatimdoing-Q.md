[Low] Token and oracle decimals cannot be more than 36
Description:
Oracle denial of service could occur if oracle feed decimal and token decimal summed up more than 36, since there is no check
Recommendation: when adding a new feed in setFeed verify that the oracle and toke decimal sums to less or equal to 36.

[Low] Missing Zero Address Check 

Description:

The following locations are missing input validation:
- BorrowControl.sol L14 missing zero address check on _operator address 
- BorrowControl.sol L26 missing zero address check on _operator address 
- DBR.sol L39 missing zero address check on _operator  address 
- DBR.sol L170 missing zero address check on to address   
- DBR.sol L188 missing zero address check on to address 
- DBR.sol L349 missing zero address check on to address 
- Fed.sol L39 missing zero address check on _gov
- Fed.sol L40 missing zero address check on _chair 
- Fed.sol L48 missing zero address check on _gov 
- Oracle.sol L32 missing zero address check on _operator address 
- Market.sol L62 missing zero address check on _gov 
- Market.sol L63 missing zero address check on _lender
- Market.sol L64 missing zero address check on _pauseGuardian
- Market.sol L65 missing zero address check on _escrowImplementation


Recommendation: Add the missing zero address checks 

[Low] Validate Replenishment Bps

Description: The range of the newReplenishmentPriceBps is not verified in setReplenishmentPricePbs

Location: DBR.sol L62

Recommendation: Confirm newReplenishmentPriceBps less than 10000 (range check)


[Low] Prevent Minting by Setting Supply Ceiling Below Current Supply 

Description: In Fed changeSupplyCeiling, it is possible to set the supply ceiling below the current global supply. 
Therefore, it is possible to contract the supply and prevent future mints. 

Location: Fed.sol L59

Recommendation: Confirm _supplyCeiling is equal or greater than the current globalSupply


[Low] Emitting events when state variable

Description: 
It is good practice to emit events when updating a state variable.

The following locations are missing events when updating a state variable:
- BorrowControl.sol L26 when operator variable is updated, an event should be emitted.
- Fed.sol L50 when gov variable is updated,an event should be emitted.
- Fed.sol L57 when supplyCeiling is updated, an event should be emitted.
- Fed.sol L66 when chair is updated, an event should be emitted 
- Fed.sol L75 when chair is updated, an event should be emitted 
- Oracle.sol L53 when oracle is added or updated for a token 
- Market.sol L118 when oracle is added or updated in the market 
- Market.sol L124 when the borrow controller is updated 
- Market.sol L130 when governance address is updated 
- Market.sol L136 when the lender is updated 
- Market.sol L142 when the pause guardian is updated 
- Market.sol L149 when collateral factor is updated 
- Market.sol L161 when liquidation factor is updated
- Market.sol L172 when replenishment incentive is updated
- Market.sol L183 when liquidation incentive is updated
- Market.sol L194 when liquidation fee is updated


Recommendation: Add the missing events when updating state variables.


[Low] Double Spend Allowance Exploit 

Description:
The approve function allows another user to spend on behalf of the token owner.
The approve function is vulnerable to sandwich attacks, where the spender can use both the old 
and the new allowance due to transaction order.

Recommendation: Since DBR.sol is a non standard ERC20-token, the contract should include functions such as increaseAllowance and decreaseAllowance 
provide such exploit similar to OpenZeppelin's standard ERC20 template.