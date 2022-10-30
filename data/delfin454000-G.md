### Avoid use of '&&' within a `require` function
Splitting into separate `require()` statements saves gas
___
[DBR.sol: L249](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L249)
```solidity
            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```
Recommendation:
```solidity
            require(recoveredAddress != address(0), "INVALID_SIGNER");
            recoveredAddress == owner, "INVALID_SIGNER");
```

Similarly for the following:

[Market.sol: L75](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L75)

[Market.sol: L162](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162)

[Market.sol: L173](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L173)

[Market.sol: L184](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L184)

[Market.sol: L195](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L195)

[Market.sol: L448](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L448)

[Market.sol: L512](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L512)
___
___