

## G01 Saving SLOAD's. 12 in total (~1200 gas)

```DBR.sol```
1 - ```dueTokensAccrued[user]``` is accessed twice (line 123 and line 124). cache ```dueTokensAccrued[user]``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124
2 - ```balances[user]``` is accessed twice (line 123 and line 124). cache ```balances[user]``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124
3 - ```dueTokensAccrued[user]``` is accessed twice (line 136 and line 137). cache ```dueTokensAccrued[user]``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137
4 - ```balances[user]``` is accessed twice (line 136 and line 137). cache ```balances[user]``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137
5 - ```lastUpdated[user]``` is accessed twice (line 286 and line 287). cache ```lastUpdated[user]``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286-L287


```Market.sol```
6 - ```gov``` is accessed twice (line 1214 and line 216). cache ```gov``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L214-L216
7 - ```escrows[user]``` is accessed twice (line 246). cache ```escrows[user]``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246
8 - ```collateralFactorBps``` is accessed three times (line 359 and 360). cache ```collateralFactorBps``` and use from stack to save 2 SLOAD's.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L359-L360
9 - ```collateralFactorBps``` is accessed three times (line 376 and 377). cache ```collateralFactorBps``` and use from stack to save 2 SLOAD's.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L376-L377


```Oracle.sol```
10 - Replace lines 67 and 68 and use cached ```operator``` within the require statement to save 1 SLOAD.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67-L68




## G02 Add control to avoid unnecessary calculations

```Fed.sol```
You can add ```if(x == 0) return 0;``` type of controls to avoid unnecessary calculations for the below instances:
1 - ```if(collateralBalance == 0) return 0;``` above line 315
2 - ```if(collateralBalance == 0) return 0;``` above line 326
3 - ```if(collateralValue == 0) return 0;```   above line 336
4 - ```if(collateralValue == 0) return 0;```   above line 346




## G03 Reorganize lines to avoid unnecessary execution

```Fed.sol```

Lines 92-93 can be carried above line 90, so that if ```require(globalSupply <= supplyCeiling);``` fails, unnecessary operations would be avoided.
```expansion()``` function would look like this:
~~~
86:     function expansion(IMarket market, uint amount) public {
87:         require(msg.sender == chair, "ONLY CHAIR");
88:         require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
89:         require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
90:         globalSupply += amount;
91:         require(globalSupply <= supplyCeiling);
92:         dola.mint(address(market), amount);
93:         supplies[market] += amount;
94:         emit Expansion(market, amount);
95:     }
~~~


## G04 Split require statements that use &&. 8 instances, 3 gas per instance (24 gas)

```DBR.sol```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249

```Market.sol```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512



## G05 No need to check ```== true``` in conditional statements. 1 instance, 6 gas per instance (6 gas)

```DBR.sol```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L350
