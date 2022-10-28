## Missing `zero address` validation

If contract miss this zero check address validation there is chance that contract will loose some functionality, which can cause the redeployment of contract.

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39-L41
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L80

## `require()` statement should have descriptive reason strings

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L91

## `TODO` comments

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35

## TYPOS

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36 ///@audit: `addres`
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50  ///@audit: `oprator`
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87  ///@audit: `allowe`
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128  ///@audit: `th`
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L146  ///@audit `mus`
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L181  ///@audit `liqudation`

## Use of `block.timestamp` 
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L124
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L89
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L135
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L148
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224

## Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L616

## `NatSpec` is incomplete

* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L221-L226    /// @audit Missing: '@return'
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L240-L245    /// @audit Missing: '@return'
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L287-L292    /// @audit Missing: '@return'
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L308-L312    /// @audit Missing: '@return'
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L574-L578    /// @audit Missing: '@return'
* https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L365-L370    /// @audit Missing: '@return'
