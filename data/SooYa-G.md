## Splitting `require()` that use `&&` saves gas
Instead of using the && operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement (saving 3 gas per &):
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512

## `<X> = <X> + <Y>` is more efficient than `<X> += <Y>`
Using operation `<X> = <X> + <Y>` is cheaper than `<X> += <Y>`. This also works for subtraction. So i recommend to change the line code below :
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L289
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L362
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L376
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360


## Add unchecked {} for subtractions where the operands cannot underflow because of a previous `require()` or `if`-statement
since checking for underflow is already done in previous `require()` or `if`-statement, unchecked the subtractions saves gas for the operation.
```require(a <= b); x = b - a``` => ```require(a <= b); unchecked { x = b - a }``` 
There are some instances for this issue:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L171-L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195-L196
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L373-L374

## Check for zero amount in transfer function saves a gas from wasted transaction
When making a transfer with a zero amount (which means we don't send anything), the transaction will still require gas. Therefore, adding a check before transferring will prevent wasted gas use.
There are some instances in this issue:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L278
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L531
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559


## Duplicated check for user should be refactored to a modifier or function
By refactored the `require()`/`revert()` to a modifier or function saves deployment gas
There are 2 instances for this issue:
- gov
``` require(msg.sender == gov, "ONLY GOVERNMENT"); ```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L58
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67

- chair
``` require(msg.sender == chair, "ONLY CHAIR"); ```
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104

