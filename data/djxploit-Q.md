## 1) Avoid `ecrecover` as it is vulnerable to signature malleability

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L226
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L425
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L489

## 2) Avoid usage of `block.timestamp` as it can be manipulated by miners

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L135
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L224
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L423
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L487
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L89

## 3) Missing 0 address check

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39-L41
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L80
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67

##  4) `initialize` functions is vulnerable to frontrunning

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L30
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L44

## 5) Use of `tx.origin` is risky. Avoid it.
```
      if(msgSender == tx.origin) return true;

```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L47


## 6) `TODO` comments should be resolved before going into production

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35

## 7) Missing `NatSpec` details -  @return

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L240-L245
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L287-L292
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L318-L323
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L329-L334
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L339-L344
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L348-L353
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L365-L370

## 8) `indexed` field should be added to events
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L382
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L616-L619


## 9)TYPOS should be corrected

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36 - `addres`
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50 - `oprator`
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87 - `allowe`


