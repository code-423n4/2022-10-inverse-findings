-> MISSING INDEXED FIELDS IN EVENT

Fields, that are indexed, are more quickly accessible to off-chain tools that parse events. Every indexed field cost extra gas.
If there are three or more fields in an event you should use at least 3 or more indexed fields. When there are less than three fields all should be indexed.


https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=event%20Transfer(address%20indexed%20from%2C%20address%20indexed%20to%2C%20uint256%20amount)%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=event%20Approval(address%20indexed%20owner%2C%20address%20indexed%20spender%2C%20uint256%20amount)%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=event%20Deposit(address%20indexed%20account%2C%20uint%20amount)%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=event%20Borrow(address%20indexed%20account%2C%20uint%20amount)%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#:~:text=event%20RecordDailyLow(address%20indexed%20token%2C%20uint%20price)%3B

-> _MINT SHOULD BE AVOID WHERE EVER POSSIBLE

_mint() is discouraged in favor of _safeMint() that insures the recipient is either an  IERC721Receiver or implements an EOA.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=user%5D%20%2B%3D%20replenishmentCost%3B-,_mint,-(user%2C%20amount)%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=OR%20OPERATOR%22)%3B-,_mint(to%2C%20amount)%3B,-%7D
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=event%20CreateEscrow(address%20indexed%20user%2C%20address%20escrow)%3B


