- [Gas](#gas)
    - [**1. Reduce boolean comparison**](#1-reduce-boolean-comparison)
    - [**2. Avoid compound assignment operator in state variables**](#2-avoid-compound-assignment-operator-in-state-variables)
        - [Total gas saved: **13 * 25 = 325**](#total-gas-saved-13--25--325)
    - [**3. Reduce the size of error messages Long revert Strings**](#3-reduce-the-size-of-error-messages-long-revert-strings)
        - [Total gas saved: **18 * 4 = 72**](#total-gas-saved-18--4--72)
    - [**4. Avoid unused returns**](#4-avoid-unused-returns)
    - [**5. delete optimization**](#5-delete-optimization)
        - [Total gas saved: **5 * 2 = 10**](#total-gas-saved-5--2--10)
    - [**6. Use the unchecked keyword**](#6-use-the-unchecked-keyword)
    - [**7. Remove unnecessary variables**](#7-remove-unnecessary-variables)
    - [**8. Avoid duplicate code**](#8-avoid-duplicate-code)
    - [**9. Gas saving using variable cache**](#9-gas-saving-using-variable-cache)
    - [**10. Avoid the call Market.predictEscrow**](#10-avoid-the-call-marketpredictescrow)

# Gas

## **1. Reduce boolean comparison**

Compare a boolean value using `== true` or `== false` instead of using the boolean value is more expensive.

`NOT` opcode it's cheaper than using `EQUAL` or `NOTEQUAL` when the value it's false, or just the value without `== true` when it's true, because it will use less opcodes inside the VM.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.16;

contract TesterA {
function testEqual(bool a) public view returns (bool) { return a == true; }
}

contract TesterB {
function testNot(bool a) public view returns (bool) { return a; }
}
```

Gas saving executing: **18 per entry for == true**

```
TesterA.testEqual:   21814
TesterB.testNot:     21796   
```

```javascript
pragma solidity 0.8.16;

contract TesterA {
function testEqual(bool a) public view returns (bool) { return a == false; }
}

contract TesterB {
function testNot(bool a) public view returns (bool) { return !a; }
}
```

Gas saving executing: **15 per entry for == false**

```
TesterA.testEqual:   21814
TesterB.testNot:     21799
```

**Affected source code:**

Use the value instead of `!= true`:

- [DBR.sol:350](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L350)
- [Fed.sol:89](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L89)

## **2. Avoid compound assignment operator in state variables**

Using compound assignment operators for state variables (like `State += X` or `State -= X` ...) it's more expensive than using operator assignment (like `State = State + X` or `State = State - X` ...).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
uint private _a;
function testShort() public {
_a += 1;
}
}

contract TesterB {
uint private _a;
function testLong() public {
_a = _a + 1;
}
}
```

Gas saving executing: **13 per entry**

```
TesterA.testShort: 43507
TesterB.testLong:  43494
```

**Affected source code:**

- [DBR.sol:172](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L172)
- [DBR.sol:174](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L174)
- [DBR.sol:196](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L196)
- [DBR.sol:198](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L198)
- [DBR.sol:288](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L288)
- [DBR.sol:289](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L289)
- [DBR.sol:304](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L304)
- [DBR.sol:316](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L316)
- [DBR.sol:332](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L332)
- [DBR.sol:360](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360)
- [DBR.sol:362](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L362)
- [DBR.sol:374](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L374)
- [DBR.sol:376](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L376)
- [Fed.sol:91](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L91)
- [Fed.sol:92](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L92)
- [Fed.sol:110](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L110)
- [Fed.sol:111](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L111)
- [Market.sol:395](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L395)
- [Market.sol:397](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L397)
- [Market.sol:534](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L534)
- [Market.sol:535](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L535)
- [Market.sol:565](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L565)
- [Market.sol:568](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L568)
- [Market.sol:599](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L599)
- [Market.sol:600](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L600)

### Total gas saved: **13 * 25 = 325**

## **3. Reduce the size of error messages (Long revert Strings)**

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testShortRevert(bool path) public view {
require(path, "test error");
}
}

contract TesterB {
function testLongRevert(bool path) public view {
require(path, "test big error message, more than 32 bytes");
}
}
```

Gas saving executing: **18 per entry**

```
TesterA.testShortRevert: 21886
TesterB.testLongRevert:  21904
```

**Affected source code:**

- [DBR.sol:63](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L63)
- [DBR.sol:326](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L326)
- [Market.sol:76](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L76)
- [Market.sol:214](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L214)

### Total gas saved: **18 * 4 = 72**

## **4. Avoid unused returns**

Having a method that always returns the same value is not correct in terms of consumption, if you want to modify a value, and the method will perform a `revert` in case of failure, it is not necessary to return a `true`, since it will never be `false`. It is less expensive not to return anything, and it also eliminates the need to double-check the returned value by the caller.

**Affected source code:**

- [DBR.sol:158-162](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L158-L162)
- [DBR.sol:170-178](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170-L178)
- [DBR.sol:188-202](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188-L202)

## **5. `delete` optimization**

Use `delete` instead of set to default value (`false` or `0`).

5 gas could be saved per entry in the following affected lines:

**Affected source code:**

- [BorrowController.sol:38](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)
- [DBR.sol:91](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L91)

### Total gas saved: **5 * 2 = 10**

## **6. Use the `unchecked` keyword**

When an underflow or overflow cannot occur, one might conserve gas by using the `unchecked` keyword to prevent unnecessary arithmetic underflow/overflow tests.

- The subtraction has already been checked to be safe before.

```diff
    function contraction(IMarket market, uint amount) public {
        require(msg.sender == chair, "ONLY CHAIR");
        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
        uint supply = supplies[market];
        require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
        market.recall(amount);
        dola.burn(amount);
+       unckeched {
        supplies[market] -= amount;
        globalSupply -= amount;
+       }
        emit Contraction(market, amount);
    }
```

- The subtraction has already been checked to be safe before.

```diff
    function getProfit(IMarket market) public view returns (uint) {
        uint marketValue = dola.balanceOf(address(market)) + market.totalDebt();
        uint supply = supplies[market];
        if(supply >= marketValue) return 0;
+       unckeched {
        return marketValue - supply;
+       }
    }
```

- If `globalSupply` don't overflow, `supplies` also won't overflow.

```diff
    function expansion(IMarket market, uint amount) public {
        require(msg.sender == chair, "ONLY CHAIR");
        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
        require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
        dola.mint(address(market), amount);
+       globalSupply += amount;
+       unckeched {
        supplies[market] += amount;
+       }
-       globalSupply += amount;
        require(globalSupply <= supplyCeiling);
        emit Expansion(market, amount);
    }
```

- The subtraction has already been checked to be safe before.

```diff
    function totalSupply() public view returns (uint) {
-       if(totalDueTokensAccrued > _totalSupply) return 0;
+       if(totalDueTokensAccrued >= _totalSupply) return 0;
+       unckeched {
        return _totalSupply - totalDueTokensAccrued;
+       }
    }
```

- The subtraction has already been checked to be safe before, also it could be saved some math operations caching with a variable.

```diff
    function balanceOf(address user) public view returns (uint) {
        uint debt = debts[user];
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
-       if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
-       return balances[user] - dueTokensAccrued[user] - accrued;
+       accrued += dueTokensAccrued[user];
+       if(accrued >= balances[user]) return 0;
+       unckeched {
+           return balances[user] - accrued;
+       }
    }
```

- The subtraction has already been checked to be safe before, also it could be saved some math operations caching with a variable.

```diff
    function deficitOf(address user) public view returns (uint) {
        uint debt = debts[user];
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
-       if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
-       return dueTokensAccrued[user] + accrued - balances[user];
+       accrued += dueTokensAccrued[user];
+       if(accrued <= balances[user]) return 0;
+       unckeched {
+           return accrued - balances[user];
+       }
    }
```

- If `totalDueTokensAccrued` don't overflow, `dueTokensAccrued` also won't overflow.

```diff
    function accrueDueTokens(address user) public {
        uint debt = debts[user];
        if(lastUpdated[user] == block.timestamp) return;
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+       totalDueTokensAccrued += accrued;
+       unckeched {
        dueTokensAccrued[user] += accrued;
+       }
-       totalDueTokensAccrued += accrued;
        lastUpdated[user] = block.timestamp;
        emit Transfer(user, address(0), accrued);
    }
```

- The subtraction has already been checked to be safe before, also it could be saved some math operations caching with a variable.

```diff
    function getWithdrawalLimitInternal(address user) internal returns (uint) {
        IEscrow escrow = predictEscrow(user);
        uint collateralBalance = escrow.balance();
        if(collateralBalance == 0) return 0;
        uint debt = debts[user];
        if(debt == 0) return collateralBalance;
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
        if(collateralBalance <= minimumCollateral) return 0;
+       unckeched {
        return collateralBalance - minimumCollateral;
+       }
    }
```

- The subtraction has already been checked to be safe before, also it could be saved some math operations caching with a variable.

```diff
    function getWithdrawalLimit(address user) public view returns (uint) {
        IEscrow escrow = predictEscrow(user);
        uint collateralBalance = escrow.balance();
        if(collateralBalance == 0) return 0;
        uint debt = debts[user];
        if(debt == 0) return collateralBalance;
        if(collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
        if(collateralBalance <= minimumCollateral) return 0;
+       unckeched {
        return collateralBalance - minimumCollateral;
+       }
    }
```

- If `debts[borrower]` don't overflow, `totalDebt` also won't overflow.

```diff
    function borrowInternal(address borrower, address to, uint amount) internal {
        require(!borrowPaused, "Borrowing is paused");
        if(borrowController != IBorrowController(address(0))) {
            require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
        }
        uint credit = getCreditLimitInternal(borrower);
        debts[borrower] += amount;
        require(credit >= debts[borrower], "Exceeded credit limit");
+       unckeched {
        totalDebt += amount;
+       }
        dbr.onBorrow(borrower, amount);
        dola.transfer(to, amount);
        emit Borrow(borrower, amount);
    }
```

- The subtraction has already been checked to be safe before, also it could be saved some math operations caching with a variable.

```diff
    function repay(address user, uint amount) public {
-       uint debt = debts[user];
-       require(debt >= amount, "Insufficient debt");
+       require(debts[user] >= amount, "Insufficient debt");
+       unckeched {
        debts[user] -= amount;
        totalDebt -= amount;
+       }
        dbr.onRepay(user, amount);
        dola.transferFrom(msg.sender, address(this), amount);
        emit Repay(user, msg.sender, amount);
    }
```

- The subtraction has already been checked to be safe before, also it could be saved some math operations caching with a variable.

```diff
    function liquidate(address user, uint repaidDebt) public {
        require(repaidDebt > 0, "Must repay positive debt");
        uint debt = debts[user];
        require(getCreditLimitInternal(user) < debt, "User debt is healthy");
        require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
        uint price = oracle.getPrice(address(collateral), collateralFactorBps);
        uint liquidatorReward = repaidDebt * 1 ether / price;
        liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
+       unckeched {
        debts[user] -= repaidDebt;
        totalDebt -= repaidDebt;
+       }
        dbr.onRepay(user, repaidDebt);
        dola.transferFrom(msg.sender, address(this), repaidDebt);
        IEscrow escrow = predictEscrow(user);
        escrow.pay(msg.sender, liquidatorReward);
        if(liquidationFeeBps > 0) {
            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
            if(escrow.balance() >= liquidationFee) {
                escrow.pay(gov, liquidationFee);
            }
        }
        emit Liquidate(user, msg.sender, repaidDebt, liquidatorReward);
    }
```

**Affected source code:**

- [Fed.sol:103-113](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L103-L113)
- [Fed.sol:124](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L124)
- [Fed.sol:91-92](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L91-L92)
- [DBR.sol:109-112](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L109-L112)
- [DBR.sol:120-125](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L120-L125)
- [DBR.sol:133-138](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L133-L138)
- [DBR.sol:284-292](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L284-L292)
- [Market.sol:362](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L362)
- [Market.sol:379](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L379)
- [Market.sol:397](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L397)
- [Market.sol:531-539](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L531-L539)
- [Market.sol:599-600](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L599-L600)

## **7. Remove unnecessary variables**

The following state variables can be removed without affecting the logic of the contract since they are not used and/or are redundant because they could be used inline.

```diff
    function contraction(IMarket market, uint amount) public {
        require(msg.sender == chair, "ONLY CHAIR");
        require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
-       uint supply = supplies[market];
-       require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
+       require(amount <= supplies[market], "AMOUNT TOO BIG"); // can't burn profits
        market.recall(amount);
        dola.burn(amount);
        supplies[market] -= amount;
        globalSupply -= amount;
        emit Contraction(market, amount);
    }
```

**Affected source code:**

- [Fed.sol:106-107](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L106-L107)

## **8. Avoid duplicate code**

The `viewPrice` and `getPrice` methods of the `Oracle` contract are very similar, the only difference being the following peace of code:

```js
            if(todaysLow == 0 || normalizedPrice < todaysLow) {
                dailyLows[token][day] = normalizedPrice;
                todaysLow = normalizedPrice;
                emit RecordDailyLow(token, normalizedPrice);
            }
```

It's recommended to reuse the code in order to be more readable and smaller for deployment.

**Affected source code:**

- [Oracle.sol:126-130](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L126-L130)
- [Oracle.sol:79-103](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L79-L103)

## **9. Gas saving using variable cache**

```diff
    function getEscrow(address user) internal returns (IEscrow) {
+       IEscrow escrow = escrows[user];
+       if(escrow != IEscrow(address(0))) return escrow;
-       if(escrows[user] != IEscrow(address(0))) return escrows[user];
-       IEscrow escrow = createEscrow(user);
+       escrow = createEscrow(user);
+       escrows[user] = escrow;
        escrow.initialize(collateral, user);
-       escrows[user] = escrow;
        return escrow;
    }
```

**Affected source code:**

- [Market.sol:245-251](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L245-L251)

## **10. Avoid the call `Market.predictEscrow`**

The call to `predictEscrow` supposes an unnecessary expense in the following methods, this is because in the case of not existing in the mapping `escrows[user]` will not have been created yet, and the `balance` or `pay` methods cannot be called if was not created.

It's recommended to use the stored value `escrows[user]`.

**Affected source code:**

- [Market.sol:313](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L313)
- [Market.sol:324](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L324)
- [Market.sol:354](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L354)
- [Market.sol:371](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L371)
- [Market.sol:603](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L603)
