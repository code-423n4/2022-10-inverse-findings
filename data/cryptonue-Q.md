# [L] No two way transfer on setting Governance `setGov()`

In `Market.sol` there is a modifier named `onlyGov` which control the market crucial function. To update this `onlyGov` address, there is only one function `setGov()` without two step transfer pattern. This can cause a problem if the function is being set with wrong gov address because it can't revert the value. Consider to use two step transfer patter for this kind of `ownership` function.

```
File: Market.sol
130:     function setGov(address _gov) public onlyGov { gov = _gov; }
```

# [L] Borrow function does not check if amount > 0
In `Market.sol` contract, `borrowInternal()` function, the amount value is not being check if it's greater than 0. Thus if function call with 0 amount, the transfer will process with 0 transfer token, which is a waste of gas, it's better to check condition and revert.
```
File: Market.sol
389:     function borrowInternal(address borrower, address to, uint amount) internal {
390:         require(!borrowPaused, "Borrowing is paused");
391:         if(borrowController != IBorrowController(address(0))) {
392:             require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
393:         }
394:         uint credit = getCreditLimitInternal(borrower);
395:         debts[borrower] += amount;
396:         require(credit >= debts[borrower], "Exceeded credit limit");
397:         totalDebt += amount;
398:         dbr.onBorrow(borrower, amount);
399:         dola.transfer(to, amount);
400:         emit Borrow(borrower, amount);
401:     }
```

# [NC] Open TODO comment

For production, there should not be any `TODO` left on the code
```
File: INVEscrow.sol
34:     constructor(IXINV _xINV) {
35:         xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
36:     }
```

# [NC] Inconsistency of using uint vs uint256
Even though both is similar but it's good to have standard for quality code
example:
```
File: DBR.sol
14:     uint256 public _totalSupply;
15:     address public operator;
16:     address public pendingOperator;
17:     uint public totalDueTokensAccrued;
18:     uint public replenishmentPriceBps;

File: DBR.sol
121:         uint debt = debts[user];
122:         uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```

# [NC] Maximum line length is 120 characters
There are some of code > 120 characters per line in (Market.sol, Oracle.sol)

