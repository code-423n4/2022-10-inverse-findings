# [01] Increment can be made unchecked to save gas

It can save 30-40 gas.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L521

# [02] x += y costs more gas than x = x + y for state variables

Using the addition operator instead of plus-equals saves 113 [gas](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8).

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534-L535

# [03] Cache state variable reads

Caching of a state variable replace each Gwarmaccess (100 gas) with a cheaper stack read.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246

# [04] Don’t compare boolean expressions to boolean literals

if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L350
