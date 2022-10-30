# Summary

| Id | Title |
| -- | ----- |
| 1 | Borrower can avoid paying interest by spamming `accrueDueTokens(...)` function  |
| 2 | `liquidationFeeBps` cannot be set back to `0` |
| 3 | No upper limit in `setReplenishmentPriceBps(...)` |

## 1. Borrower can avoid paying interest by spamming `accrueDueTokens(...)` function 

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L288

In function `DBR.accrueDueTokens(...)`, accrued due is calculated as 
```solidity
uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```

This calculation has a division without `PRECISION`. If `debt` is small enough, borrower can spam this function (calling it every block) and the rouding down calculation to `0` will help borrower avoid paying interest.


### Recommendation
Consider adding a `PRECISION` in interest calcultion

## 2. `liquidationFeeBps` cannot be set back to `0`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L194-L197

In constructor, `liquidationFeeBps` is not set and has default value `0`. After that, governance can call `setLiquidationFeeBps(...)` to set value for `liquidationFeeBps`. 
```solidity
function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
    require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee"); // @audit cannot be set to 0
    liquidationFeeBps = _liquidationFeeBps;
}
```
However, when it value is changed, it cannot be changed back to `0`. So in case, governance want to remove fee to help boost the market, they cannot do it.

### Recommendation
Consider allowing `liquidationFeeBps` to be set back to 0


## 3. No upper limit in `setReplenishmentPriceBps(...)`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L62

There is no upper limit in `setReplenishmentPriceBps(...)` function. Operator can set unreasonable high `replenishmentPriceBps` value that even bigger than `10000`.

### Recommendation
Consider adding a upper limit in `setReplenishmentPriceBps(...)` function


