# Unlocked pragma

## Problem:
Pragma is not locked, it's a bets practice to lock it to a level.


## Suggestion:
Don't use **pragma solidity ^0.8.13;**, use **pragma solidity 0.8.13;** 




# In **Fed.sol**, some code could be put in a modifier


## Problem:
This code: **require(msg.sender == gov, "ONLY GOV");** could be put in a modifier as it's called in many functions.

## Suggestion:
Put the mentionned code in a modifier and use the modifier where necessary.



# In **Fed.sol** , gov could pause expansion by setting low ceiling (maybe it's ok)


## Problem:
If a zero ceiling is set, Fed, won't be able to expanse to markets anymore/


## Suggestion:
If this an unwanted behavior, make ceiling be always bigger than supply.             

# In **DBR.sol** and ** Market.sol ** ecrecover is subject to signature malleability



## Problem:

See this article for example: http://coders-errand.com/malleability-ecdsa-signatures/
In our case, it shouldn't be dangerous as we use nonces, but it's a best practice to use openzeppelin library.

## Suggestion:
Use openzeppelin library

# In **Fed.sol** , check, effect, interaction pattern is not respected.

## Problem:

See here for example:

** function expansion(IMarket market, uint amount) public {
        require(msg.sender == chair, "ONLY CHAIR");
        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
        require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");

       
        dola.mint(address(market), amount);
        supplies[market] += amount;
        globalSupply += amount;

       
        require(globalSupply <= supplyCeiling);
        emit Expansion(market, amount);
    } **


** Dola.mint ** should be called after updating ** supplies ** and ** globalSupply **, for example.

It would limit the risks of reentrency and is a best practice.

We have also the case in **contraction** function

## Suggestion

Update the state before calling external contracts.

# In **Fed.sol** **takeProfit**:
 2 transfers could be simplified to one (**recall (address)** for example) because 2 transfers costs a lot of gas. (This should be in gas optimizations, but I did not have anymore time!)