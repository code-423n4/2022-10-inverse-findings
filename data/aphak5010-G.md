# Inverse Finance Gas Optimization Findings
## Summary
The Gas savings are calculated using the `forge test --gas-report` command.  
The same tests were used that are provided in the contest repository.

| Issue      | Title | Instances | Estimated Gas Savings (Deployments) | Estimated Gas Savings (Method calls) |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| G-00      | Use `<10001` instead of `<=10000`       | 1 | 200 | 3 |
## Use `<10001` instead of `<=10000`
Deployment Gas saved: **200**  
Method call Gas saved: **3**

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162)

Fix:  
```solidity
require(_liquidationFactorBps > 0 && _liquidationFactorBps < 10001, "Invalid liquidation factor");
```

