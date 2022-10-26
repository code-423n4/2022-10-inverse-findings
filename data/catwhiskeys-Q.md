# 1) Function order
Functions should be ordered following the Solidity conventions.
Inside each contract, library or interface, use the following order:
Type declarations
State variables
Events
Modifiers
Functions

In these contracts events have been placed at the end, after functions.
4 contracts:
Market.sol 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L614-L620
DBR.sol 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381-L386
Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L146-L147
Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L140-L141

Recommended mitigation steps:
Place events before functions and modifiers.


# 2) Open TODOs
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.
Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35


# 3) Typos
mus - must
Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L146


# 4) Code and comment lines are too long
7 Instances:
Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12-L14
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53

SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L49

INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L25
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62

GovTokenEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L16
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L56

Recommended mitigation steps:
Consider dividing code lines into a shorter chunks to increase readability