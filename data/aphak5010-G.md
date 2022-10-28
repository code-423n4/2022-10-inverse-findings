# Inverse Finance Gas Optimization Findings
## Summary
The Gas savings are calculated using the `forge test --gas-report` command.  
The same tests were used that are provided in the contest repository.

| Issue      | Title | Instances | Estimated Gas Savings (Deployments) | Estimated Gas Savings (Method calls) |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| G-00      | Use `<10001` instead of `<=10000`       | 1 | 200 | 3 |
| G-01      | Use functions instead of modifiers       | 4 modifiers | 101306 | - |
| G-02      | `X = X + Y` costs less Gas than `X += Y`       | 7 | 10214 | 237 |

## [G-00] Use `<10001` instead of `<=10000`
Deployment Gas saved: **200**  
Method call Gas saved: **3**

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162)

Fix:  
```solidity
require(_liquidationFactorBps > 0 && _liquidationFactorBps < 10001, "Invalid liquidation factor");
```

## [G-01] Use functions instead of modifiers
Modifiers make code more elegant and readable but cost more Gas than functions.  
Arguably, the additional Gas cost outweighs the readability benefit.

There are 4 modifiers defined in the codebase:  
| Modifier      | Deployment Gas saved |
| ----------- | ----------- |
BorrowController.sol: `onlyOperator` | 7006 |
DBR.sol: `onlyOperator` | 23627 |
Oracle.sol: `onlyOperator` | 10006 |
Market.sol: `onlyGov` | 60667 |

Example of changing modifier `onlyGov` in market to a function:  
```solidity
-    modifier onlyGov {
+    function onlyGov() internal view {
         require(msg.sender == gov, "Only gov can call this function");
-        _;
     }

```
The function can then be used like this:  
```solidity
-    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
+    function setLiquidationFactorBps(uint _liquidationFactorBps) public {
+        onlyGov();
         require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
         liquidationFactorBps = _liquidationFactorBps;
     }
```

## [G-02] `X = X + Y` costs less Gas than `X += Y`
The same is true for `X -= Y`.  
Instances of this are included here as well.

This does usually not work with structs or mappings.
Deployment Gas saved (all instances): **10214**  
Method call Gas saved (all instances): **237**  

There are 7 instances of this.  
[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L289](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L289)

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360)

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L92](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L92)

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L111](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L111)

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L397](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L397)

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L568](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L568)

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L535](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L535)

Fix:  
```solidity
-        totalDueTokensAccrued += accrued;
+        totalDueTokensAccrued = totalDueTokensAccrued + accrued;
```
