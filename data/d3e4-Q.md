
## [L-01] Two-step transfer of operator and governance
The operator is critical for the functioning of BorrowController.sol. [Currently it is transferred in a single transaction and no check is performed on the address](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26). Consider implementing a two-step process just like in DBR.sol.
And similarly for [governance in Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48-L51).
And similarly for [governance in Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130).


## [L-02] Don't let chair == gov
Considering the comment at[Fed.sol#L73](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L73):
`@dev Useful for immediately removing chair powers in case of a wallet compromise.`
the governance should not be allowed to be the chair in [`Fed.changeChair()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L66-L69).

## [L-03] Don't uncheck balance updates
While unlikely, it is not inconceivable that an account's balance will overflow (especially considering the comment [`@dev This function will revert if a user has a balance of more than 2^255-1 DBR`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L142) and the fact that the accounting of the effective balance is done by a difference such that `balances[user]` can be arbitrarily high while the effective balance still is zero). `DBR.transfer()`, `DBR.transferFrom()` and `DBR._mint()` may then annihilate an account's balance by the unchecked `balances[to] += amount;`:
```solidity
unchecked {
    balances[to] += amount;
}
```
Consider removing the `unchecked` from [DBR.sol#L173-L175](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L173-L175), [DBR.sol#L197-L199](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L197-L199) and [DBR.sol#L361-L363](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L361-L363), and perhaps also, for good measure, the unchecked for total supply reduction in `DBR._burn()` at [DBR.sol#L375-L377](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L375-L377).

## [L-04] Missing constructor address(0)-checks
When deploying Market.sol a failure to properly set `gov`, `escrowImplementation`, `dbr` and `collateral` will result in a broken contract, necessitating redeployment. Consider adding address(0)-checks for these in the [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L61-L90)


## [L-05] Functions return bool but cannot return false
Some functions return bool but only return true unless reverting. This means the return value is meaningless. This may cause problems if they are elsewhere expected to return `false` rather than revert.

Instances:
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L158-L162
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170-L178
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188-L202

## [NC-01] Inconsistent range check for `replenishmentIncentiveBps`
In Market.sol `replenishmentIncentiveBps` is required to be >0 [in its setter function](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L173) but not [in the constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L76). Consider making the checks the same for both. (It seems not strictly necessary to require that it be >0 as this simply means there will be no incentive, and the difference in incentive between 0 and 1 is minimal, and can always be easily changed, so this requirement might be dispensed with.)


## [NC-02] Refactor oracle price
Oracle.sol returns `normalizedPrice` from `Oracle.viewPrice()` and `Oracle.getPrice()` in `36 - tokenDecimals` decimals (see [Oracle.sol#L82-L88](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L82-L88) for the calculation).
Since it returns the price in DOLA, it seems unnecessarily confusing to not return this in `DOLA.decimals()` (18).
It seems better to compartmentalize the decimals such that these functions simply return in DOLA decimals, and wherever the value is used it's converted into the appropriate decimals there. 
This value is currently retrieved in Market.sol by [`getCollateralValue()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L315), [`getCollateralValueInternal()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L326), [`getWithdrawalLimitInternal()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L360), [`getWithdrawalLimit()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L377) and [`liquidate()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L596-L597). In all of these cases is the returned `normalizedPrice` divided by 18 decimals (`1 ether`) (which means the `36` could just have been `18` from the beginning) and gives the value of the (collateral) token in 18 decimals. This also means there is a hardcoded dependency on DOLA's having 18 decimals.
Consider refactoring by changing [Oracle.sol#L82-L88](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L82-L88):
```solidity
uint price = feeds[token].feed.latestAnswer();
require(price > 0, "Invalid feed price");
// normalize price
uint8 feedDecimals = feeds[token].feed.decimals();
uint8 tokenDecimals = feeds[token].tokenDecimals;
uint8 decimals = 36 - feedDecimals - tokenDecimals;
uint normalizedPrice = price * (10 ** decimals);
```
into
```solidity
uint price = feeds[token].feed.latestAnswer();
require(price > 0, "Invalid feed price");
// normalize price
uint8 feedDecimals = feeds[token].feed.decimals();
uint8 decimals = 18 - feedDecimals;
uint normalizedPrice = price * (10 ** decimals);
```
which returns `normalizedPrice` in 18 decimals.
And changing every occurrence of
`<collateral token> * oracle.[view|get](address(collateral), collateralFactorBps) / 1 ether`
(<collateral token decimals> + 36 - <collateral token decimals> - 18 = 18 decimals)
into
`<collateral token> * oracle.refactored[view|get]Price(address(collateral), collateralFactorBps) / 10**(<collateral token decimals>) {[*|/] 10**<optional to debt token decimals if other than 18>}`
(<collateral token decimals> + 18 - <collateral token decimals> = 18 decimals).

## [NC-03] Refactor price calculation in Oracle.sol
[`Oracle.viewPrice()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L78-L105) really just returns the minimum of `dampenedPrice` and `normalizedPrice`, except if `twoDayLow == 0` in which case it returns normalizedPrice. Consider the below steps to refactor to make this clearer. The same is applicable to `getPrice()` at [L134-L140](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L134-L140).

```solidity
95: uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
96: uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
97: if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
98:    uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
99:    return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
100: }
101: return normalizedPrice;
```
Inline `newBorrowingPower` and move definition of `dampenedPrice` out of the if-clause:
```solidity
uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
if(twoDayLow > 0 && normalizedPrice * collateralFactorBps / 10000 > twoDayLow) {
    return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
}
return normalizedPrice;
```
`normalizedPrice * collateralFactorBps / 10000 > twoDayLow`
<=> `normalizedPrice > twoDayLow * 10000 / collateralFactorBps`
<=> `normalizedPrice > dampenedPrice`,
so we get
```solidity
uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
if(twoDayLow > 0 && normalizedPrice > dampenedPrice) {
    return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
}
return normalizedPrice;
```
which is the same as
```solidity
uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
return twoDayLow > 0 && normalizedPrice > dampenedPrice ? dampenedPrice: normalizedPrice;
```

## [NC-04] `Fed.getProfit()` may return int
[`Fed.getProfit()`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L120-L125) may as well return an int to indicate loss. That profit > 0 is already checked in `Fed.takeProfit()` at [L133] (https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L133).


## [NC-05] Typos
[addres -> address](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L36)
[oprator -> operator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L50)
[replen -> replenishment price](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L60)
[allowe -> allowed](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L87)
[th -> the](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L128)
[deb -> debt](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L129)
[replenishment price -> replenishment cost](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L321)
[dola -> DOLA](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L133)
[mus -> must](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L146)
[liqudation -> liquidation](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L181)
[liquidation factor -> liquidation incentive](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L191)
[od -> of](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L201)
[liquidateable -> liquidatable](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L575)
[Th -> The](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L589)
[user user -> user](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L589)
[chainlink -> Chainlink](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L48)
[chainlink -> Chainlink](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L50)