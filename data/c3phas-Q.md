## QA

### Missing checks for address(0x0) when assigning values to address state variables

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L77-L80
```solidity
File: /src/Market.sol
77:        gov = _gov;
78:        lender = _lender;
79:        pauseGuardian = _pauseGuardian;
80:        escrowImplementation = _escrowImplementation;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L32
```solidity
File: /src/Oracle.sol
32:        operator = _operator;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L39-L40
```solidity
File: /src/Fed.sol
39:        gov = _gov;
40:        chair = _chair;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L14
```solidity
File: /src/BorrowController.sol
14:        operator = _operator;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L39
```solidity
File: /src/DBR.sol
39:        operator = _operator;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L34
```solidity
File: /src/escrows/GovTokenEscrow.sol
34:        beneficiary = _beneficiary;
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L48
```solidity
File: /src/escrows/INVEscrow.sol
48:        beneficiary = _beneficiary;
```

### Constants should be defined rather than using magic numbers
There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. 

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L74
```solidity
File: /src/Market.sol

//@audit: 10000
74:        require(_collateralFactorBps < 10000, "Invalid collateral factor");

//@audit: 10000
75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

//@audit: 10000
76:        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

//@audit: 10000
150:        require(_collateralFactorBps < 10000, "Invalid collateral factor");

//@audit: 10000
162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

//@audit: 10000
173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

//@audit: 10000
184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

//@audit: 10000
195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

//@audit: 10000
336:        return collateralValue * collateralFactorBps / 10000;

//@audit: 10000
346:        return collateralValue * collateralFactorBps / 10000;

//@audit: 10000
360:        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

//@audit: 10000
377:        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

//@audit: 10000
563:        uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;

//@audit: 10000
564:        uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;

//@audit: 10000
583:        return debt * liquidationFactorBps / 10000;

//@audit: 10000
595:        require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

//@audit: 10000
598:        liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

//@audit: 10000
606:            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L87
```solidity
File: /src/Oracle.sol

//@audit: 36
87:            uint8 decimals = 36 - feedDecimals - tokenDecimals;

//@audit: 10
88:            uint normalizedPrice = price * (10 ** decimals);

//@audit: 10000
95:            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

//@audit: 10000
98:                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;

//@audit: 36
121:            uint8 decimals = 36 - feedDecimals - tokenDecimals;

//@audit: 10
122:            uint normalizedPrice = price * (10 ** decimals);

//@audit: 10000
134:            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

//@audit: 10000
137:                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L122
```solidity
File:/src/DBR.sol

//@audit: 365 days
122:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

//@audit: 365 days
135:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

//@audit: 365 days
148:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

//@audit: 365 days
287:        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

//@audit: 10000
330:        uint replenishmentCost = amount * replenishmentPriceBps / 10000;


```

### Public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents' functions and change the visibility from external to public.
Functions marked by external use call data to read arguments, where public will first allocate in local memory and then load them.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L118
```solidity
File: /src/Market.sol
118:    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

124:    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

130:    function setGov(address _gov) public onlyGov { gov = _gov; }

136:    function setLender(address _lender) public onlyGov { lender = _lender; }

142:    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }

149:    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161:    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172:    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183:    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194:    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

212:    function pauseBorrows(bool _value) public {

267:    function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {

370:    function getWithdrawalLimit(address user) public view returns (uint) {

422:    function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

486:    function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

520:    function invalidateNonce() public {

546:    function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {

578:    function getLiquidatableDebt(address user) public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44
```solidity
File: /src/Oracle.sol
44:    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

61:    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }

66:    function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48
```solidity
File: /src/Fed.sol
48:     function changeGov(address _gov) public {

57:    function changeSupplyCeiling(uint _supplyCeiling) public {

66:    function changeChair(address _chair) public {

75:    function resign() public {

86:    function expansion(IMarket market, uint amount) public {

103:    function contraction(IMarket market, uint amount) public {

131:    function takeProfit(IMarket market) public {
```

### Open todos
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L35
```solidity
File: /src/escrows/INVEscrow.sol
35:        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```

### Missing friendly revert strings
 require()/revert() statements should have descriptive reason strings.
 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L93
```solidity
File: /src/Fed.sol
93:        require(globalSupply <= supplyCeiling);
```

### Missing event for critical initialize function
Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L30-L36
```solidity
File: /src/escrows/GovTokenEscrow.sol
30:    function initialize(IERC20 _token, address _beneficiary) public {

36:    }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L44-L52
```solidity
File: /src/escrows/INVEscrow.sol
44:    function initialize(IERC20 _token, address _beneficiary) public {

52:    }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L25-L29
```solidity
File: /src/escrows/SimpleERC20Escrow.sol
25:    function initialize(IERC20 _token, address) public {

29:    }
```
### Missing events for setters
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L118
```solidity
File: /src/Market.sol
118:    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

124:    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

130:    function setGov(address _gov) public onlyGov { gov = _gov; }

136:    function setLender(address _lender) public onlyGov { lender = _lender; }

142:    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }


149:  function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
150:        require(_collateralFactorBps < 10000, "Invalid collateral factor");
151:        collateralFactorBps = _collateralFactorBps;
152:    }


161:    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
163:        liquidationFactorBps = _liquidationFactorBps;
164:    }


172:    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
174:        replenishmentIncentiveBps = _replenishmentIncentiveBps;
175:    }


183:    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
185:        liquidationIncentiveBps = _liquidationIncentiveBps;
186:    }


194:    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
196:        liquidationFeeBps = _liquidationFeeBps;
197:    }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44
```solidity
File: /src/Oracle.sol
44:     function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

53:    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

61:    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }

```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26
```solidity
File: /src/BorrowController.sol
26:     function setOperator(address _operator) public onlyOperator { operator = _operator; }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53
```solidity
File: /src/DBR.sol
53:    function setPendingOperator(address newOperator_) public onlyOperator {

62:    function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

```
### Lack of event emission after critical functions
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48
```solidity
File: /src/Fed.sol
48:     function changeGov(address _gov) public {

57:    function changeSupplyCeiling(uint _supplyCeiling) public {

66:    function changeChair(address _chair) public {
```

### Inconsistency in using uint vs uint256
`uint` is simply an alias for `uint256` . Throughout the code , the use of `uint` seems to be the preference. 

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L58-L59
```solidity
File: /src/Market.sol
58:    mapping (address => uint) public debts; // user => debt
59:    mapping(address => uint256) public nonces; // user => nonce
```

Since  majority of the code is utilizing `uint` we should utilize this in all the codebase for consistency

### Code Structure Deviates From Best-Practice
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier. Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Some constructs deviate from this recommended best-practice: Modifiers are in the middle of contracts. External/public functions are mixed with internal/private ones.

In the  following, events are declared at the end of the file.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L614-L620
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L146-L147
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L140-L141
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L381-L386


We have internal functions coming before public ones
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L460


 **Recommendation:**
Consider adopting recommended best-practice for code structure and layout.

See: [https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-layout](https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-layout)



