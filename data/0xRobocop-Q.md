The next statements on the contract Market.sol, don't follow the recommended best practice to do multiplications first and then dividing:

[LoC 360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360)

[LoC 377](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377)

In the case of these two, they are used in the functions to calculate the witdrawalLimit of an account, the following code snippet is taken from that functions:

```
uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
if(collateralBalance <= minimumCollateral) return 0;
return collateralBalance - minimumCollateral;

```

If  `minimumCollateral` is calculated wrong then the contract will allow a user to withdraw more collateral than the expected. So its better to follow best practices when doing multiplications and divisions.

The next one, also did not follow the same rule, but its impact its only on the calculation of `liquidationFee`:

[LoC 606](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606)