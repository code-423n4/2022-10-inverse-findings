# QA report

## Low Risk
| L-N    |Issue|Instances|
|:------:|:----|:-------:|
| [L&#x2011;01] | Open TODO | 1 |

## Non-critical

| N-N    |Issue|Instances|
|:------:|:----|:-------:|
| [N&#x2011;01] | Require without custom errors | 3 |
| [N&#x2011;02] | Non-library/interface files should use fixed compiler versions, not floating ones | 8 |
| [N&#x2011;03] | Each interface should be declared in separate files | 13 |
| [N&#x2011;04] | Lint |  |
| [N&#x2011;05] | Constants should be defined and documented rather than using magic numbers | 25 |
| [N&#x2011;06] | Wrong order in contracts | 8 |
| [N&#x2011;07] | Critical functions don't emit events | 25 |

## Low Risk

### [L-01] Open TODO

Open TODO can point to architecture or programming issues that still need to be resolved.

```solidity
File: src/escrows/INVEscrow.sol

35        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```


## Non-critical

### [N-01] Require without custom errors

Use custom errors to give the idea what was the cause of failure

```solidity
File: src/escrows/GovTokenEscrow.sol

67        require(msg.sender == beneficiary);
```

```solidity
File: src/escrows/INVEscrow.sol

91        require(msg.sender == beneficiary);
```

```solidity
File: src/Fed.sol

93        require(globalSupply <= supplyCeiling);
```

### [N-02] Non-library/interface files should use fixed compiler versions, not floating ones

```solidity
File: src/BorrowController.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/DBR.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/Market.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/Oracle.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/Fed.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/escrows/SimpleERC20Escrow.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/escrows/INVEscrow.sol

2 pragma solidity ^0.8.13;
```

```solidity
File: src/escrows/GovTokenEscrow.sol

2 pragma solidity ^0.8.13;
```

### [N-03] Each interface should be declared in separate files

```solidity
File: src/Market.sol

 5 interface IERC20 {
 6     function transfer(address,uint) external returns (bool);
 7     function transferFrom(address,address,uint) external returns (bool);
 8     function balanceOf(address) external view returns (uint);
 9 }

11 interface IOracle {
12     function getPrice(address,uint) external returns (uint);
13     function viewPrice(address,uint) external view returns (uint);
14 }

16 interface IEscrow {
17     function initialize(IERC20 _token, address beneficiary) external;
18     function onDeposit() external;
19     function pay(address recipient, uint amount) external;
20     function balance() external view returns (uint);
21 }

23 interface IDolaBorrowingRights {
24     function onBorrow(address user, uint additionalDebt) external;
25     function onRepay(address user, uint repaidDebt) external;
26     function onForceReplenish(address user, uint amount) external;
27     function balanceOf(address user) external view returns (uint);
28     function deficitOf(address user) external view returns (uint);
29     function replenishmentPriceBps() external view returns (uint);
30 }

32 interface IBorrowController {
33     function borrowAllowed(address msgSender, address borrower, uint amount) external returns (bool);
34 }
```

```solidity
File: src/Oracle.sol

4 interface IChainlinkFeed {
5     function decimals() external view returns (uint8);
6     function latestAnswer() external view returns (uint);
7 }
```

```solidity
File: src/Fed.sol

 4 interface IMarket {
 5     function recall(uint amount) external;
 6     function totalDebt() external view returns (uint);
 7     function borrowPaused() external view returns (bool);
 8 }

10 interface IDola {
11     function mint(address to, uint amount) external;
12     function burn(uint amount) external;
13     function balanceOf(address user) external view returns (uint);
14     function transfer(address to, uint amount) external returns (bool);
15 }

17 interface IDBR {
18     function markets(address) external view returns (bool);
19 }
```

```solidity
File: src/escrows/SimpleERC20Escrow.sol

5 interface IERC20 {
6     function transfer(address,uint) external returns (bool);
7     function transferFrom(address,address,uint) external returns (bool);
8     function balanceOf(address) external view returns (uint);
9 }
```

```solidity
File: src/escrows/INVEscrow.sol

 5 interface IERC20 {
 6     function transfer(address,uint) external returns (bool);
 7     function transferFrom(address,address,uint) external returns (bool);
 8     function balanceOf(address) external view returns (uint);
 9     function approve(address, uint) external view returns (uint);
10     function delegate(address delegatee) external;
11     function delegates(address delegator) external view returns (address delegatee);
12 }

14 interface IXINV {
15     function balanceOf(address) external view returns (uint);
16     function exchangeRateStored() external view returns (uint);
17     function mint(uint mintAmount) external returns (uint);
18     function redeemUnderlying(uint redeemAmount) external returns (uint);
19     function syncDelegate(address user) external;
20 }
```

```solidity
File: src/escrows/GovTokenEscrow.sol

 5 interface IERC20 {
 7     function transfer(address,uint) external returns (bool);
 8     function transferFrom(address,address,uint) external returns (bool);
 9     function balanceOf(address) external view returns (uint);
10     function delegate(address delegatee) external;
11     function delegates(address delegator) external view returns (address delegatee);
12 }
```

### [N-04] Lint

Remove space:

```solidity
File: src/BorrowController.sol

9 \n
```

```solidity
File: src/DBR.sol

10 \n
```

```solidity
File: src/Market.sol

37 \n
```

```solidity
File: src/Oracle.sol

 17 \n

148 \n
```

```solidity
File: src/Fed.sol

 27 \n

139 \n

142 \n
```

### [N-05] Constants should be defined and documented rather than using magic numbers

```solidity
File: src/Oracle.sol

 87            uint8 decimals = 36 - feedDecimals - tokenDecimals;

 95            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

 98                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;

121            uint8 decimals = 36 - feedDecimals - tokenDecimals;

134            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

137                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
```

```solidity
File: src/Market.sol

74        require(_collateralFactorBps < 10000, "Invalid collateral factor");

75        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

76        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

150        require(_collateralFactorBps < 10000, "Invalid collateral factor");

162        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

173        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

195        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

336        return collateralValue * collateralFactorBps / 10000;

346        return collateralValue * collateralFactorBps / 10000;

360        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

377        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

563        uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;

564        uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;

583        return debt * liquidationFactorBps / 10000;

595        require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

598        liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

606            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
```

```solidity
File: DBR.sol

330        uint replenishmentCost = amount * replenishmentPriceBps / 10000;
```

### [N-06] Wrong order in contracts

The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions
Suggestion: Consider reorder the events and modifiers

```solidity
File: src/BorrowController.sol

17    modifier onlyOperator {
18        require(msg.sender == operator, "Only operator");
19        _;
20    }
```

```solidity
File: DBR.sol

 44    modifier onlyOperator {
 45        require(msg.sender == operator, "ONLY OPERATOR");
 46        _;
 47    }

381    event Transfer(address indexed from, address indexed to, uint256 amount);
382    event Approval(address indexed owner, address indexed spender, uint256 amount);
383    event AddMinter(address indexed minter);
384    event RemoveMinter(address indexed minter);
385    event AddMarket(address indexed market);
386    event ChangeOperator(address indexed newOperator);
```

```solidity
File: src/Market.sol

 92    modifier onlyGov {
 93        require(msg.sender == gov, "Only gov can call this function");
 94        _;
 95    }

614    event Deposit(address indexed account, uint amount);
615    event Borrow(address indexed account, uint amount);
616    event Withdraw(address indexed account, address indexed to, uint amount);
617    event Repay(address indexed account, address indexed repayer, uint amount);
618    event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);
619    event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
620    event CreateEscrow(address indexed user, address escrow);
```

```solidity
File: src/Oracle.sol

 35    modifier onlyOperator {
 37        require(msg.sender == operator, "ONLY OPERATOR");
 38        _;
 39    }

146    event ChangeOperator(address indexed newOperator);
147    event RecordDailyLow(address indexed token, uint price);
```

```solidity
File: src/Fed.sol

140    event Expansion(IMarket indexed market, uint amount);
141    event Contraction(IMarket indexed market, uint amount);
```

### [N-07] Critical functions don't emit events

Each function that changes the state of the contract should have an associated event to facilitate off-chain monitoring

```solidity
File: src/BorrowController.sol

26    function setOperator(address _operator) public onlyOperator { operator = _operator; }

32    function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

38    function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```

```solidity
File: src/DBR.sol

 53    function setPendingperator(address newOperator_) public onlyOperator {

 62    function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

259    function invalidateNonce() public {
```

```solidity
File: src/Market.sol

118    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

124    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

130    function setGov(address _gov) public onlyGov { gov = _gov; }

136    function setLender(address _lender) public onlyGov { lender = _lender; }

142    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }

149    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

212    function pauseBorrows(bool _value) public {

520    function invalidateNonce() public {
```

```solidity
File: src/Oracle.sol

44    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

53    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

61    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

```solidity
File: src/Fed.sol

48    function changeGov(address _gov) public {

57    function changeSupplyCeiling(uint _supplyCeiling) public {

66    function changeChair(address _chair) public {

75    function resign() public {
```
