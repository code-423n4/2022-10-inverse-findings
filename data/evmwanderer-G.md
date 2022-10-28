## Summary

| |Issue|Instances|
|-|:-|:-:|
| 1 | Use unchecked math | 2 |
| 2 | Redundant token transfer | 1 |


### 1. Use unchecked math
Overflow preconditions are guarded by the revert statements above these operations, so they can go in the unchecked block.

*There are 2 instances of this issue:*

```solidity
File: src/DBR.sol   #1

172:           balances[msg.sender] -= amount;
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L172


```solidity
File: src/DBR.sol   #2

196:           balances[msg.sender] -= amount;
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L196


### 2. Redundant token transfer
When recalling tokens two transfers are performed.

`market` -> `fed` -> `governance`

At the following functions

```solidity
File: src/escrows/Market.sol  

function recall(uint amount) public {

require(msg.sender == lender, "Only lender can recall");

dola.transfer(msg.sender, amount);

}
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L203


```solidity
File: src/escrows/Fed.sol  

function takeProfit(IMarket market) public {

uint profit = getProfit(market);

if(profit > 0) {

market.recall(profit);

dola.transfer(gov, profit);

}

```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L131

This can be achieved in one transfer with the following modifications

```solidity
File: src/escrows/Market.sol  

function recall(uint amount, address recipient) public {

require(msg.sender == lender, "Only lender can recall");

dola.transfer(recipient, amount);

}
```

```solidity
File: src/escrows/Fed.sol  

function takeProfit(IMarket market) public {

uint profit = getProfit(market);

if(profit > 0) {

market.recall(profit, gov);

}

```
