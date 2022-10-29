### Split `require` statements containing AND into multiple statements. 

Locations:
[1](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162)
[2](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L172)
[3](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L184)
[4](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L195)
[5](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L448)
[6](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L512)
[7](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L249)


#### Description

It is cheaper to split a `require` containing `&&` into multiple `require`s. The savings is somewhat variable between 12-18 gas per split.

Prioritize the `require` most likely to cause the revert first.

After modifying the `require` at location 6 above and running the `snapshot` command:

```
forge snapshot --diff
[⠰] Compiling...
[⠘] Compiling 7 files with 0.8.16
[⠊] Solc 0.8.16 finished in 5.35s
[...snip...]
test_permit_increasesAllowanceByAmount() (gas: -14 (-0.021%)) 
test_permit_reverts_whenNonceInvaidated() (gas: -14 (-0.031%)) 
Overall gas change: -28 (-0.052%)
```