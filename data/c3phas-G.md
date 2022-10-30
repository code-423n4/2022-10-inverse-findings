## FINDINGS
NB: Some functions have been truncated where necessary to just show affected parts of the code.

In  the report some places might be denoted with audit tags to show the actual place affected.

### Note on Gas estimates.
I've tried to give the exact amount of gas being saved from running the included tests. Whenever the function is within the test coverage, the gas before and after will be included, and often a diff of the code will also accompany this.

Some functions are not covered by the test cases or are internal/private functions. In this case, the gas can be estimated by looking at the opcodes involved eg Number of SLOADs saved. Whenever this method is applied the gas saved is stated as an approximate ie `~` symbol will be used, and most importantly there will be no benchmarks for gas before and after. 

**For caching storage variables**
In some cases , the gas savings would only apply one way ie happy vs sad path and in this cases, no exact gas savings are given and no benchmarks for before and after. I've also separated this and put them under findings that gave weird values after caching even though they should save gas. 

The optimizer is set to run with the default runs(200).

### Tighly pack storage variables/optimize the order of variable declaration(Save 1 Storage slot)

Here, the storage variables can be tightly packed to save 1 storage SLOT from 14 SLOTS to 13 SLOTS. Helps avoid a Gsset of 20000 gas. 

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L38-L59
```solidity
File: /src/Market.sol

//@audit: Move borrowPaused next to address gov 
    address public gov;
    address public lender;
    address public pauseGuardian;
    address public immutable escrowImplementation;
    IDolaBorrowingRights public immutable dbr;
    IBorrowController public borrowController;
    IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
    IERC20 public immutable collateral;
    IOracle public oracle;
    uint public collateralFactorBps;
    uint public replenishmentIncentiveBps;
    uint public liquidationIncentiveBps;
    uint public liquidationFeeBps;
    uint public liquidationFactorBps = 5000; // 50% by default
    bool immutable callOnDepositCallback;
    bool public borrowPaused;
    uint public totalDebt;
    uint256 internal immutable INITIAL_CHAIN_ID;
    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
    mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce

```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..1a87801 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -34,7 +34,7 @@ interface IBorrowController {
 }

 contract Market {
-
+    bool public borrowPaused;
     address public gov;
     address public lender;
     address public pauseGuardian;
@@ -50,7 +50,6 @@ contract Market {
     uint public liquidationFeeBps;
     uint public liquidationFactorBps = 5000; // 50% by default
     bool immutable callOnDepositCallback;
-    bool public borrowPaused;
     uint public totalDebt;
     uint256 internal immutable INITIAL_CHAIN_ID;
     bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
```


## Use Short Circuiting rules to your advantage
The operators `||` and `&&` apply the common short-circuiting rules. This means that in the expression `f(x) || g(y)`, if `f(x)` evaluates to `true`, `g(y)` will not be evaluated even if it may have side-effects.
So setting less costly function to “f(x)” and setting costly function to “g(x)” is efficient.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L349-L352
### DBR.sol.mint(): reorder the checks (save 2097 gas on average)

|        | Min    | Average | Median   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before | 2823    | 48997   | 51000 | 51000 |
| After  | 2823    | 46900   | 48809 | 48809 |

```solidity
File: /src/DBR.sol
349:    function mint(address to, uint amount) public {
350:        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..b65d685 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -347,7 +347,7 @@ contract DolaBorrowingRights {
     @param amount Amount of DBR to mint.
     */
     function mint(address to, uint amount) public {
-        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
+        require(msg.sender == operator || minters[msg.sender] == true , "ONLY MINTERS OR OPERATOR");
         _mint(to, amount);
     }
```

### Use immutable on variables that are set and never changed after(2.1 K per variable per usage)

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L11-L12
```solidity
File: /src/DBR.sol
11:    string public name;
12:    string public symbol;
```

### Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L245-L251
### Market.sol.getEscrow(): escrows[user] should be cached(Saves ~100 gas)
```solidity
File: /src/Market.sol
245:    function getEscrow(address user) internal returns (IEscrow) {
246:        if(escrows[user] != IEscrow(address(0))) return escrows[user];

251:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..66eb94f 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -243,7 +243,8 @@ contract Market {
     @param user The address of the user owning the escrow.
     */
     function getEscrow(address user) internal returns (IEscrow) {
-        if(escrows[user] != IEscrow(address(0))) return escrows[user];
+        IEscrow _escrow = escrows[user];
+        if(_escrow  != IEscrow(address(0))) return _escrow ;

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L353-L363
### Market.sol.getWithdrawalLimitInternal(): collateralFactorBps should be cached(saves 1 SLOAD : ~94 gas)
```solidity
File: /src/Market.sol
353:    function getWithdrawalLimitInternal(address user) internal returns (uint) {

360:        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

363:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..5573ccc 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -357,7 +357,8 @@ contract Market {
         uint debt = debts[user];
         if(debt == 0) return collateralBalance;
         if(collateralFactorBps == 0) return 0;
-        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
+        uint _collateralFactorBps = collateralFactorBps;
+        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), _collateralFactorBps) * 10000 / _collateralFactorBps;
         if(collateralBalance <= minimumCollateral) return 0;
         return collateralBalance - minimumCollateral;
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L531-L539
### Market.sol.repay(): debts[user] should use the cached value(Saves 63 gas on average)

|        | Min    | Average | Median  | Max  |
| ------ | --- | ------- | ---- | ---- |
| Before |759     | 5192    | 7409 | 7409 |
| After  |759     | 5129    | 7315 | 7315 |
```solidity
File: /src/Market.sol
531:    function repay(address user, uint amount) public {
532:        uint debt = debts[user];
533:        require(debt >= amount, "Insufficient debt");
534:        debts[user] -= amount; //@audit: use cached value (debt)

539:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..077339d 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -531,7 +531,7 @@ contract Market {
     function repay(address user, uint amount) public {
         uint debt = debts[user];
         require(debt >= amount, "Insufficient debt");
-        debts[user] -= amount;
+        debts[user] = debt - amount;
         totalDebt -= amount;
         dbr.onRepay(user, amount);
         dola.transferFrom(msg.sender, address(this), amount);
```


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L86-L95
### Fed.sol.expansion(): globalSupply should be cached(saves 107 gas on average)

|        | Min    | Average | Median   | Max    |
| ------ | --- | ------- | ----- | ------ |
| Before | 657    | 95100   | 98247 | 113747 |
| After  | 657    | 94993   | 98136 | 113636 |

```solidity
File: /src/Fed.sol
86:    function expansion(IMarket market, uint amount) public {

92:        globalSupply += amount;
93:        require(globalSupply <= supplyCeiling);
94:        emit Expansion(market, amount);
95:    }
```

```diff
diff --git a/src/Fed.sol b/src/Fed.sol
index 1e819bb..50fa2e8 100644
--- a/src/Fed.sol
+++ b/src/Fed.sol
@@ -89,8 +89,10 @@ contract Fed {
         require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
         dola.mint(address(market), amount);
         supplies[market] += amount;
-        globalSupply += amount;
-        require(globalSupply <= supplyCeiling);
+        uint _globalSupply = globalSupply;
+        _globalSupply = _globalSupply + amount;
+        require(_globalSupply <= supplyCeiling);
+        globalSupply = _globalSupply;
         emit Expansion(market, amount);
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L103-L113
### Fed.sol.contraction(): supplies[market] use cached value(Saves 35 gas on average)

|        | Min    | Average | Median  | Max   |
| ------ | --- | ------- | ---- | ----- |
| Before | 614    | 13749   | 8152 | 29057 |
| After  | 614    | 13714   | 8152 | 28968 |

```solidity
File:/src/Fed.sol
103:    function contraction(IMarket market, uint amount) public {

106:        uint supply = supplies[market];

110:        supplies[market] -= amount;
```

```diff
diff --git a/src/Fed.sol b/src/Fed.sol
index 1e819bb..8dc253c 100644
--- a/src/Fed.sol
+++ b/src/Fed.sol
@@ -107,7 +107,7 @@ contract Fed {
         require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
         market.recall(amount);
         dola.burn(amount);
-        supplies[market] -= amount;
+        supplies[market] = supply - amount;
         globalSupply -= amount;
         emit Contraction(market, amount);
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L109-L112
### DBR.sol.totalSupply(): totalDueTokensAccrued and \_totalSupply shoud be cached(Saves  138 gas on average)

|        | Min    | Average | Median  | Max  |
| ------ | --- | ------- | ---- | ---- |
| Before | 483    | 1694    | 1676 | 2765 |
| After  | 502    | 1556    | 1575 | 2575 |

```solidity
File: /src/DBR.sol
109:    function totalSupply() public view returns (uint) {
110:        if(totalDueTokensAccrued > _totalSupply) return 0;
111:        return _totalSupply - totalDueTokensAccrued;
112:    }
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..ac00981 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -107,8 +107,10 @@ contract DolaBorrowingRights {
     @return uint representing the total supply of DBR.
     */
     function totalSupply() public view returns (uint) {
-        if(totalDueTokensAccrued > _totalSupply) return 0;
-        return _totalSupply - totalDueTokensAccrued;
+        uint _totalDueTokensAccrued = totalDueTokensAccrued;
+        uint tempTotalSupply = _totalSupply;
+        if(_totalDueTokensAccrued > tempTotalSupply) return 0;
+        return tempTotalSupply - _totalDueTokensAccrued;
     }

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L120-L125
### DBR.sol.balanceOf(): dueTokensAccrued[user] and balances[user] should be cached(saves 320 gas on average)
|        | Min    | Average | Median  | Max  |
| ------ | --- | ------- | ---- | ---- |
| Before |  1489   | 3948    | 1999 | 9999 |
| After  | 1493    | 3628    | 1644 | 9644 |

```solidity
File: /src/DBR.sol
120:    function balanceOf(address user) public view returns (uint) {

123:        if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
124:        return balances[user] - dueTokensAccrued[user] - accrued;
125:    }
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..bcbdb31 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -120,8 +120,10 @@ contract DolaBorrowingRights {
     function balanceOf(address user) public view returns (uint) {
         uint debt = debts[user];
         uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
-        if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
-        return balances[user] - dueTokensAccrued[user] - accrued;
+        uint _dueTokensAccrued = dueTokensAccrued[user];
+        uint _balances = balances[user];
+        if(_dueTokensAccrued + accrued > _balances) return 0;
+        return _balances - _dueTokensAccrued - accrued;
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L133-L138
### DBR.sol.deficitOf(): dueTokensAccrued[user] and balances[user] should be cached(Saves  275 gas on average)
|        | Min    | Average | Median  | Max   |
| ------ | --- | ------- | ---- | ----- |
| Before | 1555    | 2840    | 2065 | 10065 |
| After  |  1559   | 2565    | 1710 | 9710  |

```solidity
File: /src/DBR.sol
133:    function deficitOf(address user) public view returns (uint) {

136:        if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
137:        return dueTokensAccrued[user] + accrued - balances[user];
138:    }
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..91b3d6c 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -133,8 +133,10 @@ contract DolaBorrowingRights {
     function deficitOf(address user) public view returns (uint) {
         uint debt = debts[user];
         uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
-        if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
-        return dueTokensAccrued[user] + accrued - balances[user];
+        uint _dueTokensAccrued = dueTokensAccrued[user];
+        uint _balances = balances[user];
+        if(_dueTokensAccrued + accrued < _balances) return 0;
+        return _dueTokensAccrued + accrued - _balances;
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L284-L292
### DBR.sol.accrueDueTokens(): lastUpdated[user] should be cached(Saves 191 gas on average)

|        | Min    | Average | Median   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before | 25906    | 39331   | 43806 | 43806 |
| After  |  25715   | 39140   | 43615 | 43615 |

```solidity
File: /src/DBR.sol
284:    function accrueDueTokens(address user) public {
285:        uint debt = debts[user];
286:        if(lastUpdated[user] == block.timestamp) return;
287:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..c70fcd7 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -283,8 +283,9 @@ contract DolaBorrowingRights {
     */
     function accrueDueTokens(address user) public {
         uint debt = debts[user];
-        if(lastUpdated[user] == block.timestamp) return;
-        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+        uint _lastUpdated = lastUpdated[user];
+        if(_lastUpdated == block.timestamp) return;
+        uint accrued = (block.timestamp - _lastUpdated) * debt / 365 days;
         dueTokensAccrued[user] += accrued;
         totalDueTokensAccrued += accrued;
         lastUpdated[user] = block.timestamp;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L59-L64
### INVEscrow.sol.pay(): token should be cached(Saves 1 SLOAD ~97 gas) - not covered by the tests
```solidity
File: /src/escrows/INVEscrow.sol
59:    function pay(address recipient, uint amount) public {

61:        uint invBalance = token.balanceOf(address(this));

63:        token.transfer(recipient, amount);
64:    }
```

```diff
diff --git a/src/escrows/INVEscrow.sol b/src/escrows/INVEscrow.sol
index 376bb47..31476f7 100644
--- a/src/escrows/INVEscrow.sol
+++ b/src/escrows/INVEscrow.sol
@@ -58,9 +58,10 @@ contract INVEscrow {
     */
     function pay(address recipient, uint amount) public {
         require(msg.sender == market, "ONLY MARKET");
-        uint invBalance = token.balanceOf(address(this));
+         IERC20  _token = token;
+        uint invBalance = _token.balanceOf(address(this));
         if(invBalance < amount) xINV.redeemUnderlying(amount - invBalance); // we do not check return value because next call will fail if this fails anyway
-        token.transfer(recipient, amount);
+        _token.transfer(recipient, amount);
     }

```

## Logically should utilize caching but tests gave weird values after caching
**NB:** This can be excluded when working out the total gas saved but still worth pointing it out as in theory gas could be saved by implementing the changes stated. However running the tests with this changes either increased the cost or there were no changes. This could be as a result of savings dependent on what path we follow(happy vs sad)

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L591-L612
### Market.sol.liquidate(): debts[user] should use the cached value(save 1 SLOAD ~100 gas)

```solidity
File: /src/Market.sol
591:    function liquidate(address user, uint repaidDebt) public {
592:        require(repaidDebt > 0, "Must repay positive debt");
593:        uint debt = debts[user];

599:        debts[user] -= repaidDebt;

612:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..6128714 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -596,7 +596,7 @@ contract Market {
         uint price = oracle.getPrice(address(collateral), collateralFactorBps);
         uint liquidatorReward = repaidDebt * 1 ether / price;
         liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
-        debts[user] -= repaidDebt;
+        debts[user] = debt - repaidDebt;
         totalDebt -= repaidDebt;
         dbr.onRepay(user, repaidDebt);
         dola.transferFrom(msg.sender, address(this), repaidDebt);
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L370-L380
### Market.sol.getWithdrawalLimit(): collateralFactorBps should be cached(saves 1 SLOAD : ~94 gas) 
```solidity
File: /src/Market.sol
370:    function getWithdrawalLimit(address user) public view returns (uint) {

377:        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

380:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..284b3f9 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -374,7 +374,8 @@ contract Market {
         uint debt = debts[user];
         if(debt == 0) return collateralBalance;
         if(collateralFactorBps == 0) return 0;
-        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
+        uint _collateralFactorBps = collateralFactorBps;
+        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), _collateralFactorBps) * 10000 / _collateralFactorBps;
         if(collateralBalance <= minimumCollateral) return 0;
         return collateralBalance - minimumCollateral;
     }
```


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L389-L401
### Market.sol.borrowInternal(): borrowController should be cached(Saves ~94 gas)
```solidity
File: /src/Market.sol
389:    function borrowInternal(address borrower, address to, uint amount) internal {
390:        require(!borrowPaused, "Borrowing is paused");
391:        if(borrowController != IBorrowController(address(0))) {
392:            require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
393:        }

401:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..2a08459 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -388,8 +388,9 @@ contract Market {
     */
     function borrowInternal(address borrower, address to, uint amount) internal {
         require(!borrowPaused, "Borrowing is paused");
-        if(borrowController != IBorrowController(address(0))) {
-            require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
+         IBorrowController _borrowController = borrowController;
+        if(_borrowController != IBorrowController(address(0))) {
+            require(_borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
         }
         uint credit = getCreditLimitInternal(borrower);
         debts[borrower] += amount;
```



https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L591-L612
### Market.sol.liquidate(): liquidationFeeBps should be cached(Saves 1 SLOAD ~97 gas) - Happy path
```solidity
File: /src/Market.sol
591:    function liquidate(address user, uint repaidDebt) public {

605:        if(liquidationFeeBps > 0) {
606:            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
            
612:    }
```

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..caa0ec7 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -602,8 +602,9 @@ contract Market {
         dola.transferFrom(msg.sender, address(this), repaidDebt);
         IEscrow escrow = predictEscrow(user);
         escrow.pay(msg.sender, liquidatorReward);
-        if(liquidationFeeBps > 0) {
-            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
+        uint _liquidationFeeBps = liquidationFeeBps;
+        if(_liquidationFeeBps > 0) {
+            uint liquidationFee = repaidDebt * 1 ether / price * _liquidationFeeBps / 10000;
             if(escrow.balance() >= liquidationFee) {
                 escrow.pay(gov, liquidationFee);
             }
```


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L66-L71
### Oracle.sol.claimOperator(): pendingOperator should be cached
```solidity
File: /src/Oracle.sol
66:    function claimOperator() public {
67:        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
68:        operator = pendingOperator;

71:    }
```

```diff
diff --git a/src/Oracle.sol b/src/Oracle.sol
index 14338ed..c04d6c9 100644
--- a/src/Oracle.sol
+++ b/src/Oracle.sol
@@ -64,10 +64,12 @@ contract Oracle {
     @notice Claims the operator role. Only successfully callable by the pending operator.
     */
     function claimOperator() public {
-        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
-        operator = pendingOperator;
-        pendingOperator = address(0);
-        emit ChangeOperator(operator);
+        address _pendingOperator = pendingOperator;
+        require(msg.sender == _pendingOperator, "ONLY PENDING OPERATOR");
+
+        operator = _pendingOperator;
+        pendingOperator = address(0) ;
+        emit ChangeOperator(_pendingOperator);
     }

     /**
```


## Cached value used only once
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L120-L125
### DBR.sol.balanceOf(): no need to cache value here(save 8 gas)
|        | Min    | Average | Mid   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before |  1489   | 3948   | 1999 | 9999 |
| After  |  1481   | 3940   | 1991 | 9991 |

```solidity
File: /src/DBR.sol
120:    function balanceOf(address user) public view returns (uint) {
121:        uint debt = debts[user];
122:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..2e1daf4 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -118,8 +118,8 @@ contract DolaBorrowingRights {
     @return uint representing the balance of the user.
     */
     function balanceOf(address user) public view returns (uint) {
-        uint debt = debts[user];
-        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+        //uint debt = debts[user];
+        uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
         if(dueTokensAccrued[user] + accrued > balances[user]) return 0;
         return balances[user] - dueTokensAccrued[user] - accrued;
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L133-L138
### DBR.sol.deficitOf(): Save  8 gas by not caching
|        | Min    | Average | Mid   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before |  1555   | 2840   | 2065 | 10065 |
| After  |  1547   | 2832   | 2057 | 10057 |

```solidity
File: /src/DBR.sol
133:    function deficitOf(address user) public view returns (uint) {
134:        uint debt = debts[user];
135:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..01d7123 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -131,8 +131,7 @@ contract DolaBorrowingRights {
     @return uint representing the deficit of the user.
     */
     function deficitOf(address user) public view returns (uint) {
-        uint debt = debts[user];
-        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+        uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
         if(dueTokensAccrued[user] + accrued < balances[user]) return 0;
         return dueTokensAccrued[user] + accrued - balances[user];
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L146-L150
### DBR.sol.signedBalanceOf(): Save 8 gas by not caching

|        | Min    | Average | Mid   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before |  1662   | 1662   | 1662 | 1662 |
| After  |  1654   | 1654   | 1654 | 1654 |

```solidity
File: /src/DBR.sol
146:    function signedBalanceOf(address user) public view returns (int) {
147:        uint debt = debts[user];
148:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..ff203cf 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -144,8 +144,7 @@ contract DolaBorrowingRights {
     @return Returns a signed int of the user's balance
     */
     function signedBalanceOf(address user) public view returns (int) {
-        uint debt = debts[user];
-        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+        uint accrued = (block.timestamp - lastUpdated[user]) *  debts[user] / 365 days;
         return int(balances[user]) - int(dueTokensAccrued[user]) - int(accrued);
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L284-L292
### DBR.sol.accrueDueTokens(): Save 7 gas 
|        | Min    | Average | Mid   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before |  25906   | 39331   | 43806 | 43806 |
| After  |  25899   | 39324   | 43799 | 43799 |

```solidity
File: /src/DBR.sol
284:    function accrueDueTokens(address user) public {
285:        uint debt = debts[user];
286:        if(lastUpdated[user] == block.timestamp) return;
287:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
```

```diff
diff --git a/src/DBR.sol b/src/DBR.sol
index aab6daf..2ff50c3 100644
--- a/src/DBR.sol
+++ b/src/DBR.sol
@@ -282,9 +282,8 @@ contract DolaBorrowingRights {
     @param user The address of the user to accrue DBR debt to.
     */
     function accrueDueTokens(address user) public {
-        uint debt = debts[user];
         if(lastUpdated[user] == block.timestamp) return;
-        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+        uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
         dueTokensAccrued[user] += accrued;
         totalDueTokensAccrued += accrued;
         lastUpdated[user] = block.timestamp;
```

### Internal/Private functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Affected code:

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L226-L238
```solidity
File: /src/Market.sol
226:    function createEscrow(address user) internal returns (IEscrow instance) {

353:     function getWithdrawalLimitInternal(address user) internal returns (uint) {


```

### Multiple accesses of a mapping/array should use a local variable cache

Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times saves ~42 gas per access due to not having to perform the same offset calculation every time.
**Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it**

To help the optimizer,declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. 
As an example, instead of repeatedly calling ```someMap[someIndex]```, save its reference like this: ```SomeStruct storage someStruct = someMap[someIndex]``` and use it.


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L78-L105
### Oracle.sol.viewPrice(): feeds[token] should be cached(saves 167 gas on average )

|        | Min    | Average | Mid  | Max   |
| ------ | --- | ------- | ---- | ----- |
| Before | 816    | 5908    | 4160 | 18638 |
| After  |   816  | 5741    | 3925 | 18403 |

```solidity
File: /src/Oracle.sol
78:    function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
79:        if(fixedPrices[token] > 0) return fixedPrices[token];
80:        if(feeds[token].feed != IChainlinkFeed(address(0))) {
81:            // get price from feed
82:            uint price = feeds[token].feed.latestAnswer();
83:            require(price > 0, "Invalid feed price");
84:            // normalize price
85:            uint8 feedDecimals = feeds[token].feed.decimals();
86:            uint8 tokenDecimals = feeds[token].tokenDecimals;

105:    }
```

```diff
diff --git a/src/Oracle.sol b/src/Oracle.sol
index 14338ed..cd34a9d 100644
--- a/src/Oracle.sol
+++ b/src/Oracle.sol
@@ -77,13 +77,14 @@ contract Oracle {
     */
     function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
         if(fixedPrices[token] > 0) return fixedPrices[token];
+        FeedData  storage _feeds = feeds[token];
         if(feeds[token].feed != IChainlinkFeed(address(0))) {
             // get price from feed
-            uint price = feeds[token].feed.latestAnswer();
+            uint price = _feeds.feed.latestAnswer();
             require(price > 0, "Invalid feed price");
             // normalize price
-            uint8 feedDecimals = feeds[token].feed.decimals();
-            uint8 tokenDecimals = feeds[token].tokenDecimals;
+            uint8 feedDecimals = _feeds.feed.decimals();
+            uint8 tokenDecimals = _feeds.tokenDecimals;
             uint8 decimals = 36 - feedDecimals - tokenDecimals;
             uint normalizedPrice = price * (10 ** decimals);
             uint day = block.timestamp / 1 days;
```


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L112-L144
### Oracle.sol.getPrice(): feeds[token] should be cached in local storage(Saves 229 gas on average)
|        | Min    | Average | Midian   | Max   |
| ------ | --- | ------- | ----- | ----- |
| Before |  861   | 29429   | 35913 | 40413 |
| After  | 861    | 29208   | 35678 | 40178 |
```solidity
File: /src/Oracle.sol
112:    function getPrice(address token, uint collateralFactorBps) external returns (uint) {
113:        if(fixedPrices[token] > 0) return fixedPrices[token];
114:        if(feeds[token].feed != IChainlinkFeed(address(0))) {
115:            // get price from feed
116:            uint price = feeds[token].feed.latestAnswer();
117:            require(price > 0, "Invalid feed price");
118:            // normalize price
119:            uint8 feedDecimals = feeds[token].feed.decimals();
120:            uint8 tokenDecimals = feeds[token].tokenDecimals;

144:    }
```

```diff
diff --git a/src/Oracle.sol b/src/Oracle.sol
index 14338ed..1ed124a 100644
--- a/src/Oracle.sol
+++ b/src/Oracle.sol
@@ -111,13 +111,14 @@ contract Oracle {
     */
     function getPrice(address token, uint collateralFactorBps) external returns (uint) {
         if(fixedPrices[token] > 0) return fixedPrices[token];
-        if(feeds[token].feed != IChainlinkFeed(address(0))) {
+        FeedData  storage _feeds = feeds[token];
+        if(_feeds.feed != IChainlinkFeed(address(0))) {
             // get price from feed
-            uint price = feeds[token].feed.latestAnswer();
+            uint price = _feeds.feed.latestAnswer();
             require(price > 0, "Invalid feed price");
             // normalize price
-            uint8 feedDecimals = feeds[token].feed.decimals();
-            uint8 tokenDecimals = feeds[token].tokenDecimals;
+            uint8 feedDecimals = _feeds.feed.decimals();
+            uint8 tokenDecimals = _feeds.tokenDecimals;
             uint8 decimals = 36 - feedDecimals - tokenDecimals;
             uint normalizedPrice = price * (10 ** decimals);
             // potentially store price as today's low
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L389-L401
### Market.sol.borrowInternal(): debts[borrower] should use a local variable
```solidity
File: /src/Market.sol
389:    function borrowInternal(address borrower, address to, uint amount) internal {

395:        debts[borrower] += amount; //@audit: Initial access
396:        require(credit >= debts[borrower], "Exceeded credit limit");//@audit: 2nd access

401:    }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L559-L572
### Market.sol.forceReplenish(): debts[user] use a local variable 
```solidity
File: /src/Market.sol
559:    function forceReplenish(address user, uint amount) public {

565:        debts[user] += replenishmentCost; //@audit: 1st access
566:        uint collateralValue = getCollateralValueInternal(user);
567:        require(collateralValue >= debts[user], "Exceeded collateral value");//@audit: 2nd access

572:    }
```

### Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L19-L20
```solidity
File: /src/DBR.sol
19:    mapping(address => uint256) public balances;
20:    mapping(address => mapping(address => uint256)) public allowance;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L26-L28
```solidity
File: /src/DBR.sol
26:    mapping (address => uint) public debts; // user => debt across all tracked markets
27:    mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
28:    mapping (address => uint) public lastUpdated; // user => last update timestamp
```

### `keccak256()` should only need to be called on a specific string literal once

It should be saved to an immutable variable, and the variable used instead. If the hash is being used as a part of a function selector, the cast to `bytes4` should also only be done once


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L105
```solidity
File: /src/Market.sol
105:      keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
106:      keccak256(bytes("DBR MARKET")),
107:      keccak256("1"),


432:       keccak256(
433:          "BorrowOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
434:    ),


496:      keccak256(
497:          "WithdrawOnBehalf(address caller,address from,uint256 amount,uint256 nonce,uint256 deadline)"
498:      ),
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L233-L235
```solidity
File: /src/DBR.sol
233:    keccak256(
234:       "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
235:   ),
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L270-L272
```solidity
File: /src/DBR.sol
270:        keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
271:        keccak256(bytes(name)),
272:        keccak256("1"),
```
### Duplicated require()/revert() checks should be refactored to a modifier or function
This saves deployement gas

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L49
```solidity
File: /src/Fed.sol
49:        require(msg.sender == gov, "ONLY GOV");

58:        require(msg.sender == gov, "ONLY GOV");

67:        require(msg.sender == gov, "ONLY GOV");

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L76
```solidity
File: /src/Fed.sol
76:        require(msg.sender == chair, "ONLY CHAIR");

87:        require(msg.sender == chair, "ONLY CHAIR");

104:       require(msg.sender == chair, "ONLY CHAIR");
```

### x += y costs more gas than x = x + y for state variables

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L395
```solidity
File: /src/Market.sol
395:        debts[borrower] += amount;

397:        totalDebt += amount;

534:        debts[user] -= amount;
535:        totalDebt -= amount;

565:        debts[user] += replenishmentCost;

568:        totalDebt += replenishmentCost;

599:        debts[user] -= repaidDebt;
600:        totalDebt -= repaidDebt;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L91-L92
### Fed.sol.expansion(): Save 114 gas on average

|     | Average | Min  | Max  |
| --- | ------- | ---- | ---- |
| Before    | 95100    | 98247 | 113747 |
| After    | 94986   | 98129 | 113629 |

```solidity
File: /src/Fed.sol
92:        globalSupply += amount;
```

```diff
diff --git a/src/Fed.sol b/src/Fed.sol
index 1e819bb..2d6f0d5 100644
--- a/src/Fed.sol
+++ b/src/Fed.sol
@@ -89,7 +89,7 @@ contract Fed {
         require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
         dola.mint(address(market), amount);
         supplies[market] += amount;
-        globalSupply += amount;
+        globalSupply = globalSupply  +  amount;
         require(globalSupply <= supplyCeiling);
         emit Expansion(market, amount);
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L103-L113
### Fed.sol.contraction(): Saves 7 gas on average
|     | Average | Min  | Max  |
| --- | ------- | ---- | ---- |
| Before    | 13749    | 8152 | 29057 |
| After    | 13742   | 8152 | 29040 |

```solidity
File: /src/Fed.sol
111:        globalSupply -= amount;
```

```diff
diff --git a/src/Fed.sol b/src/Fed.sol
index 1e819bb..c474584 100644
--- a/src/Fed.sol
+++ b/src/Fed.sol
@@ -108,7 +108,7 @@ contract Fed {
         market.recall(amount);
         dola.burn(amount);
         supplies[market] -= amount;
-        globalSupply -= amount;
+        globalSupply = globalSupply - amount;
         emit Contraction(market, amount);
     }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360
```solidity
File: /src/DBR.sol
360:        _totalSupply += amount;

376:            _totalSupply -= amount;
```
### Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53
```solidity
File: /src/Oracle.sol

//@audit: uint8 tokenDecimals
53:    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
```

### Using unchecked blocks to save gas
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block
[see resource](https://github.com/ethereum/solidity/issues/10695)

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L362
```solidity
File: /src/Market.sol
362:        return collateralBalance - minimumCollateral;
```

The operation `collateralBalance - minimumCollateral` cannot underflow due to the check on [Line 361](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L361) that ensures that `collateralBalance` would be greater than `minimumCollateral` before performing the arithmetic operation

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L379
```solidity
File: /src/Market.sol
379:        return collateralBalance - minimumCollateral;
```
The operation `collateralBalance - minimumCollateral` cannot underflow due to the check on [Line 378](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L378) that ensures that `collateralBalance` would be greater than `minimumCollateral` before performing the arithmetic operation

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L110
```solidity
File: /src/Fed.sol
110:        supplies[market] -= amount;
```

The operation `supplies[market] - amount` cannot underflow due to the check on [Line 107](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L107) that ensures that the above operation would only be performed if `supplies[market]`  is greater than `amount`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L124
```solidity
File: /src/Fed.sol
124:         return marketValue - supply;
```

The operation `marketValue - supply` cannot underflow due to the check on [Line 123](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L123) that ensures that the above operation would only be performed if `marketValue`  is greater than `supply`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L111
```solidity
File: /src/DBR.sol
111:        return _totalSupply - totalDueTokensAccrued;
```
The operation `_totalSupply - totalDueTokensAccrued` cannot underflow due to the check on [Line 110](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L110) that ensures that the above operation would only be performed if `_totalSupply`  is greater than `totalDueTokensAccrued`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L374
```solidity
File: /src/DBR.sol
374:        balances[from] -= amount;
```
The operation `balances[from] -= amount` cannot underflow due to the check on [Line 373](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L373) that ensures that the above operation would only be performed if `balances[from]`  is greater than `amount`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L62
```solidity
File: /src/escrows/INVEscrow.sol
62:        if(invBalance < amount) xINV.redeemUnderlying(amount - invBalance); // we do not check return value because next call will fail if this fails anyway
```

The operation `amount - invBalance` cannot underflow due to the check on `if(invBalance < amount)`

### Splitting require() statements that use && saves gas - (saves 8 gas per &&)

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per &&
The gas difference would only be realized if the revert condition is realized(met).

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L75
```solidity
File: /src/Market.sol
75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

448:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

512:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L249
```solidity
File: /src/DBR.sol
249:            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```
**Proof**
**The following tests were carried out in remix with both optimization turned on and off**

```solidity
function multiple (uint a) public pure returns (uint){
	require ( a > 1 && a < 5, "Initialized");
	return  a + 2;
}
```
**Execution cost**
21617 with optimization and using &&
21976 without optimization and using &&


After splitting the require statement
```solidity
function multiple(uint a) public pure returns (uint){
	require (a > 1 ,"Initialized");
	require (a < 5 , "Initialized");
	return a + 2;
}
```
**Execution cost**
21609 with optimization and split require
21968 without optimization and using split require

### Use shorter revert strings(less than 32 bytes) 
Every reason string takes at least 32 bytes so make sure your string fits in 32 bytes or it will become more expensive.Each extra chunk of byetes past the original 32 incurs an MSTORE which costs 3 gas

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L76
```solidity
File: /src/Market.sol
76:        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

214:            require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L63
```solidity
File: /src/DBR.sol
63:        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

326:        require(markets[msg.sender], "Only markets can call onForceReplenish");


```

I suggest shortening the revert strings to fit in 32 bytes, or using custom errors.
