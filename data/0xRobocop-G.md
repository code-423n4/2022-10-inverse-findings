[GAS-1]

The functions [viewPrice](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L78) and [getPrice](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L112) from the Oracle.sol contract both contain the next if-clause:

```
if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
                return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
}
```

In order to the body of the if-clause to execute the two conditions needs to be true, so we that, we know that newBorrowingPower needs to be greater than twoDayLow.

Now we can focus on the ` return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;` statement. It returns the lowest of `dampenedPrice` and `normalizedPrice`. 

We can see in the if-clause that `dampenedPrice = twoDayLow * 10000 / collateralFactorBps;`

We can see that newBorrowingPower is calculated as `newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;` if we solve for `normalizedPrice` we get that `normalizedPrice = newBorrowingPower * 10000 / collateralFactorBps`. 

And because `newBorrowingPower > twoDayLow` is true in order of the body of the if-clause to execute, then we know than `dampenedPrice` will always be lower than `normalizedPrice`, so we can save the gas on doing the comparison between them.

