# Lot of function in all contracts could be external instead of public

## Problem:
Most of them should not be pulic as they are not used in the contract. Plus, it could save gas to have them external and it's a best practice.

Examples: 

- ** addMinter**, ** removeMinter** in ** DBR.sol ** 
- ** expansion **, ** contraction ** in ** Fed.sol ** 
- a lot in all contracts

## Suggestion:

If function is not used inside the contract, change its visibility from ** public ** to ** external **

# In ** DolaBorrowingRights.sol ** ** name ** , ** symbol **  could be immutable 


## Problem:
These two variables are never changed and should not, having them immutable could save gas and would make sure they will never be modified.
 
## Suggestion:

Put ** name ** , ** symbol ** as immutable in ** DolaBorrowingRights.sol ** 




# in ** DBR.sol ** the function ** transfer**,  ** transferFrom ** and ** burn ** could be optimized by adding one line in the **unchecked ** bloc


## Problem:
For example in ** transfer **, we have:

        ``require(balanceOf(msg.sender) >= amount, "Insufficient balance");

        balances[msg.sender] -= amount;
        unchecked {
            balances[to] += amount;
        }``

The second line could be put in the unchecked bloc as the previous require ensures that it won't underflow.


## Suggestion:
In the 3 functions, add the previous line in the ** unchecked** bloc to save a little bit of gas.
             

# In ** Market.sol ** and ** DBR.Sol ** some code is called two times.

- these require are called 2 times: 1st by market, then in forcereplenish

-  // xcz this replenishmentCost is computed in onforcereplenish too


## Problem:
In ** forceReplenish **, the 2 requires, are also called in ** onForceReplenish **. 
In ** forceReplenish **, replenishmentCost computation, is also called in ** onForceReplenish **. 
 
## Suggestion:
Code could be refactored to execute these codes only one time. For example, passing the result of the computation as a parameter to ** onForceReplenish **.


# In ** Market.sol **, function ** liquidate ** getPrice is called two times


## Problem:
It's computed in ** getCreditLimitInternal**  and then directly **  uint price = oracle.getPrice(address(collateral), collateralFactorBps);** 

## Suggestion

Compute the price only one time and give it as a parameter (for example), could save some gas.



