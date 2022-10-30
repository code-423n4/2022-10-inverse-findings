# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Remove redundant checks in `onForceReplenish` function | 1 |
| 2      | Multiple address mappings can be combined into a single mapping of an address to a struct | 6 |
| 3      | Use `unchecked` blocks to save gas  |  3 |
| 4      | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  |  22 |
| 5      | Splitting `require()` statements that uses && saves gas  |  5 |

## Findings

### 1- Remove redundant checks in `onForceReplenish` function (saves 1120 gas) :

The function `onForceReplenish` from the `DBR.sol` contract can only be called from the market using the `forceReplenish` function which already contains the checks around the user deficit, so it's redundant to verify the same checks in the `onForceReplenish` function and it conts more gas.

As you can see in the code below the first lines of the `forceReplenish` function :

File: src/Market.sol [Line 560-562](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L560-L562)

```
function forceReplenish(address user, uint256 amount) public {
        uint256 deficit = dbr.deficitOf(user);
        require(deficit > 0, "No DBR deficit");
        require(deficit >= amount, "Amount > deficit");

        ...

}
```

And here are the first line of the `onForceReplenish` function :

File: src/DBR.sol [Line 327-329](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L327-L329)

```
function onForceReplenish(address user, uint256 amount) public {
        require(markets[msg.sender], "Only markets can call onForceReplenish");
        uint256 deficit = deficitOf(user);
        require(deficit > 0, "No deficit");
        require(deficit >= amount, "Amount > deficit");

        ...

}
```

The first check in the `onForceReplenish` function ensure that only the `forceReplenish` function can call this function, and all other checks are the same in both functions.

The `onForceReplenish` function should be modified as follow :
```
function onForceReplenish(address user, uint256 amount) public {
    require(markets[msg.sender], "Only markets can call onForceReplenish");
    // @audit removed the redundant checks
    uint256 replenishmentCost = (amount * replenishmentPriceBps) / 10000;
    accrueDueTokens(user);
    debts[user] += replenishmentCost;
    _mint(user, amount);
}
```

This will save **1120 gas** on average according to the tests.

### 2- Multiple address mappings can be combined into a single mapping of an address to a struct :

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

There are 6 instances of this issue :

The first one :

```
File: src/DBR.sol

26       mapping (address => uint) public debts;
27       mapping (address => uint) public dueTokensAccrued;
28       mapping (address => uint) public lastUpdated; 
```

These mappings could be refactored into the following struct and mapping for example :

```
struct BorrowInfo {
    uint256 debt;
    uint256 dueTokensAccrued;
    uint256 lastUpdated;
}
    
mapping(address => BorrowInfo) private _usersBorrow;
```

And the second is :

```
File: src/Market.sol

57       mapping (address => IEscrow) public escrows; 
58       mapping (address => uint) public debts; 
59       mapping(address => uint256) public nonces; 
```

These mappings could be refactored into the following struct and mapping for example :

```
struct UserInfo {
    IEscrow escrow;
    uint256 debt;
    uint256 nonce;
}
    
mapping(address => UserInfo) private _usersInfo;
```

### 3- Use `unchecked` blocks to save gas :

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

Consider wrapping with an unchecked block here (around **25 gas** saved per instance), there are 3 instances of this issue:

File: src/Market.sol [Line 534-535](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534-L535)

```
debts[user] -= amount;
totalDebt -= amount;
```

The above operations cannot underflow due to the check :

[require(debt >= amount, "Insufficient debt");](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533) 

And because totalDebt only get increase/decreased when debts[user] changes, the totalDebt can underflow if debts[user] doesn't underflow

File: src/Market.sol [Line 599-600](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L599-L600)

```
debts[user] -= repaidDebt;
totalDebt -= repaidDebt;
```

The above operations cannot underflow due to the check :

[require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595) 
 
The liquidationFactorBps is always lower than 10000 so repaidDebt is always lower than debts[user] and because totalDebt only get increase/decreased when debts[user] changes, the totalDebt can underflow if debts[user] doesn't underflow.

File: src/Fed.sol [Line 110-110](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110-L111)

```
supplies[market] -= amount;
globalSupply -= amount;
```

The above operations cannot underflow due to the check :

[require(amount <= supply, "AMOUNT TOO BIG");](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L107) 
 
Because globalSupply only get increase/decreased when supplies[market] changes, the globalSupply can underflow if supplies[market] doesn't underflow.

### 4- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

Using the addition/substraction operator instead of plus-equals/minus-equals saves **113 gas** as explained [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)

There are 22 instances of this issue:

```
File: src/DBR.sol

172     balances[msg.sender] -= amount;
174     balances[to] += amount;
196     balances[from] -= amount;
288     dueTokensAccrued[user] += accrued;
289     totalDueTokensAccrued += accrued;
332     debts[user] += replenishmentCost;
360     _totalSupply += amount;
362     balances[to] += amount;
274     balances[from] -= amount;
376     _totalSupply -= amount;

File: src/Fed.sol 

91      supplies[market] += amount;
92      globalSupply += amount;
110     supplies[market] -= amount;
111     globalSupply -= amount;

File: src/Market.sol

395     debts[borrower] += amount;
397     totalDebt += amount;
534     debts[user] -= amount;
535     totalDebt -= amount;
565     debts[user] += replenishmentCost;
568     totalDebt += replenishmentCost;
599     debts[user] -= repaidDebt;
600     totalDebt -= repaidDebt;
```

### 5- Splitting `require()` statements that uses && saves gas (around 8 gas saved per instance) :

There are 5 instances of this issue :

```
File: src/Market.sol

75      require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
162     require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173     require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184     require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195     require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
```
