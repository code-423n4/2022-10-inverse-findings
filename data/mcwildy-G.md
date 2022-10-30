# GAS OPTIMIZATIONS REPORT

## INVERSE

### Avoid using `&&` in `require()` or `if()` statements

Use multiple `require()`/`if` statements instead

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L75:  require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

line#L162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

line#L184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

line#L195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

line#L512: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L249: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

### Use x = x + y instead of x += y for storage variables

[Fed.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol):

```solidity
line#L92:  globalSupply += amount;

line#L111: globalSupply -= amount;

line#L91:  supplies[market] += amount;

line#L110: supplies[market] -= amount;
```

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L360: _totalSupply += amount;

line#L376: _totalSupply -= amount;

line#L172: balances[msg.sender] -= amount;

line#L174: balances[to] += amount;

line#L198: balances[to] += amount;

line#L362: balances[to] += amount;

line#L304: debts[user] += additionalDebt;

line#L316: debts[user] -= repaidDebt;

line#L288: dueTokensAccrued[user] += accrued;
```

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L397: totalDebt += amount;

line#L535: totalDebt -= amount;

line#L600: totalDebt -= repaidDebt;

line#L395: debts[borrower] += amount;

line#L565: debts[user] += replenishmentCost;

line#L599: debts[user] -= repaidDebt;
```

### Use custom errors instead of `require` strings

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L45:  require(msg.sender == operator, "ONLY OPERATOR");

line#L63:  require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

line#L171: require(balanceOf(msg.sender) >= amount, "Insufficient balance");

line#L195: require(balanceOf(from) >= amount, "Insufficient balance");

line#L249: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

line#L301: require(markets[msg.sender], "Only markets can call onBorrow");

line#L314: require(markets[msg.sender], "Only markets can call onRepay");

line#L326: require(markets[msg.sender], "Only markets can call onForceReplenish");

line#L329: require(deficit >= amount, "Amount > deficit");

line#L350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

[Oracle.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol):

```solidity
line#L36:  require(msg.sender == operator, "ONLY OPERATOR");

line#L67:  require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");

line#L104: revert("Price not found");

line#L117: require(price > 0, "Invalid feed price");
```

[GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol):

```solidity
line#L31:  require(market == address(0), "ALREADY INITIALIZED");

line#L44:  require(msg.sender == market, "ONLY MARKET");
```

[BorrowController.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol):

```solidity
line#L18:  require(msg.sender == operator, "Only operator");
```

[Fed.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol):

```solidity
line#L49:  require(msg.sender == gov, "ONLY GOV");

line#L58:  require(msg.sender == gov, "ONLY GOV");

line#L76:  require(msg.sender == chair, "ONLY CHAIR");

line#L87:  require(msg.sender == chair, "ONLY CHAIR");

line#L89:  require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");

line#L104: require(msg.sender == chair, "ONLY CHAIR");

line#L107: require(amount <= supply, "AMOUNT TOO BIG");
```

[SimpleERC20Escrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol):

```solidity
line#L26:  require(market == address(0), "ALREADY INITIALIZED");

line#L37:  require(msg.sender == market, "ONLY MARKET");
```

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L74:  require(_collateralFactorBps < 10000, "Invalid collateral factor");

line#L75:  require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

line#L93:  require(msg.sender == gov, "Only gov can call this function");

line#L150: require(_collateralFactorBps < 10000, "Invalid collateral factor");

line#L173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

line#L184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

line#L204: require(msg.sender == lender, "Only lender can recall");

line#L214: require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");

line#L236: require(instance != IEscrow(address(0)), "ERC1167: create2 failed");

line#L390: require(!borrowPaused, "Borrowing is paused");

line#L396: require(credit >= debts[borrower], "Exceeded credit limit");

line#L423: require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

line#L462: require(limit >= amount, "Insufficient withdrawal limit");

line#L487: require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

line#L533: require(debt >= amount, "Insufficient debt");

line#L561: require(deficit > 0, "No DBR deficit");

line#L567: require(collateralValue >= debts[user], "Exceeded collateral value");

line#L592: require(repaidDebt > 0, "Must repay positive debt");

line#L595: require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
```

[INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol):

```solidity
line#L45:  require(market == address(0), "ALREADY INITIALIZED");

line#L60:  require(msg.sender == market, "ONLY MARKET");
```

### Use `1` and `2` instead of `true` and `false`

EVM word size is 32 bytes. The `uint256 1` and `uint256 2` will be cheaper to use than the `boolean true` and `boolean false` since the first are 32 bytes long and the last are only 1 byte

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L52:  bool immutable callOnDepositCallback;

line#L53:  bool public borrowPaused;
```

### Use `uint256` instead of the smaller uints where possible

The word-size in ethereum is 32 bytes which means that every variables which is sized with less bytes will cost more to operate with

[Oracle.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol):

```solidity
line#L20:  uint8 tokenDecimals;

line#L53:  function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

line#L86:  uint8 tokenDecimals = feeds[token].tokenDecimals;

line#L87:  uint8 decimals = 36 - feedDecimals - tokenDecimals;

line#L120: uint8 tokenDecimals = feeds[token].tokenDecimals;

line#L121: uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L422: function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

line#L486: function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
```

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L13:  uint8 public constant decimals = 18;

line#L220: uint8 v,
```

### Pack storage variables in less slots to save gas

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L38:  /// @audit Currently storage variables are packed in 15 slots but could fit in 14
```

### Do not compare a boolean to `true` or `false`

Directly use the boolean

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

### Cache storage variables in memory

Optimization comes from using MSTORE and MLOAD instead of SSTORE and SLOAD

[Oracle.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol):

```solidity
           /// @audit Cache `feeds`. Used 3 times in `viewPrice`
line#L82:  uint price = feeds[token].feed.latestAnswer();
line#L85:  uint8 feedDecimals = feeds[token].feed.decimals();
line#L86:  uint8 tokenDecimals = feeds[token].tokenDecimals;

           /// @audit Cache `feeds`. Used 3 times in `getPrice`
line#L116: uint price = feeds[token].feed.latestAnswer();
line#L119: uint8 feedDecimals = feeds[token].feed.decimals();
line#L120: uint8 tokenDecimals = feeds[token].tokenDecimals;
```

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
           /// @audit Cache `dbr`. Used 3 times in `forceReplenish`
line#L560: uint deficit = dbr.deficitOf(user);
line#L563: uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
line#L569: dbr.onForceReplenish(user, amount);
```

### Mark functions as `payable` to avoid the low-level evm `is-a-payment-provided` check

The EVM checks whether a payment is provided to the function and this check costs additional gas. If the function is marked as `payable` there is no such check

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L203: function recall(uint amount) public {

line#L212: function pauseBorrows(bool _value) public {
```

[Oracle.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol):

```solidity
line#L66:  function claimOperator() public {
```

[SimpleERC20Escrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol):

```solidity
line#L36:  function pay(address recipient, uint amount) public {
```

[INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol):

```solidity
line#L59:  function pay(address recipient, uint amount) public {

line#L90:  function delegate(address delegatee) public {
```

[Fed.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol):

```solidity
line#L48:  function changeGov(address _gov) public {

line#L57:  function changeSupplyCeiling(uint _supplyCeiling) public {

line#L75:  function resign() public {

line#L86:  function expansion(IMarket market, uint amount) public {
```

[GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol):

```solidity
line#L43:  function pay(address recipient, uint amount) public {

line#L66:  function delegate(address delegatee) public {
```

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L70:  function claimOperator() public {
```

### `x < y + 1` is cheaper than `x <= y`

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L171: require(balanceOf(msg.sender) >= amount, "Insufficient balance");

line#L195: require(balanceOf(from) >= amount, "Insufficient balance");

line#L329: require(deficit >= amount, "Amount > deficit");

line#L373: require(balanceOf(from) >= amount, "Insufficient balance");
```

[Fed.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol):

```solidity
line#L93:  require(globalSupply <= supplyCeiling);

line#L107: require(amount <= supply, "AMOUNT TOO BIG");

line#L123: if(supply >= marketValue) return 0;
```

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

line#L361: if(collateralBalance <= minimumCollateral) return 0;

line#L396: require(credit >= debts[borrower], "Exceeded credit limit");

line#L423: require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

line#L487: require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

line#L533: require(debt >= amount, "Insufficient debt");

line#L567: require(collateralValue >= debts[user], "Exceeded collateral value");

line#L582: if(credit >= debt) return 0;

line#L607: if(escrow.balance() >= liquidationFee) {
```

### Use `constant` and `immutable` keywords for storage varibles if they doesn't change through contract's lifecycle

That way the variable will go to the contract's bytecode and will not allocate a storage slot

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L11:  string public name;

line#L12:  string public symbol;
```

### Do not use strings longer than 32 bytes

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L76:  require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

line#L214: require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L63:  require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

line#L326: require(markets[msg.sender], "Only markets can call onForceReplenish");
```

### Use `private` visibility modifiers instead of `public` for storage `constant`s

Saves 3000+ gas per variable when deploying contract

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L13:  uint8 public constant decimals = 18;
```

### Use `if (x != 0)` instead of `if (x > 0)` for uints comparison

Saves 3 gas per `if`-check

[DBR.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol):

```solidity
line#L63:  require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

line#L328: require(deficit > 0, "No deficit");
```

[Fed.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol):

```solidity
line#L133: if(profit > 0) {
```

[Oracle.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol):

```solidity
line#L83:  require(price > 0, "Invalid feed price");

line#L96:  uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

line#L117: require(price > 0, "Invalid feed price");

line#L135: uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
```

[Market.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol):

```solidity
line#L75:  require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

line#L162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

line#L184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

line#L195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

line#L592: require(repaidDebt > 0, "Must repay positive debt");
```

[INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol):

```solidity
line#L81:  if(invBalance > 0) {
```
