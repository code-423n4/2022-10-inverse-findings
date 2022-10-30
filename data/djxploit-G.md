## 1)  State variable can be packed

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L53)

## 2)  Add unchecked keyword

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L362](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L362)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L379](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L379)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287)

## 3)  Cache storage variables used consecutively to save gas

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L110-L111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L110-L111) - `totalDueTokensAccrued`
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124) ,
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137) - `balances[user]`
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124),
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137) - `dueTokensAccrued`
`lastUpdated`  - [https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286-L287](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286-L287)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246) - `escrows[user]`
`liquidationFeeBps` - [https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605-L606](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605-L606)
`collateralFactorBps` - [https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L359-L360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L359-L360),
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L376-L377](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L376-L377)
`fixedPrices` - [https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L79](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L79), 
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L113](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L113)
`pendingOperator` - [https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67-L68](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67-L68)

## 4) Use calldata instead of memory 

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L32-L33](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L32-L33)
## 5)  require should be at start to save gas
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L303](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L303)

## 6) Combine multiple mappings with same key to struct

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L25-L27](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L25-L27)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57-L59](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57-L59)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L19-L20](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L19-L20)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L23-L28](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L23-L28)

## 7) Don’t emit storage variable

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L74](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L74) - `operator`
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L70](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L70)

## 8) Splitting `require()` statements that use `&&` saves gas (even a single &&)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)
    
