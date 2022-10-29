1.Wrong require condition get implemented!!

@notice Claims the operator role. Only successfully callable by the pending operator

require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67

As per the comment function claimOperator should only be callable by the pending operator and in require condition it is reverting if msg.sender = pendingoperator.
 
 Recommendation: Make require condition as require(msg.sender != pendingoperator,"ONLY PENDING OPERATOR")

 2.Make a check like other functions on uint debt:

 https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L593

 Make a check on uint debt as other functions like https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578  so that uint debt never be equal to zero.