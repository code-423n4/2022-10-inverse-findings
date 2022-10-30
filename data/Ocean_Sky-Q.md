N-01 Event is missing indexed fields.

Each event should use three indexed fields if there are three or more fields
The indexed parameters for logged events will allow you to search for these events using the indexed parameters as filters.

Instances:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382

N-02 Constants should be defined rather than using magic numbers.

Instances:
https://github.com/InverseFinance/FiRM-code4rena/blob/main/src/Market.sol#L73-L75
https://github.com/InverseFinance/FiRM-code4rena/blob/main/src/Market.sol#L161

Mitigation:
Define constant variables for the literal values aforementioned.

N-03 Floating Pragmas
In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. 
Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
	
Proof of concept
https://swcregistry.io/docs/SWC-103
	
Instances
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L2
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L2
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L2
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L2
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L2

N-04 File is Missing Natspec on various lines of codes.
	
Every contract file should have rich documentation (natspec) for its functions, variables, parameters and more.
	
Instances:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101

N-05 Typo error

Misspelled the words "operator", "allowed", "the" in comments.

Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128