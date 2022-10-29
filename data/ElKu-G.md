 # Gas Optimizations

## Summary Of Findings:

|  | Issue | *Total Average Gas Saved
-- | -- | --  
1 | Optimizations in `Market.sol` | 5506 
2 | Optimizations in `DBR.sol` | 3205
3 | Optimizations in `Fed.sol` | 4720
4 | Use `bytes32` instead of `string` whenever possible | 295*2 = 590
5 | mapping's of similar type could be combined to turn into a mapping to a struct | 16*3 = 48

 *Total Average Gas Saved* - This simply means after making the mitigations, the forge gas report was compared with non-optimized version and the difference in average gas was added together for all affected functions. 

## Detailed Report on Gas Optimization Findings:

### 1. <ins>Optimizations in `Market.sol`</ins>

In [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol), the optimizations were centered around the following facts:
1. [predictEscrow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L292-L306) method was introduced to save gas when getting the escrow address. It was said that this saves gas compared to reading directly from the mapping. This was found to be not true. 
2. Its better to read the `escrow` address from mapping instead of using [getEscrow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L245-L251) in [withdrawInternal](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L463) method. As, if the escrow hasn't created yet, the next statements would cause a revert anyway.
3. Use unchecked arithmetic, whenever we are sure that the particular arithmetic operation wont cause an underflow or overflow. 
4. A `public` function which is never accessed from inside the contract should be declared as `external` to save gas.
5. If a state variable is accessed more than once in a method, its good to cache its value into a memory variable. As reads from memory costs 3 gas while storage reads cost 100 gas.

The following mitigations were performed in `Market.sol` to test this:

```diff
diff --git "a/.\\2022-10-inverse_original\\src\\Market.sol" "b/.\\2022-10-inverse_gas\\src\\Market.sol"
index 9585b85..9c252c3 100644
--- "a/.\\2022-10-inverse_original\\src\\Market.sol"
+++ "b/.\\2022-10-inverse_gas\\src\\Market.sol"
@@ -310,7 +310,7 @@ contract Market {
     @param user Address of the user.
     */
     function getCollateralValue(address user) public view returns (uint) {
-        IEscrow escrow = predictEscrow(user);
+        IEscrow escrow = escrows[user];
         uint collateralBalance = escrow.balance();
         return collateralBalance * oracle.viewPrice(address(collateral), collateralFactorBps) / 1 ether;
     }
@@ -321,7 +321,7 @@ contract Market {
     @param user Address of the user.
     */
     function getCollateralValueInternal(address user) internal returns (uint) {
-        IEscrow escrow = predictEscrow(user);
+        IEscrow escrow = escrows[user];
         uint collateralBalance = escrow.balance();
         return collateralBalance * oracle.getPrice(address(collateral), collateralFactorBps) / 1 ether;
     }
@@ -351,7 +351,7 @@ contract Market {
     @param user Address of the user.
     */
     function getWithdrawalLimitInternal(address user) internal returns (uint) {
-        IEscrow escrow = predictEscrow(user);
+        IEscrow escrow = escrows[user];  //@audit reading from mapping saves gas
         uint collateralBalance = escrow.balance();
         if(collateralBalance == 0) return 0;
         uint debt = debts[user];
@@ -359,7 +359,9 @@ contract Market {
         if(collateralFactorBps == 0) return 0;
         uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
         if(collateralBalance <= minimumCollateral) return 0;
-        return collateralBalance - minimumCollateral;
+        unchecked { //@audit this will never underflow because of `if` condition
+            return collateralBalance - minimumCollateral;
+        }
     }
 
     /**
@@ -367,16 +369,20 @@ contract Market {
      The withdrawal limit is how much collateral a user can withdraw before their loan would be underwater.
     @param user Address of the user.
     */
-    function getWithdrawalLimit(address user) public view returns (uint) {
-        IEscrow escrow = predictEscrow(user);
+    //@audit a function which is not called internally can be declared as `external`.
+    function getWithdrawalLimit(address user) external view returns (uint) {  
+        IEscrow escrow = escrows[user];
         uint collateralBalance = escrow.balance();
         if(collateralBalance == 0) return 0;
         uint debt = debts[user];
         if(debt == 0) return collateralBalance;
-        if(collateralFactorBps == 0) return 0;
-        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
+        uint collateralFactorBpsCached = collateralFactorBps;  //@audit cache state variables which are repeatedly read
+        if(collateralFactorBpsCached == 0) return 0;  //@audit use cache value here and the line below
+        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBpsCached) * 10000 / collateralFactorBpsCached;  
         if(collateralBalance <= minimumCollateral) return 0;
-        return collateralBalance - minimumCollateral;
+        unchecked { //@audit this will never underflow because of `if` condition
+            return collateralBalance - minimumCollateral;
+        }
     }
 
     /**
@@ -392,11 +398,13 @@ contract Market {
             require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
         }
         uint credit = getCreditLimitInternal(borrower);
-        debts[borrower] += amount;
-        require(credit >= debts[borrower], "Exceeded credit limit");
+        uint debt = debts[borrower];   //@audit cache state variable
+        debt += amount;  //@audit use cached state variable in arithmetic operation
+        require(credit >= debt, "Exceeded credit limit");  //@audit use cached state variable in comparison
         totalDebt += amount;
         dbr.onBorrow(borrower, amount);
         dola.transfer(to, amount);
+        debts[borrower] = debt;  //@audit write back the new value to state variable. 
         emit Borrow(borrower, amount);
     }
 
@@ -460,7 +468,7 @@ contract Market {
     function withdrawInternal(address from, address to, uint amount) internal {
         uint limit = getWithdrawalLimitInternal(from);
         require(limit >= amount, "Insufficient withdrawal limit");
-        IEscrow escrow = getEscrow(from);
+        IEscrow escrow = escrows[from]; //@audit read from mapping directly
         escrow.pay(to, amount);
         emit Withdraw(from, to, amount);
     }
@@ -562,12 +570,14 @@ contract Market {
         require(deficit >= amount, "Amount > deficit");
         uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
         uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;
-        debts[user] += replenishmentCost;
+        uint debt = debts[user];  //@audit cache state variable
+        debt += replenishmentCost;  //@audit use cached state variable for addition
         uint collateralValue = getCollateralValueInternal(user);
-        require(collateralValue >= debts[user], "Exceeded collateral value");
+        require(collateralValue >= debt, "Exceeded collateral value"); //@audit use cached state variable for comparison
         totalDebt += replenishmentCost;
         dbr.onForceReplenish(user, amount);
         dola.transfer(msg.sender, replenisherReward);
+        debts[user] = debt; //@audit write back the new value to state variable
         emit ForceReplenish(user, msg.sender, amount, replenishmentCost, replenisherReward);
     }
 
@@ -596,11 +606,11 @@ contract Market {
         uint price = oracle.getPrice(address(collateral), collateralFactorBps);
         uint liquidatorReward = repaidDebt * 1 ether / price;
         liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
-        debts[user] -= repaidDebt;
+        debts[user] = debt - repaidDebt;  //@audit use already cached value here for subtraction
         totalDebt -= repaidDebt;
         dbr.onRepay(user, repaidDebt);
         dola.transferFrom(msg.sender, address(this), repaidDebt);
-        IEscrow escrow = predictEscrow(user);
+        IEscrow escrow = escrows[user];   //read directly from the mapping
         escrow.pay(msg.sender, liquidatorReward);
         if(liquidationFeeBps > 0) {
             uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
```

The following methods showed gas saving when pre and post optimized gas reports from forge was compared:

|  | Method | Avg Gas usage Before | Avg Gas usage After | Gas Saved
-- | -- | -- | -- | --
1 | borrow | 160804 |  156344 | 4460
2 | borrowOnBehalf  | 71814 | 71750 | 64
3 | depositAndBorrow  | 280836 | 280646 | 190
4 | forceReplenish   | 63401 | 63163| 238
5 | getCreditLimit  | 7722 | 7659 | 63
6 | getLiquidatableDebt  | 6965 | 6923 | 42
7 | getWithdrawalLimit   | 4119 | 4033 | 86
8 | liquidate   | 7867 | 7819 | 48
9 | withdraw  | 21261 | 21033 | 228
10 | withdrawOnBehalf | 17996 | 17909 | 87

Total Gas Saved = **5506**.

### 2. <ins>Optimizations in `DBR.sol`</ins>

In [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol), the optimizations were centered around the following facts:

1. Use unchecked arithmetic, whenever we are sure that the particular arithmetic operation wont cause an underflow or overflow. 
2. If a state variable is accessed more than once in a method, its good to cache its value into a memory variable. As reads from memory costs 3 gas while storage reads cost 100 gas.
3. Emit cached memory variables instead of storage variables whenever possible. 

The following mitigations were performed in `DBR.sol` to test this:

```diff
diff --git "a/.\\2022-10-inverse_original\\src\\DBR.sol" "b/.\\2022-10-inverse_gas\\src\\DBR.sol"
index aab6daf..a9129bc 100644
--- "a/.\\2022-10-inverse_original\\src\\DBR.sol"
+++ "b/.\\2022-10-inverse_gas\\src\\DBR.sol"
@@ -68,10 +68,11 @@ contract DolaBorrowingRights {
     @notice claims the Operator role if set as pending operator.
     */
     function claimOperator() public {
-        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
-        operator = pendingOperator;
+        address pendingOperatorCached = pendingOperator;  //@audit cache pendingOperator which is accessed multiple times 
+        require(msg.sender == pendingOperatorCached, "ONLY PENDING OPERATOR"); //@audit use cached value here
+        operator = pendingOperatorCached; //@audit use cached value here
         pendingOperator = address(0);
-        emit ChangeOperator(operator);
+        emit ChangeOperator(pendingOperatorCached); //@audit use cached value in the emit statement
     }
 
     /**
@@ -107,8 +108,13 @@ contract DolaBorrowingRights {
     @return uint representing the total supply of DBR.
     */
     function totalSupply() public view returns (uint) {
-        if(totalDueTokensAccrued > _totalSupply) return 0;
-        return _totalSupply - totalDueTokensAccrued;
+        // Caching because we assume that mostly total DBR minted will be greater than total DBR accrued.
+        uint totalDueTokensAccruedCached = totalDueTokensAccrued; //@audit cache state variable
+        uint _totalSupplyCached = _totalSupply;   //@audit cache state variable
+        if(totalDueTokensAccruedCached > _totalSupplyCached) return 0;   //@audit use cached value
+        unchecked {   //@audit use unchecked block as the above `if` makes sure it wont underflow
+            return _totalSupplyCached - totalDueTokensAccruedCached;  //@audit use cached value
+        }
     }
 
     /**
@@ -120,8 +126,11 @@ contract DolaBorrowingRights {
     function balanceOf(address user) public view returns (uint) {
         uint debt = debts[user];
         uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
-        if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
-        return balances[user] - dueTokensAccrued[user] - accrued;
+        uint sum = dueTokensAccrued[user] + accrued;  //@audit caches the addition and the state variable in one go.
+        if(sum > balances[user]) return 0; //@audit use cached value here
+        unchecked{  //@audit use unchecked block as the above `if` makes sure it wont underflow
+            return balances[user] - sum;  //@audit subtract cached value here.
+        }
     }
 
     /**
@@ -133,8 +142,12 @@ contract DolaBorrowingRights {
     function deficitOf(address user) public view returns (uint) {
         uint debt = debts[user];
         uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
-        if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
-        return dueTokensAccrued[user] + accrued - balances[user];
+        uint sum = dueTokensAccrued[user] + accrued;  //@audit caches the addition and the state variable in one go.
+        uint balance = balances[user];   //@audit caches the state variable
+        if(sum < balance) return 0;  //@audit use cached value here
+        unchecked {  //@audit use unchecked block as the above `if` makes sure it wont underflow
+            return sum - balance;    //@audit subtract cached value here.
+        }
     }
     
     /**
@@ -283,8 +296,9 @@ contract DolaBorrowingRights {
     */
     function accrueDueTokens(address user) public {
         uint debt = debts[user];
-        if(lastUpdated[user] == block.timestamp) return;
-        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+        uint lastUpdatedCached = lastUpdated[user];  //@audit cache state variable
+        if(lastUpdatedCached == block.timestamp) return;  //@audit use caches value
+        uint accrued = (block.timestamp - lastUpdatedCached) * debt / 365 days; //@audit use caches value
         dueTokensAccrued[user] += accrued;
         totalDueTokensAccrued += accrued;
         lastUpdated[user] = block.timestamp;
```

The following methods showed gas saving when pre and post optimized gas reports from forge was compared:

#### Gas savings in DBR.sol:
|  | Method | Avg Gas usage Before | Avg Gas usage After | Gas Saved
-- | -- | -- | -- | --
1 | accrueDueTokens | 39313 |  39122 | 191
2 | balanceOf  | 3920 |  3689 | 231
3 | burn | 5796 |  5562 | 234
4 | deficitOf  | 2817 |  2469 | 348
5 | onBorrow  | 53059 |  52895 | 164
6 | onForceReplenish  | 33401 |  32968 | 433
7 | totalSupply  | 1688 |  1499 | 189
8 | transfer  | 16391 |  16130 | 261
9 | transferFrom | 9938 |  9799 | 139

#### Gas savings in Market.sol:
|  | Method | Avg Gas usage Before | Avg Gas usage After | Gas Saved
-- | -- | -- | -- | --
1 | borrow  | 160804 |  160651 | 153
2 | borrowOnBehalf  | 71814 |  71763 | 51
3 | depositAndBorrow | 280836 |  280684 | 152
4 | forceReplenish  | 63401 |  62742 | 659

Total Gas Saved = Gas savings in `DBR.sol` + Gas savings in `Market.sol` = 2190 + 1015 = **3205**.

### 3. <ins>Optimizations in `Fed.sol`</ins>

In [Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol), the optimizations were centered around the following facts:

1. Use unchecked arithmetic, whenever we are sure that the particular arithmetic operation wont cause an underflow or overflow. 
2. If a state variable is accessed more than once in a method, its good to cache its value into a memory variable. As reads from memory costs 3 gas while storage reads cost 100 gas. 

The following mitigations were performed in `Fed.sol` to test this:

```diff
diff --git "a/.\\2022-10-inverse_original\\src\\Fed.sol" "b/.\\2022-10-inverse_gas\\src\\Fed.sol"
index 1e819bb..ea5ecea 100644
--- "a/.\\2022-10-inverse_original\\src\\Fed.sol"
+++ "b/.\\2022-10-inverse_gas\\src\\Fed.sol"
@@ -88,9 +88,13 @@ contract Fed {
         require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
         require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
         dola.mint(address(market), amount);
-        supplies[market] += amount;
-        globalSupply += amount;
-        require(globalSupply <= supplyCeiling);
+        uint globalSupplyCached = globalSupply;  //@audit cache state variable
+        globalSupplyCached += amount;  //@audit user arithmetic on cached value
+        require(globalSupplyCached <= supplyCeiling); //@audit use require early on as possible. Use cached value.
+        unchecked{  //@audit use unchecked arithmetic as globalSupplyCached would have overflown if the below would overflow.
+            supplies[market] += amount;
+        }
+        globalSupply = globalSupplyCached;  //@audit write back changed value into storage
         emit Expansion(market, amount);
     }
 
@@ -107,8 +111,10 @@ contract Fed {
         require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
         market.recall(amount);
         dola.burn(amount);
-        supplies[market] -= amount;
-        globalSupply -= amount;
+        supplies[market] = supply - amount;  //@audit use already cached value
+        unchecked { //@audit use unchecked arithmetic as supplies would have underflown if the below would underflow.
+            globalSupply -= amount;
+        }
         emit Contraction(market, amount);
     }
 
@@ -121,7 +127,9 @@ contract Fed {
         uint marketValue = dola.balanceOf(address(market)) + market.totalDebt();
         uint supply = supplies[market];
         if(supply >= marketValue) return 0;
-        return marketValue - supply;
+        unchecked {     //@audit use unchecked as the above condition makes sure that it will never underflow
+            return marketValue - supply;  
+        }
     }
 
     /**
```

The following methods showed gas saving when pre and post optimized gas reports from forge was compared:

#### Gas savings in DBR.sol:
|  | Method | Avg Gas usage Before | Avg Gas usage After | Gas Saved
-- | -- | -- | -- | --
1 | onBorrow  | 53059 | 52348 | 711

#### Gas savings in Fed.sol:
|  | Method | Avg Gas usage Before | Avg Gas usage After | Gas Saved
-- | -- | -- | -- | --
1 | contraction  | 13734 |  13674 | 60
2 | expansion  | 95081 |  94000 | 1081
3 | takeProfit | 25830 |  25804 | 26

#### Gas savings in Market.sol:
|  | Method | Avg Gas usage Before | Avg Gas usage After | Gas Saved
-- | -- | -- | -- | --
1 | borrow  | 160804 |  157962 | 2842

Total Gas Saved = Gas savings in `DBR.sol` + Gas savings in `Fed.sol` + Gas savings in `Market.sol` = 711 + 1167 + 2842= **4720**.

### 4. <ins>Use `bytes32` instead of `string` whenever possible</ins>

For savings character strings, `bytes32` could be used instead of `string` type if the character array has less than 32 characters. This [simple test in remix](https://gist.github.com/El-Ku/9a1fc60ff58d776854f44b966b842668) shows that when deploying, 25000 gas could be saved and the execution cost of getter function could be reduced by 295 gas. 

There are two instances of this in [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol):
```solidity
Line 11:    string public name;
Line 12:    string public symbol;
```

### 5. <ins>mapping's of similar type could be combined to turn into a mapping to a struct</ins>

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol) has the following three mappings, which maps from `user address` to their `debts`, `dueTokensAccrued` and `lastUpdated`. 

```solidity
    mapping (address => uint) public debts; // user => debt across all tracked markets
    mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
    mapping (address => uint) public lastUpdated; // user => last update timestamp
```

These can be combined into a structure and then used inside a mapping like this:
```solidity
    struct User{
        uint256 debts;
        uint256 dueTokensAccrued;
	uint256 lastUpdated;
    }
    mapping (address => User) public userInfo; 
```

This [simple remix test](https://gist.github.com/El-Ku/dee0e0abec1e60f23b0c48b80078d4f8) shows that we can save 16 gas per access of each struct field.

## Conclusions:

The items listed from 1 to 3 were mitigated and exact gas savings are tabulated to show their value. Items 4 and 5 needed more changes in the codebase, so I couldn't find out the exact gas saving, but their worthiness was proven with simple remix tests.

Items 1 to 3: Exact average gas saved              = 5506 + 3205 + 4720 = **13431**.
Items 4 to 5: Approximate average gas saved   = 590 + 48 = **638**.