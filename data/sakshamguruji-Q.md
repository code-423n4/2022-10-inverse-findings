## USE OF tx.origin

The use of tx.origin can be manipulated by fishing the origin address by executing a transaction on attacker's behalf therefore an attacker can pretend to be the originator of the contract and execute the function protected by something like require(msgSender = tx.origin). Read more about it here 
https://solidity-by-example.org/hacks/phishing-with-tx-origin/.

Affected Code:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L47

Remediation:
Use something like `require(msg.sender == owner)` and in the constructor set `owner = msg.sender` so that msg.sender is the owner of the contract or the who deployed the contract.


## EVENT IS MISSING INDEXED FIELDS

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

Affected Code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L140
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L141
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L614-L620
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L147


##  USE A MODIFIER INSTEAD OF USING THE SAME REQUIRE STATEMENT AGAIN AND AGAIN

Instead of using the same require statements again and again , it should be considered to put those statement in a modifier and then use those 
modifiers in functions where needed.

Affected code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49
This require statement should be kept in a modifier and that modifier can be used in function  https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48 , https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57 , 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66
Similarly  , use a onlyChair modifier for the functions that require msg.sender is chair.

## LACK OF ZERO ADDRESS CHECKS

There are multiple functions where they lack zero address checks , if set to zero address these functions can set the addresses to useless addresses and make them useless.

Affected code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36
Check if the recipient is not a zero address , if it is funds may be transferred to a zero address and be lost forever

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57
Add a 0 address check here https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53 to avoid setting the pendingOperator to a 0 address.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81 Here we set the minter  , add a zero address check here so that we dont 
add invalid address(s) to this mapping.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
Here we add a market to the markets mapping  ,   add a zero address check here so that we dont 
add invalid address(s) to this mapping and populate the mapping with garbage values.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158
Add a zero address check in the approve function to avoid mistakenly approving zero address .

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66
Avoid mistakenly setting the `chair` address to 0 address.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
Might save gas to check 0 address beforehand .(Further checks won't be executed)

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
Avoid setting the oracle to a 0 address

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
Avoid setting the borrowController to a 0 address

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
Avoid setting the lender to a zero address.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
Avoid setting the pauseGuardian to zero address

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591
check if the user to liquidate is not the zero address.



## SPELLING MISTAKES IN CODE

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L45
Instead of `fed contact` should be `fed contract`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87
Change `allowe` to `allowed`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128
Change `th` to `the`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L146
Change `mus` to `must`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L589
Change `Th` to `The`

## CHANGE FUNCTION NAMES TO AVOID CONFUSION

Consider changing the names of these functions to avoid confusion as there are more function with similar name -

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L278
Change the name to something like depositOnBehalf as there is already a function with name deposit.


## FUNCTION PARAMETERS SHOULD HAVE A UNDERSCORE

It is a convention to declare function parameters with an underscore. Instances of functions with parameters without an underscore - 

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L120
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L120
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L133
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L215
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L284
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L340
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L349
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L359
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L372
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L78
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L112
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L90
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L66
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L258
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L278
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L292
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L312
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L323
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L334
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L344
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L370
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L408
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L460
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L472
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L531
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591


## INTERNAL FUNCTIONS SHOULD BEGIN WITH AN UNDERSCORE

It is a naming convention to name the internal functions by having a underscore at the start(or at the end) . Change the names from e.g. functionName to _functionName.
Instances of this issue:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L323
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L344
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L460


## FLOATING PRAGMA USED THROUGHOUT THE CODEBASE

Throughout the codebase floating pragma version has been used , to be on a safer side all the development should be done on a fixed pragma/solidity version as there may be a vulnerability in the later versions within that pragma which may be abused in the future.






 