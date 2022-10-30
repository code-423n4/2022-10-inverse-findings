# **Low and Non-Critical**

Serial No. | Issue Name | Instances
--- | --- | ---
Q-1 | Open TODOs | 1
Q-2 | Cross-Chain Replay Attacks | 6

-----
***Total instances found in this contest: 7 | Among all files in scope***


## [Q-01] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

### Instances:
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L35
### References:

[https://code4rena.com/reports/2022-05-sturdy/#l-09-open-todos](https://code4rena.com/reports/2022-05-sturdy/#l-09-open-todos)


-----


## [Q-02] Cross-Chain Replay Attacks

Storing the `block.chainid` is not safe. See [this](https://github.com/code-423n4/2021-04-maple-findings/issues/2) issue from a prior contest for details.

Ethereum fork in the feature, the `chained` will change however the one used by the permits will not be enabling a user to use any new permits on both chains thus breaking the token on the forked chain permanently.

### Instances:
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L88
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L98
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L108
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L40
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L263
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L273

### References:

[https://github.com/code-423n4/2021-04-maple-findings/issues/2](https://github.com/code-423n4/2021-04-maple-findings/issues/2)


-----

