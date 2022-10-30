## REPLACE `MODIFIER` WITH `FUNCTION`
Modifiers make code more elegant, but cost more than normal functions.

Instances include:
[`src/BorrowController.sol:17`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L17)
[`src/DBR.sol:44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L44)
[`src/Market.sol:92`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L92)
[`src/Oracle.sol:35`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L35)

## Require/Revert strings longer than 32 bytes cost additional gas
Instances include:
[`src/Market.sol:76`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76)
[`src/Market.sol:214`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214)
[`src/DBR.sol:63`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63)
[`src/DBR.sol:326`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326)

## Splitting require() statements that use && saves gas
Instances include:
[`src/Market.sol:75`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
[`src/Market.sol:162`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
[`src/Market.sol:173`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)
[`src/Market.sol:184`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)
[`src/Market.sol:195`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)
[`src/Market.sol:448`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)
[`src/Market.sol:512`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512)
[`src/DBR.sol:249`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249)

## MULTIPLE ACCESSES OF A MAPPING/ARRAY SHOULD USE A LOCAL VARIABLE CACHE
instances:
[`src/DBR.sol:284-L292`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L284-L292)
[`src/DBR.sol:133-L138`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L133-L138)      
[`src/DBR.sol:120-L125`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L120-L125)      
[`src/Fed.sol:103-L113`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103-L113)      
[`src/Market.sol:591-L612`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591-L612)
[`src/Market.sol:559-L572`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559-L572)
[`src/Market.sol:389-L401`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389-L401)
[`src/Market.sol:245-L251`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245-L251)
[`src/Oracle.sol:78-L105`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L78-L105)  
[`src/Oracle.sol:112-L144`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L112-L144)