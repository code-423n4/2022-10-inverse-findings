`require()` and `revert()` Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (**2100 gas**) in a function that may ultimately revert in the unhappy case.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L303