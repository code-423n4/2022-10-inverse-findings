## Non-critical

### Contract is missing events for certain actions

Locations
[1](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44)
[2](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53)
[3](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61)
[4](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48)
[5](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L66)
[6](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L75)
[7](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53)
[8](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26)
[9](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L118)
[10](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130)
[11](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L124)
[12](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L136)
[13](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L142)
[14](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L212)

#### Description

Sensitive actions like access control changes and ownership on contracts should emit events for easy tracking and notification.

#### Suggested course of action

Implement new events for functions that should have them.

## Low

### Unorthodox token decimals normalization algorithm limits maximum token precision and introduces operational risk

Locations:
[1](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L85-L88)
[2](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L119-L122)

#### Description

The `Oracle` contract handles the recording of pricing data for the lending system. Other contracts utilize `viewPrice()` and `getPrice()` to fetch pricing data.

These functions include the following lines:

```
uint8 feedDecimals = feeds[token].feed.decimals();
uint8 tokenDecimals = feeds[token].tokenDecimals;
uint8 decimals = 36 - feedDecimals - tokenDecimals;
uint normalizedPrice = price * (10 ** decimals);
```

`tokenDecimals` comes from data supplied to the setter function `setFeed()` as an argument set by the operator. `feedDecimals` comes from data supplied by the Chainlink infrastructure.

As such there are a couple of potential issues. 

1) It's unclear from the code why two 'samples' of the token's precision are being taken during normalization. If the operator supplies an incorrect value via `setFeed()`, the normalized price could be off by one or more orders of magnitude. The gain compared to the operational risk is unclear.
1) Due to the subtraction, assuming the two values are equal, the protocol is limiting itself to tokens that use  at most 18 decimals of precision. While this is the overwhelming majority of tokens, this makes the contract somewhat inflexible against future developments.

#### Suggested course of action

Consider the business case for allowing the operator to supply a token precision that diverges from the Chainlink-supplied value. If the intention is to tacitly support other oracles that might not directly supply precision values, carefully weigh the risk of such oracles later adding such data to the operation of the protocol oracle.

## Low
​
### DBR token lacks address(0) guards on transfer functions while possessing public burn function
​
#### Description
​
The `DolaBorrowingRights` contract implements most ERC20 semantics, including transfer functions and mint/burn functions.
​
However, while `burn()` is public and emits an Event for burning, the `transfer()` and `transferFrom()` functions lack a check to block 'burning' of tokens by sending them to the 0 address. 
​
#### Suggested course of action
​
Modify the transfer functions to that it's clear that burning tokens is handled via `burn()` and so that the correct intentioned events are emitted.

