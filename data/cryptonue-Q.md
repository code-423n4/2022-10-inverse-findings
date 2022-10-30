# [L] There is no two way transfer on setting Governance `setGov()` and setting Operator `setOperator()`

In `Market.sol` there is a modifier named `onlyGov` which control the market crucial function. To update this `onlyGov` address, there is only one function `setGov()` without two step transfer pattern. This can cause a problem if the function is being set with wrong gov address because it can't revert the value. Consider to use two step transfer patter for this kind of `ownership` function.

there is also an instance of this issue, `setOperator` inside `BorrowController.sol` contract
```
File: Market.sol
130:     function setGov(address _gov) public onlyGov { gov = _gov; }

File: BorrowController.sol
26:     function setOperator(address _operator) public onlyOperator { operator = _operator; }

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

# [L] No upper bound for `replenishmentPriceBps` in `DBR.sol` contract
```
File: DBR.sol
62:     function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
63:         require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
64:         replenishmentPriceBps = newReplenishmentPriceBps_;
65:     }
```

# [L] `Pay` function is not check for availability of token
In `GovTokenEscrow.sol` and `SimpleERC20Escrow.sol` contracts, the `pay()` function is not checking if the balance of the token inside the contract is enough to transfer based on the `amount` input. (Unlike INVEscrow.sol)
```
File: SimpleERC20Escrow.sol
36:     function pay(address recipient, uint amount) public {
37:         require(msg.sender == market, "ONLY MARKET");
38:         token.transfer(recipient, amount);
39:     }

File: GovTokenEscrow.sol
43:     function pay(address recipient, uint amount) public {
44:         require(msg.sender == market, "ONLY MARKET");
45:         token.transfer(recipient, amount);
46:     }

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

# [NC] No emit event on `setReplenishmentPriceBps`
It's best to emit an event if function change some settings