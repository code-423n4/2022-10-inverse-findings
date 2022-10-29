1.Make a zero address check on address token!!

 Check if a function parameter address by default not calling zero address make require statement that point that address =! 0 this will reduce wastage of gas if a function set to zero address..

 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L78

2.2.Functions marked as  payable  are 24 gas cheaper than their counterpart (in non-payable functions, Solidity adds an extra check to ensure msg.value is zero). When users can't mistakenly send ETH to a function (as an example, when there's an onlyOwner modifier or alike), it is safe to mark it as payable.

 Proof of Concept
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61

Refernce: https://github.com/code-423n4/2022-01-elasticswap-findings/issues/41

Recommended Mitigation Steps
Mark as payable the external functions that have a trusted modifier which prevents users from mistakenly sending Ether.
