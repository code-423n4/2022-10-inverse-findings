## Check If A Minter Already Exists

On the function here https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195 it adds a new minter , there should be a 
require statement like this `require(minters[_minter] == false)`  to check if the minter is already added this would save gas as the function will
not execute fully(setting the mapping yo true and then emitting an event)

Similarly , add a require that require(minters[_minter] == true) here in this function https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90

and Similarly check if the market already exists here https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90


## Change > to >= and < to <= To Save Gas

On line https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123 instead of doing   ` if(dueTokensAccrued[user] + accrued > balances[user]) return 0` do `   `if(dueTokensAccrued[user] + accrued >= balances[user]) return 0` because if `dueTokensAccrued[user] + accrued = balances[user]` then the statement below `balances[user] - dueTokensAccrued[user] - accrued` would return zero , so changing to >= would make us avoid the last check .

Similarly , https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L133 here in this function instead of doing 
`dueTokensAccrued[user] + accrued < balances[user]` do this `dueTokensAccrued[user] + accrued <= balances[user]`

## X += Y costs 22 more gas than X = X + Y

This can mean a lot of gas wasted in a function call when the computation is repeated n times (loops)

 Recommended Mitigation

use X = X + Y instead of X += Y (same with -).

Affected Code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L92
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L111
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L289
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L362
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L376
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L535
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L599
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L600


## MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per
mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in 
the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having
to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

Affected Code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L19-L28 (combine all address -> something mappings to one address -> struct mapping)
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57-L59
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L25-L26


## REDUCE THE SIZE OF ERROR MESSAGES (LONG REVERT STRINGS)

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert
 condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for 
computing memory offset, etc.

Affected Code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214


## SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

With enough runtime calls, the change ends up being cheaper i.e. splitting require statements using &&.

Affected Code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195






