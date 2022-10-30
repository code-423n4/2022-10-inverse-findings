[G-01] X = X + Y IS CHEAPER THAN X += Y;

Usually does not work with struct and mappings.

   'debts[borrower] += amount;'

FIX:
   
   'debts[borrower] = debts[borrower] + amount;'

INSTANCES:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395-L397-L565-L568-L598;

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174-L198-L288-L289-L304-L332-L360-L362

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91-L92



[G-01.2] <x> = <x> + 1 even more efficient than pre increment.

   ' nonces[msg.sender]++'

FIX:

   ' nonces[msg.sender] =  nonces[msg.sender] + 1'

INSTANCES: 

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L521;

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L259;

