  # Low Risk Issues

## Summary Of Findings:

|  | Issue 
-- | -- 
1 | Missing zero-address checks
2 | The checks performed in the `constructor` are not consistent with the set functions
3 | Some emit statements doesn't emit all relevant values
4 | Reason strings are missing in some `require` statements
5 | Incorrect implementation of `BorrowController`
6 | Debt's of multiple markets are cumulatively stored in the same mapping in `DBR.sol`

## Details on Findings:

### 1. <ins>Missing zero-address checks</ins>

Zero-address checks must be performed before setting important admin addresses. This is especially important in cases where the address cannot be changed once its set to zero.

The following instances were found:
1. [setOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26) in `BorrowController.sol`
2. [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L14) block in `BorrowController.sol`
3. [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L39) block in `DBR.sol`
4. [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L37-L40) block in `Fed.sol`
5. [changeGov](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48) in `Fed.sol`
6.  [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L77-L83) in `Market.sol`
7.  [setGov](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130) in `Market.sol`
8. [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L32) block in `Oracle.sol`

### 2. <ins>The checks performed in the `constructor` are not consistent with the set functions</ins>
The following instances were noticed:

1. `replenishmentPriceBps` assigned in constructor in [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L36) doesn't check if its value is greater than zero. This check is correctly performed in its [set function](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L63).
2. `replenishmentIncentiveBps` assigned in constructor in [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L76) doesn't check if its value is greater than zero. This check is correctly performed in its [set function](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L173).

### 3. <ins>Some emit statements doesn't emit all relevant values</ins>
The following instances were noticed:

1. [claimOperator()](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L74) method in `DBR.sol` doesnt emit the old operator. It only emits the new one. 
2. [borrowInternal](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L400) method in `Market.sol` doesn't emit the address `to`, to which the borrowed `Dola` is sent.

### 4. <ins>Reason strings are missing in some `require` statements</ins>
When a transaction reverts, its important to let the user know the reason behind the revert. Though this is generally done in the codebase, there are few `require` statements which doesn't have a reason string.

There are 3 instances of this:

File: [src/escrows/GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol)
```solidity
Line 67:    require(msg.sender == beneficiary);
```
File: [src/escrows/INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol)
```solidity
Line 91:    require(msg.sender == beneficiary);
```
File: [src/Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol)
```solidity
Line 93:    require(globalSupply <= supplyCeiling);
```

### 5. <ins>Incorrect implementation of `BorrowController`</ins>
`BorrowController` contract is supposed to control who can borrow from the protocol and who cannot. Until the address of the `BorrowController` is set via the [setBorrowController](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L124) function, this check will not be performed. Once we do set the address, any user who wants to borrow would need his `contractAllowlist` mapping to be turned on to `true`. This check is performed in [borrowInternal](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L391-L393) function.

This is the incorrect way to implement this. Assuming that this contract would be used for blocking a select group of malicious people, what we need is a `BlockList` and not an `AllowList`. Which means when we want to block an address we can explicitly set it to `true` in its mapping in the `BorrowController` contract. Every other address will have a default value of `false` in the mapping and will be allowed to borrow from the Market.

### 6. <ins>Debt's of multiple markets are cumulatively stored in the same mapping in `DBR.sol`</ins>
Each Market has a `debts` mapping which tells us how much the [user had borrowed](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L395) via that particular market. 

The same debts are also stored in the [DBR contract](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L304) when DBR tokens are minted to the user for his borrow. This `debts` mapping is also one dimensional just like in the Market contract. Which means that the debts across all markets are saved cumulatively in the same variable. 

This exposes the protocol to a certain risk. If one of the collaterals has an issue with its price or if any one of the markets has a particular vulnerability it can affect all the other markets in which the user has borrowed from. Because in the end, all the debts are stored in the same mapping. 

So I recommend that a 2 dimensional mapping is used to store the debts. Something like `debts[user][market]`. This will make sure that even if one market has an issue it wont affect the other markets directly.

 # Non-Critical Issues

## Summary Of Findings:

|  | Issue 
-- | -- 
1 | Use a consistent type name (`uint256` or `uint`) for 256 bit unsigned integers
2 | Slight correction in naming, brings perfection
3 | Wrong type for state variable `dola` in `Market.sol`
4 | Its better to declare events before the modifiers and functions

## Details on Findings:

### 1. <ins>Use a consistent type name (`uint256` or `uint`) for 256 bit unsigned integers</ins>

It's good to keep consistency in the type which you use. Though they are the same, the codebase will look much more professional and nicer if you use either `uint` or `uint256` instead of using both. Who knows, as the Solidity language is evolving, they might introduce new types which might confuse future readers. I would recommend to stick with `uint256`, as its more explicit. 

There are more than 100 instances of this spread out in several files:
1. [BorrowController.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol) has 1 instance.
2. [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol) has 19 instances.
3. [Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol) has 17 instances.
4. [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol) has 69 instances.
5. [Oracle.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol) has 21 instances.
6. [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol) has 1 instance.
7. [INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol) has 8 instances.
8. [SimpleERC20Escrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol) has 1 instance.

### 2. <ins>Slight correction in naming, brings perfection</ins>

In `DBR.sol`, the below mapping is named as `allowance`. It's more right to name it as `allowances`. The other variables declared together with this one are named in plural. 
```solidity
mapping(address => mapping(address => uint256)) public allowance;
```

For example: `balances`, `minters`, `debts`, `markets` etc..

Look at [ERC-20 contract](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/36951d58386b9fee81b237e6c6626c9115ccef3a/contracts/token/ERC20/ERC20.sol#L38) for a more official reference.

### 3. <ins>Wrong type for `dola` in `Market.sol`</ins>
In [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L44), the state variable `dola` is declared as `immutable`. Generally we use `immutable` type for state variables which are assigned in the constructor.

But here, the constant value is assigned when `dola` is declared. Two things could be done here:
1. Either make `dola` a `constant`. Which will save gas.
2. Or keep it as an `immutable`, but pass its value in the constructor. 

### 4. <ins>Its better to declare events before the modifiers and functions</ins>
As per [Solidity Docs - Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), the order of layout within a contract is as follows:
> Type declarations
State variables
Events
Modifiers
Functions

But this style is not followed in the following contracts:
1. [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L381-L386)
2. [Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L140-L141)
3. [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L614-L620)
4. [Oracle.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L146-L147)