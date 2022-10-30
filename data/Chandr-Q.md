# Missing events for critical operations
### Details
There is critical operation that change critical values that affect ownership of controller.
It's a best practice for these setter functions to emit events to record these changes on-chain for off-chain monitors/tools/interfaces to register the updates and react if necessary.

Instances include:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46-L49
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L64
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L151
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L163
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L174 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L185 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L196 

### Recommendation
Add event for critical operation
# PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD
### Details
Contracts are allowed to override their parents’ functions and change the visibility from external to public.

Instances include:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46-L49
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53-L55
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62-L65
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70-L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81-L84
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90-L93
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99-L102
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109-L112
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146-L150
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158-L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170-L178
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188-L202
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L215-L253
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258-L260
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300-L305
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313-L317
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325-L334
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L340-L342
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L349-L352
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L51
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57-L60
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66-L69
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75-L78
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86-L95
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103-L113
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131-L137
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L78
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L79
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149-152
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161-164
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172-175
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183-186
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194-197
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203-206
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212-219
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267-270
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L370-380
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422-451
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486-515
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L520-522
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546-549
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559-572
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578-584
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591-612
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#:53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66-L71
# Boolean equality
### Details
Detects the comparison to boolean constants.
Boolean constants can be used directly and do not need to be compare to true or false.
Instances include:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L350
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L89
### Recommendation
Remove the equality to the boolean constant.

# Zero address check
### Details
Missing Zero address check

Instances include:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L13-L15
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L54
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39-L40
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L50
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L68
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
### Recommendation
Check that the address is zero

# UNCHECKED TRANSFER
### Details
Boolean return value for transfer is not checked.

Instances include:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L135
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L205
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L280
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L399
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L537
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L570
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L602
### Recommendation
Check that the address is zero
bool success = dola.transfer(gov, profit);
require(success, "Transfer failed."); }
# Divide before multiply
### Details
Solidity integer division might truncate. As a result, performing multiplication before division can sometimes avoid loss of precision.

Instances include:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563-L564
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L597-L598
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606
### Recommendation
The multiplication should always be placed at the end to avoid miscalculations like the following one:
b=5
d=10
c=2 
a = (b/d)*c <<(5/10)*2 == 0>>
a = (b*c)/ d <<(5*2)/10 == 1>>

