## Missing checks for `address(0x0)` when assigning values to `address` state variables
It is important to add check for `address(0X0)` when assigning values to `address` state variables. adding that check could minimize the risk of human error that assigning zero address.
- operator
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
- pending operator
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53-L55
- minter
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81-L84
- governer
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
- lender
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
- pause guardian
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
- borrow controller
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
- oracle
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118

## Missing event and or timelock for critical parameter change
Events help non-contract tools to track changes, and events prevent users from being surprised by changes.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62-L65
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L51
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57-L60
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66-L69
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212-L219


## Typo
Should be "allowed" not "allowe". I suggest to fix the line below :
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87


## Place interface in separate files
It is better to organize the files by separating the interface and smart contract in different file. 
There are some instances for this issue:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L5-L9
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L4-L7
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L4-L8
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L10-L15
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L17-L19
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L5-L9
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L11-L14
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L16-L21
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L23-L30
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L32-L34
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L5-L12
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L14-L20
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L5-L11
