1. Use Custom errors to save gas:-
Recommendation :- make one contract with all errors and use it over other contract

code snippet:-
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L117
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L143
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L104
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L204
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L216
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L236
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L390
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L462
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L560
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L561
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L567
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L594
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L88
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L89
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L105
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L107
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L303
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L45
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L60
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L37


2. Use != instead of > in require :-

code snippet:-
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195

3. Use modifier instead of require  to save gas:-
Code snippet
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L71
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L60
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L91
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L37
