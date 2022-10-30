# 2022-10-INVERSE

## Gas Optimizations Report

### State variables should be cached in stack variables rather than re-reading them.

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **3** instances of this issue:_

```solidity
File: src/Market.sol

      /// @audit Cache `dbr`. Used 3 times in `forceReplenish`
560:  uint deficit = dbr.deficitOf(user);
563:  uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
569:  dbr.onForceReplenish(user, amount);
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

```solidity
File: src/Oracle.sol

      /// @audit Cache `feeds`. Used 3 times in `viewPrice`
82:   uint price = feeds[token].feed.latestAnswer();
85:   uint8 feedDecimals = feeds[token].feed.decimals();
86:   uint8 tokenDecimals = feeds[token].tokenDecimals;

      /// @audit Cache `feeds`. Used 3 times in `getPrice`
116:  uint price = feeds[token].feed.latestAnswer();
119:  uint8 feedDecimals = feeds[token].feed.decimals();
120:  uint8 tokenDecimals = feeds[token].tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol

### Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **1** instances of this issue:_

```solidity
File: src/DBR.sol

13:   uint8 public constant decimals = 18;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

### Functions that are access-restricted from most users may be marked as `payable`

Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **15** instances of this issue:_

```solidity
File: src/DBR.sol

70:   function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Fed.sol

48:   function changeGov(address _gov) public {

57:   function changeSupplyCeiling(uint _supplyCeiling) public {

66:   function changeChair(address _chair) public {

75:   function resign() public {

86:   function expansion(IMarket market, uint amount) public {

103:  function contraction(IMarket market, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol

```solidity
File: src/Market.sol

203:  function recall(uint amount) public {

212:  function pauseBorrows(bool _value) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

```solidity
File: src/Oracle.sol

66:   function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol

```solidity
File: src/escrows/GovTokenEscrow.sol

43:   function pay(address recipient, uint amount) public {

66:   function delegate(address delegatee) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol

```solidity
File: src/escrows/INVEscrow.sol

59:   function pay(address recipient, uint amount) public {

90:   function delegate(address delegatee) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol

```solidity
File: src/escrows/SimpleERC20Escrow.sol

36:   function pay(address recipient, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **25** instances of this issue:_

```solidity
File: src/DBR.sol

360:  _totalSupply += amount;

376:      _totalSupply -= amount;

289:  totalDueTokensAccrued += accrued;

172:  balances[msg.sender] -= amount;

174:      balances[to] += amount;

196:  balances[from] -= amount;

198:      balances[to] += amount;

362:      balances[to] += amount;

374:  balances[from] -= amount;

304:  debts[user] += additionalDebt;

316:  debts[user] -= repaidDebt;

332:  debts[user] += replenishmentCost;

288:  dueTokensAccrued[user] += accrued;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Fed.sol

92:    globalSupply += amount;

111:  globalSupply -= amount;

91:    supplies[market] += amount;

110:  supplies[market] -= amount;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol

```solidity
File: src/Market.sol

397:  totalDebt += amount;

535:  totalDebt -= amount;

568:  totalDebt += replenishmentCost;

600:  totalDebt -= repaidDebt;

395:  debts[borrower] += amount;

534:  debts[user] -= amount;

565:  debts[user] += replenishmentCost;

599:  debts[user] -= repaidDebt;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead

'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **12** instances of this issue:_

```solidity
File: src/DBR.sol

13:   uint8 public constant decimals = 18;

220:  uint8 v,
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Market.sol

422:  function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

486:  function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

```solidity
File: src/Oracle.sol

20:    uint8 tokenDecimals;

53:   function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

85:        uint8 feedDecimals = feeds[token].feed.decimals();

86:        uint8 tokenDecimals = feeds[token].tokenDecimals;

87:        uint8 decimals = 36 - feedDecimals - tokenDecimals;

119:      uint8 feedDecimals = feeds[token].feed.decimals();

120:      uint8 tokenDecimals = feeds[token].tokenDecimals;

121:      uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol

### Don't compare boolean expressions to boolean literals

Use `if(x)`/`if(!x)` instead of `if(x == true)`/`if(x == false)`.

_There are **1** instances of this issue:_

```solidity
File: src/DBR.sol

350:  require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

### Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single `require` check using two `require` checks can save gas

_There are **8** instances of this issue:_

```solidity
File: src/DBR.sol

249:      require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Market.sol

75:    require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

162:  require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

173:  require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184:  require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

195:  require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

448:      require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

512:      require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **21** instances of this issue:_

```solidity
File: src/DBR.sol

171:  require(balanceOf(msg.sender) >= amount, "Insufficient balance");

195:  require(balanceOf(from) >= amount, "Insufficient balance");

224:  require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

329:  require(deficit >= amount, "Amount > deficit");

373:  require(balanceOf(from) >= amount, "Insufficient balance");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Fed.sol

93:    require(globalSupply <= supplyCeiling);

107:   require(amount <= supply, "AMOUNT TOO BIG");

123:  if(supply >= marketValue) return 0;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol

```solidity
File: src/Market.sol

162:  require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

361:  if(collateralBalance <= minimumCollateral) return 0;

378:  if(collateralBalance <= minimumCollateral) return 0;

396:  require(credit >= debts[borrower], "Exceeded credit limit");

423:  require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

462:  require(limit >= amount, "Insufficient withdrawal limit");

487:  require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

533:  require(debt >= amount, "Insufficient debt");

562:  require(deficit >= amount, "Amount > deficit");

567:  require(collateralValue >= debts[user], "Exceeded collateral value");

582:  if(credit >= debt) return 0;

595:  require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

607:      if(escrow.balance() >= liquidationFee) {
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

### Tight variable packing

Solidity contracts have contiguous 32 bytes (256 bits) slots used in storage. By arranging the variables, it is possible to minimize the number of slots used within a contract’s storage and therefore reduce deployment costs.

_There are **1** instances of this issue:_

```solidity
File: src/Market.sol

38:   /// @audit Currently storage variables are packed in 15 slots but could fit in 14
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol

### Use `immutable` & `constant` for state variables that do not change their value

_There are **2** instances of this issue:_

```solidity
File: src/DBR.sol

11:   string public name;

12:   string public symbol;
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

### `require()/revert()` strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

_There are **4** instances of this issue:_

```solidity
File: src/DBR.sol

63:    require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

326:  require(markets[msg.sender], "Only markets can call onForceReplenish");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol

```solidity
File: src/Market.sol

76:    require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

214:      require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol
