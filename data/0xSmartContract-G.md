## Summary
### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] | Packing Storage | 1 |
| [G-02] | Multiple address mappings can be combined into a single mapping of an address to a struct | 2 |
| [G-03] | No need ```return``` keyword for gas efficient | 5 |
| [G-04] | Usage of uints/ints smaller than 32 bytes (256 bits) saves gas | 1 |
| [G-05] | Functions guaranteed to revert_ when callled by normal users can be marked `payable` | 11|
| [G-06] | ```x -= y (x += y) ``` costs more gas than ```x = x – y (x = x + y)``` for state variables| 27 |
| [G-07] | Optimize names to save gas [22 gas per instance]  | All contracts |
| [G-08] | Use assembly to check for ```address(0)``` | 4 |
| [G-09] | Use ``assembly`` to write _address storage values_  | 18|
| [G-10] | The solady Library's Ownable contract is significantly gas-optimized, which can be used| 1 |
| [G-11] | Setting The Constructor To Payable|5 |
| [G-12] |Reduce the size of error messages (Long revert Strings)  | 5 |
| [G-13] |Using double require instead of && consumes less gas | 11 |
| [G-14] | Multiple address mappings can be combined into a single mapping of an address to a struct | 9 |
| [G-15] | Use `external` instead of `public` for functions not called from contract  | 16 |

Total 15 issues

### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Missing `zero-address` check in `constructor` |


## Details

### [G-01] Packing Storage

Storing state variables packaging is more gas efficient

```diff


   12:     string public symbol;
-  13:     uint8 public constant decimals = 18;
+  13:     uint8 public constant decimals = 18;
   15:     address public operator;
   16      address public pendingOperator;


```
### [G-02] Multiple address mappings can be combined into a single mapping of an address to a struct

**Context:**

```diff

src/DBR.sol:
  23      mapping(address => uint256) public nonces;
- 24:     mapping (address => bool) public minters;
- 25:     mapping (address => bool) public markets;


+ 24:     mapping (address => uint256) public markets_minters;
+            // address => uint256 => 1 (minters true)
+            // address => uint256 => 2 (minters false)
+            // address => uint256 => 3 (markets true)
+            // address => uint256 => 4 (markets false)


```

**Description:**
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

 Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

### [G-03] No need ```return``` keyword for gas efficient

**Context:**

```solidity
src/BorrowController.sol:
  46      function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
  
  48          return contractAllowlist[msgSender];

src/DBR.sol:
  160          emit Approval(msg.sender, spender, amount);
  161:         return true;
  162      }

  176          emit Transfer(msg.sender, to, amount);
  177:         return true;
  178      }

  200          emit Transfer(from, to, amount);
  201:         return true;
  202      }

```

**Recommendation:**
You should remove the ```return``` keyword from the specified contexts.

**Proof Of Concept:**

```js
src/DBR.sol:
  157      */
  158:     function approve(address spender, uint256 amount) public virtual returns (bool) {
  159:         allowance[msg.sender][spender] = amount;
  160:         emit Approval(msg.sender, spender, amount);
  161:         return true;
  162:     }


```
**Gas Report:**

```js

without `return` keyword;
╭──────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/DBR.sol:DolaBorrowingRights contract ┆                 ┆       ┆        ┆       ┆         │
╞══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                          ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 1414004                                  ┆ 8019            ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                            ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ approve                                  ┆ 22463           ┆ 24038 ┆ 24563  ┆ 24563 ┆ 4       │
╰──────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯



```

```js

with `return` keyword;
╭──────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/DBR.sol:DolaBorrowingRights contract ┆                 ┆       ┆        ┆       ┆         │
╞══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                          ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 1411204                                  ┆ 8005            ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                            ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ approve                                  ┆ 22469           ┆ 24044 ┆ 24569  ┆ 24569 ┆ 4       │
╰──────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯


```
### [G-04] Usage of uints/ints smaller than 32 bytes (256 bits) saves gas

**Context:**


```diff

src/Oracle.sol:
  17  
  18:     struct FeedData {
  19:         IChainlinkFeed feed;
- 20:         uint8 tokenDecimals;
+ 20:         uint256 tokenDecimals;
  21:     }

```

**Description:**
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

### [G-05]  Functions guaranteed to revert_ when callled by normal users can be marked `payable` 

**Context:** 
```solidity
11 results - 3 files

src/BorrowController.sol:
  26:     function setOperator(address _operator) public onlyOperator { operator = _operator; }
  32:     function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
  38:     function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }

src/DBR.sol:
  53:     function setPendingOperator(address newOperator_) public onlyOperator {
  62:     function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
  81:     function addMinter(address minter_) public onlyOperator {
  90:     function removeMinter(address minter_) public onlyOperator {
  99:     function addMarket(address market_) public onlyOperator {

src/Oracle.sol:
  44:     function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
  53:     function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
  61:     function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
 
```

**Description:**
If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

**Recommendation:**
Functions guaranteed to revert when called by normal users can be marked payable  (for only ```onlyowner or admin``` functions)

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo();
        c1.foo();
    }
}

contract Contract0 {
    uint256inverse;
    
    function foo() external {
       inverse++;
    }
}

contract Contract1 {
    uint256inverse;
    
    function foo() external payable {
       inverse++;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44293                                     ┆ 252             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22308           ┆ 22308 ┆ 22308  ┆ 22308 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 41893                                     ┆ 240             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22284           ┆ 22284 ┆ 22284  ┆ 22284 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```
### [G-06] ```x -= y (x += y) ``` costs more gas than ```x = x – y (x = x + y)``` for state variables


**Description:**
```x -= y``` costs more gas than ```x = x – y``` for state variables.

**Context:**

```solidity
src/DBR.sol:
  173          unchecked {
  174:             balances[to] += amount;
  175          }

  197          unchecked {
  198:             balances[to] += amount;
  199          }

  300          uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
  301:         dueTokensAccrued[user] += accrued;
  302:         totalDueTokensAccrued += accrued;
  303          lastUpdated[user] = block.timestamp;

  316          require(deficitOf(user) == 0, "DBR Deficit");
  317:         debts[user] += additionalDebt;
  318      }

  344          accrueDueTokens(user);
  345:         debts[user] += replenishmentCost;
  346          _mint(user, amount);

  372      function _mint(address to, uint256 amount) internal virtual {
  373:         _totalSupply += amount;
  374          unchecked {
  375:             balances[to] += amount;
  376          }

src/Fed.sol:
  90          dola.mint(address(market), amount);
  91:         supplies[market] += amount;
  92:         globalSupply += amount;
  93          require(globalSupply <= supplyCeiling);

src/Market.sol:
  401          uint credit = getCreditLimitInternal(borrower);
  402:         debts[borrower] += amount;
  403          require(credit >= debts[borrower], "Exceeded credit limit");
  404:         totalDebt += amount;
  405          dbr.onBorrow(borrower, amount);

  571          uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;
  572:         debts[user] += replenishmentCost;
  573          uint collateralValue = getCollateralValueInternal(user);
  574          require(collateralValue >= debts[user], "Exceeded collateral value");
  575:         totalDebt += replenishmentCost;
  576          dbr.onForceReplenish(user, amount);

  604          uint liquidatorReward = repaidDebt * 1 ether / price;
  605:         liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
  606          debts[user] -= repaidDebt;

```

```solidity
src/DBR.sol:
  171          require(balanceOf(msg.sender) >= amount, "Insufficient balance");
  172:         balances[msg.sender] -= amount;
  173          unchecked {

  195          require(balanceOf(from) >= amount, "Insufficient balance");
  196:         balances[from] -= amount;
  197          unchecked {

  328          accrueDueTokens(user);
  329:         debts[user] -= repaidDebt;
  330      }

  386          require(balanceOf(from) >= amount, "Insufficient balance");
  387:         balances[from] -= amount;
  388          unchecked {
  389:             _totalSupply -= amount;
  390          }

src/Fed.sol:
  109          dola.burn(amount);
  110:         supplies[market] -= amount;
  111:         globalSupply -= amount;
  112          emit Contraction(market, amount);

src/Market.sol:
  540          require(debt >= amount, "Insufficient debt");
  541:         debts[user] -= amount;
  542:         totalDebt -= amount;
  543          dbr.onRepay(user, amount);

  605          liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
  606:         debts[user] -= repaidDebt;
  607:         totalDebt -= repaidDebt;
  608          dbr.onRepay(user, repaidDebt);
```



**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
  
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.swap(2,3);
        c1.swap1(2,3);
    }
}
contract Contract0 {
    uint256 public amountIn = 10;
    
    
    function swap(uint256 amountInToBin, uint256 fee)  external returns (uint256 ) {
       
       return amountIn -= amountInToBin + fee;
    }
}

contract Contract1 {
    uint256 public amountIn = 10;
    
    function swap1(uint256 amountInToBin, uint256 fee)  external returns (uint256 result1) {
       return (amountIn = amountIn - (amountInToBin + fee));
    }
}
```
**Gas Report:**
```js
╭──────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/test.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞══════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 83017                                ┆ 341             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ swap                                 ┆ 5454            ┆ 5454 ┆ 5454   ┆ 5454 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭──────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/test.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞══════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 83017                                ┆ 341             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ swap1                                ┆ 5431            ┆ 5431 ┆ 5431   ┆ 5431 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```
### [G-07] Optimize names to save gas [22 gas per instance]

**Context:** 
All Contracts

**Description:** 
Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ```Market.sol ``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

Market.sol function names can be named and sorted according to METHOD ID

```js
Sighash   |   Function Signature
========================
a9059cbb  =>  transfer(address,uint256)
23b872dd  =>  transferFrom(address,address,uint256)
70a08231  =>  balanceOf(address)
449e815d  =>  getPrice(address,uint256)
949c4fa3  =>  viewPrice(address,uint256)
ca786762  =>  initialize(IERC20,address)
12c93f59  =>  onDeposit()
c4076876  =>  pay(address,uint256)
b69ef8a8  =>  balance()
f7f11fb7  =>  onBorrow(address,uint256)
d8d2c648  =>  onRepay(address,uint256)
e94d28fc  =>  onForceReplenish(address,uint256)
36459f04  =>  deficitOf(address)
a10f84cb  =>  replenishmentPriceBps()
da3d454c  =>  borrowAllowed(address,address,uint256)
3644e515  =>  DOMAIN_SEPARATOR()
6d5a0e99  =>  computeDomainSeparator()
cb06209e  =>  setOracle(IOracle)
a9674c87  =>  setBorrowController(IBorrowController)
cfad57a2  =>  setGov(address)
46e368d4  =>  setLender(address)
48bde20c  =>  setPauseGuardian(address)
7164695a  =>  setCollateralFactorBps(uint256)
d1220a3c  =>  setLiquidationFactorBps(uint256)
6f48fbb6  =>  setReplenismentIncentiveBps(uint256)
34734dd3  =>  setLiquidationIncentiveBps(uint256)
8951b054  =>  setLiquidationFeeBps(uint256)
7d32e793  =>  recall(uint256)
c8018619  =>  pauseBorrows(bool)
f6a8419e  =>  createEscrow(address)
a26d494d  =>  getEscrow(address)
b6b55f25  =>  deposit(uint256)
b75061bb  =>  depositAndBorrow(uint256,uint256)
47e7ef24  =>  deposit(address,uint256)
4ef64ee7  =>  predictEscrow(address)
97904e42  =>  getCollateralValue(address)
48b6a3ba  =>  getCollateralValueInternal(address)
2c333e25  =>  getCreditLimit(address)
136144d0  =>  getCreditLimitInternal(address)
12d65ec6  =>  getWithdrawalLimitInternal(address)
c74e6d80  =>  getWithdrawalLimit(address)
2b49b869  =>  borrowInternal(address,address,uint256)
c5ebeaec  =>  borrow(uint256)
1ef08b75  =>  borrowOnBehalf(address,uint256,uint256,uint8,bytes32,bytes32)
42422a87  =>  withdrawInternal(address,address,uint256)
2e1a7d4d  =>  withdraw(uint256)
3525f591  =>  withdrawOnBehalf(address,uint256,uint256,uint8,bytes32,bytes32)
5a57b46f  =>  invalidateNonce()
22867d78  =>  repay(address,uint256)
ebc9b94d  =>  repayAndWithdraw(uint256,uint256)
651afe83  =>  forceReplenish(address,uint256)
02197952  =>  getLiquidatableDebt(address)
bcbaf487  =>  liquidate(address,uint256)
```
### [G-08] Use assembly to check for ```address(0)``` 


**Context:** 
```solidity
4 results - 3 files

src/DBR.sol:
  260              );
  261:             require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
  262              allowance[recoveredAddress][spender] = value;

src/Market.sol:
  454              );
  455:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
  456              borrowInternal(from, msg.sender, amount);

  518              );
  519:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
  520              withdrawInternal(from, msg.sender, amount);

src/test/mocks/ERC20.sol:
  178          require(
  179:             recoveredAddress != address(0) && recoveredAddress == owner,
  180              "INVALID_SIGNATURE"

```

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    function testGas() public view {
        c0.setOperator(address(this));
        c1.setOperator(address(this));
    }
}
contract Contract0 {
    function setOperator(address operator_) public pure {
        require(operator_) != address(0), "INVALID_RECIPIENT");    }
}
contract Contract1 {
    function setOperator(address operator_) public pure {
        assembly {
            if iszero(operator_) {
                mstore(0x00, "Callback_InvalidParams")
                revert(0x00, 0x20)
            }
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50899                                     ┆ 285             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 258             ┆ 258 ┆ 258    ┆ 258 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44893                                     ┆ 255             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 252             ┆ 252 ┆ 252    ┆ 252 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```
### [G-09] Use ``assembly`` to write _address storage values_ 


**Context:**

18 results - 5 files:

```solidity
src/BorrowController.sol:
  12  
  13:     constructor(address _operator) {
  14          operator = _operator;


src/Fed.sol:
  35  
  36:     constructor (IDBR _dbr) {
  37          dbr = _dbr;


src/BorrowController.sol:
  25      */
  26:     function setOperator(address _operator) public onlyOperator { operator = _operator; }
  27  

src/DBR.sol:
  52      */
  53:     function setPendingOperator(address newOperator_) public onlyOperator {
  54          pendingOperator = newOperator_;

  61      */
  62:     function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
  63          require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

src/Market.sol:
  117      */
  118:     function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
  119  

  123      */
  124:     function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
  125  

  129      */
  130:     function setGov(address _gov) public onlyGov { gov = _gov; }
  131  

  135      */
  136:     function setLender(address _lender) public onlyGov { lender = _lender; }
  137  

  141      */
  142:     function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
  143      

  148      */
  149:     function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
  150          require(_collateralFactorBps < 10000, "Invalid collateral factor");

  160      */
  161:     function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
  162          require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

  171      */
  172:     function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
  173          require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

  182      */
  183:     function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
  184          require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

  193      */
  194:     function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
  195          require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

src/Oracle.sol:
  43      */
  44:     function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
  45  

  52      */
  53:     function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
  54  

  60      */
  61:     function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
  62 
```


**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTestFoundry is DSTest {
    Contract1 c1;
    Contract2 c2;
    
    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }
    
    function testGas() public {
        c1.setRewardTokenAndAmount(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4,356);
        c2.setRewardTokenAndAmount(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4,356);
    }
}

contract Contract1  {
    address rewardToken ;
    uint256 reward;

    function setRewardTokenAndAmount(address token_, uint256 reward_) external {
        rewardToken = token_;
        reward = reward_;
    }
}

contract Contract2  {
    address rewardToken ;
    uint256 reward;

    function setRewardTokenAndAmount(address token_, uint256 reward_) external {
        assembly {
            sstore(rewardToken.slot, token_)
            sstore(reward.slot, reward_)           

        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50899                                     ┆ 285             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount                   ┆ 44490           ┆ 44490 ┆ 44490  ┆ 44490 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 38087                                     ┆ 221             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount                   ┆ 44457           ┆ 44457 ┆ 44457  ┆ 44457 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```
### [G-10] The solady Library's Ownable contract is significantly gas-optimized, which can be used

**Description:**
The project uses the `onlyOperator` authorization model with the `PendingOwnable.sol` contract. I recommend using _Solady's highly gas optimized contract._

https://github.com/Vectorized/solady/blob/main/src/auth/OwnableRoles.sol

### [G-11] Setting The Constructor To Payable [13 gas per instance]


**Description:**
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

**Context:**
```solidity
5 results - 5 files

src/BorrowController.sol:
  12  
  13:     constructor(address _operator) {
  14          operator = _operator;

src/DBR.sol:
  29  
  30:     constructor(
  31          uint _replenishmentPriceBps,

src/Fed.sol:
  35  
  36:     constructor (IDBR _dbr, IDola _dola, address _gov, address _chair, uint _supplyCeiling) {
  37          dbr = _dbr;

src/Market.sol:
  60  
  61:     constructor (
  62          address _gov,

src/Oracle.sol:
  28  
  29:     constructor(
  30          address _operator
```

**Recommendation:**
Set the constructor to ```payable```

**Proof Of Concept:**
https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5?u=pcaversaccio

The optimizer was turned on and set to 10000 runs

```js
contract GasTestFoundry is DSTest {
    
    Contract1 c1;
    Contract2 c2;
    
    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }
    
    function testGas() public {
        c1.x();
        c2.x();
    }
}

contract Contract1 {
    
    uint256 public dummy;
    constructor() payable {
        dummy = 1;
    }
    
    function x() public {

    }
}

contract Contract2 {
    
    uint256 public dummy;
    constructor() {
        dummy = 1;
    }
    
    function x() public {
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49563                                     ┆ 159             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤

╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49587                                     ┆ 172             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
```
### [G-12] Reduce the size of error messages (Long revert Strings) 

**Description:**
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional ``mstore``, along with additional overhead for computing memory offset, etc.


```solidity
5 results - 2 files


src/BorrowController.sol:


   62      function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
   63:         require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

  313      function onBorrow(address user, uint additionalDebt) public {
  314:         require(markets[msg.sender], "Only markets can call onBorrow");

  326      function onRepay(address user, uint repaidDebt) public {
  327:         require(markets[msg.sender], "Only markets can call onRepay");

  338      function onForceReplenish(address user, uint amount) public {
  339:         require(markets[msg.sender], "Only markets can call onForceReplenish");

src/Market.sol:

  213          if(_value) {
  214:             require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

```

**Recommendation:**
Revert strings > 32 bytes or use Custom Error()

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo();
        c1.foo();
    }
}

contract Contract0 {
    uint256 test1;
    
    function foo() external {
    require(test1 != 0, "This is Error");
    }
}

contract Contract1 {
    uint256 test1;
    
    function foo() external {
        require(test1 != 0, "Your Project Drop List Has Error with  ID and other parameters");
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44093                                     ┆ 251             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 2334            ┆ 2334 ┆ 2334   ┆ 2334 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 51705                                     ┆ 290             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 2352            ┆ 2352 ┆ 2352   ┆ 2352 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

### [G-13] Using double require instead of && consumes less gas

**Context:**

```solidity
11 results - 3 files

src/DBR.sol:
  261: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

src/Market.sol:
   75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
  162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
  173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
  184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
  195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
  455: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
  519: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

src/Oracle.sol:
   96: uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
  135: uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
  136: if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

```
### [G-14] Multiple address mappings can be combined into a single mapping of an address to a struct

**Context:**
```solidity

   9: contract DolaBorrowingRights {

  19:     mapping(address => uint256) public balances;
  23:     mapping(address => uint256) public nonces;
  24:     mapping (address => bool) public minters;
  25:     mapping (address => bool) public markets;
  26:     mapping (address => uint) public debts; // user => debt across all tracked markets
  27:     mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
  28:     mapping (address => uint) public lastUpdated; // user => last update timestamp
```

**Description:**
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

 Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
### [G-15] Use `external` instead of `public` for functions not called from contract


**Context:**
```solidity
16 results - 4 files

src/BorrowController.sol:
  26:     function setOperator(address _operator) public onlyOperator { operator = _operator; }

src/DBR.sol:
  53:     function setPendingOperator(address newOperator_) public onlyOperator {
  
  62:     function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

src/Market.sol:
  118:     function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

  124:     function setBorrowController(IBorrowController _borrowController) public onlyGov

  130:     function setGov(address _gov) public onlyGov { gov = _gov; }
  
  136:     function setLender(address _lender) public onlyGov { lender = _lender; }
  
  142:     function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
  
  149:     function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
  
  161:     function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

  172:     function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

  183:     function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

  194:     function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

src/Oracle.sol:
  44:     function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

  53:     function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { 

  61:     function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

**Description:**
Use the external function visibility for gas optimization because the public visibility modifier is equivalent to using the external and internal visibility modifier, meaning both public and external can be called from outside of your contract, which requires more gas.

Remember that of these two visibility modifiers, only the public modifier can be called from other functions inside of your contract.

###  [S-01] Missing `zero-address` check in `constructor`

**Description:**
Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly. It also wast gas as it requires the redeployment of the contract.

```solidity
5 results - 5 files

src/BorrowController.sol:
  12  
  13:     constructor(address _operator) {
  14          operator = _operator;

src/DBR.sol:
  29  
  30:     constructor(
  31          uint _replenishmentPriceBps,

src/Fed.sol:
  35  
  36:     constructor (IDBR _dbr, IDola _dola, address _gov, address _chair, uint _supplyCeiling) {
  37          dbr = _dbr;

src/Market.sol:
  60  
  61:     constructor (
  62          address _gov,

src/Oracle.sol:
  28  
  29:     constructor(
  30          address _operator
```
