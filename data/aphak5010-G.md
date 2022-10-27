# Gas Optimizations
## Use `<10001` instead of `<=10000`
[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162)

This will save 3 Gas.
`setLiquidationFactorBps` max Gas changes from 7488 to 7485.