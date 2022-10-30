### Gas Optimizations:
##### [G-01] Use `constant` instead of `immutable`   
##### [G-02] Use `external` instead of `public`
##### [G-03] Use `payable` for `onlyOwner` function


---
### Details:
#### [G-01] Use `constant` instead of `immutable`  for gas efficiency
src/Market.sol
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L44

Recommend changing from immutable to `constant`   
```sol
IERC20 public constant dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
```


#### [G-02]  Use `external` instead of `public`
src/Market.sol
onlyGov:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
---
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203 :: recall()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212 :: pauseBorrows()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267 :: depositAndBorrow()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422 :: borrowOnBehalf()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486 :: withdrawOnBehalf()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L520 :: invalidateNonce()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L531 :: repay()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546 :: repayAndWithdraw()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559 :: forceReplenish()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578 :: getLiquidatableDebt()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591 :: liquidate()

src/DBR.sol
onlyOperator:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
---
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70 :: claimOperator()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146 :: signedBalanceOf()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158 :: approve()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170 :: transfer()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188 :: transferFrom()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L215 :: permit()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258 :: invalidateNonce()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300 :: onBorrow()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313 :: onRepay()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325 :: onForceReplenish()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L340 :: burn()
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L349 :: mint()


Recommend changing to `external` since these functions are not called by internal methods



#### [G-03] Use `payable` for `onlyGov` and `onlyOperator` function
Since these functions are only for developers, chances of accidentally sending ETH along are very low 
src/Market.sol
onlyGov:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194

src/DBR.sol
onlyOperator:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99

Recommend marking these functions `payable` to save some gas

