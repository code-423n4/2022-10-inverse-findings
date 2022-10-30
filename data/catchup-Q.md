# Low Risk Issues

## 1. Missing non-zero amount check for token interactions

```DBR.sol```
~~~
158:     function approve(address spender, uint256 amount) public virtual returns (bool) {
159:         allowance[msg.sender][spender] = amount;        //@audit-issue low non-zero amount check
160:         emit Approval(msg.sender, spender, amount);
161:         return true;
162:     }
163: 
~~~
~~~
170:     function transfer(address to, uint256 amount) public virtual returns (bool) {
171:         require(balanceOf(msg.sender) >= amount, "Insufficient balance");
172:         balances[msg.sender] -= amount;
173:         unchecked {
174:             balances[to] += amount;
175:         }
176:         emit Transfer(msg.sender, to, amount);
177:         return true;
178:     }
~~~
~~~
359:     function _mint(address to, uint256 amount) internal virtual {
360:         _totalSupply += amount;     //@audit-issue low non-zero amount check
361:         unchecked {
362:             balances[to] += amount;
363:         }
364:         emit Transfer(address(0), to, amount);
365:     }
~~~
~~~
372:     function _burn(address from, uint256 amount) internal virtual {
373:         require(balanceOf(from) >= amount, "Insufficient balance");     //@audit-issue low non-zero amount check
374:         balances[from] -= amount;
375:         unchecked {
376:             _totalSupply -= amount;
377:         }
378:         emit Transfer(from, address(0), amount);
379:     }
~~~

```Market.sol```
~~~
460:     function withdrawInternal(address from, address to, uint amount) internal {
461:         uint limit = getWithdrawalLimitInternal(from);
462:         require(limit >= amount, "Insufficient withdrawal limit");        //@audit-issue low non-zero amount check
463:         IEscrow escrow = getEscrow(from);
464:         escrow.pay(to, amount);
465:         emit Withdraw(from, to, amount);
466:     }
~~~
~~~
531:     function repay(address user, uint amount) public {
532:         uint debt = debts[user];        //@audit-issue low non-zero amount check
533:         require(debt >= amount, "Insufficient debt");
534:         debts[user] -= amount;
535:         totalDebt -= amount;
536:         dbr.onRepay(user, amount);
537:         dola.transferFrom(msg.sender, address(this), amount);
538:         emit Repay(user, msg.sender, amount);
539:     }
~~~
~~~
559:     function forceReplenish(address user, uint amount) public {
560:         uint deficit = dbr.deficitOf(user);
561:         require(deficit > 0, "No DBR deficit");
562:         require(deficit >= amount, "Amount > deficit");        //@audit-issue low non-zero amount check
563:         uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
564:         uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;
565:         debts[user] += replenishmentCost;
566:         uint collateralValue = getCollateralValueInternal(user);
567:         require(collateralValue >= debts[user], "Exceeded collateral value");
568:         totalDebt += replenishmentCost;
569:         dbr.onForceReplenish(user, amount);
570:         dola.transfer(msg.sender, replenisherReward);
571:         emit ForceReplenish(user, msg.sender, amount, replenishmentCost, replenisherReward);
572:     }
~~~




## 2. Missing non-zero address checks when setting important addresses

```DBR.sol```
~~~
30:     constructor(
31:         uint _replenishmentPriceBps,
32:         string memory _name,
33:         string memory _symbol,
34:         address _operator
35:     ) {
36:         replenishmentPriceBps = _replenishmentPriceBps;
37:         name = _name;
38:         symbol = _symbol;
39:         operator = _operator;      					//@audit-issue low non-zero address check for priviliged role
40:         INITIAL_CHAIN_ID = block.chainid;
41:         INITIAL_DOMAIN_SEPARATOR = computeDomainSeparator();
42:     }
~~~

```Market.sol```
~~~
61:     constructor (
62:         address _gov,
63:         address _lender,
64:         address _pauseGuardian,
65:         address _escrowImplementation,
66:         IDolaBorrowingRights _dbr,
67:         IERC20 _collateral,
68:         IOracle _oracle,
69:         uint _collateralFactorBps,
70:         uint _replenishmentIncentiveBps,
71:         uint _liquidationIncentiveBps,
72:         bool _callOnDepositCallback
73:     ) {
74:         require(_collateralFactorBps < 10000, "Invalid collateral factor");
75:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
76:         require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
77:         gov = _gov;      					//@audit-issue low non-zero address check for priviliged role
78:         lender = _lender;
79:         pauseGuardian = _pauseGuardian;
80:         escrowImplementation = _escrowImplementation;
81:         dbr = _dbr;
82:         collateral = _collateral;
83:         oracle = _oracle;
84:         collateralFactorBps = _collateralFactorBps;
85:         replenishmentIncentiveBps = _replenishmentIncentiveBps;
86:         liquidationIncentiveBps = _liquidationIncentiveBps;
87:         callOnDepositCallback = _callOnDepositCallback;
88:         INITIAL_CHAIN_ID = block.chainid;
89:         INITIAL_DOMAIN_SEPARATOR = computeDomainSeparator();
90:     }
~~~


```Operator.sol```
~~~
29:     constructor(
30:         address _operator
31:     ) {
32:         operator = _operator;      					//@audit-issue low non-zero address check for priviliged role
33:     }
~~~

```Fed.sol```
~~~
36:     constructor (IDBR _dbr, IDola _dola, address _gov, address _chair, uint _supplyCeiling) {
37:         dbr = _dbr;
38:         dola = _dola;
39:         gov = _gov;      					//@audit-issue low non-zero address check for priviliged role
40:         chair = _chair;
41:         supplyCeiling = _supplyCeiling;
42:     }
~~~



	
# Non-Critical Issues

## 1. Typos in comments
```DBR.sol```
~~~
50:     @notice Sets pending operator of the contract. Operator role must be claimed by the new oprator. Only callable by Operator. //@audit-issue NC typo oprator->operator
~~~
~~~
60:     @param newReplenishmentPriceBps_ The new replen     //@audit-issue NC typo replen->replenisment
~~~
~~~
87:     @notice Removes a minter from the set of addresses allowe to mint DBR tokens. Only callable by Operator.    //@audit-issue NC typo allowe->allowed
~~~
~~~
128:     @notice Get the DBR deficit of an address. Will return 0 if th user has zero DBR or more.   //@audit-issue NC typo th->the
~~~

```Market.sol```
~~~
201:     @param amount The amount od DOLA to recall to the the lender.       //@audit-issue NC typo od->of
~~~
~~~
589:     @param repaidDebt Th amount of user user debt to liquidate.     //@audit-issue NC typo Th->The and double user
~~~


## 2. Public functions not called by the contract should be declared external
```DBR.sol```
~~~
53:     function setPendingOperator(address newOperator_) public onlyOperator {     //@audit-issue NC public functions not called by the contract should be declared external instead.
54:         pendingOperator = newOperator_;
55:     }
~~~

~~~
62:     function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {     //@audit-issue NC public functions not called by the contract should be declared external instead.
63:         require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
64:         replenishmentPriceBps = newReplenishmentPriceBps_;
65:     }
~~~

~~~
70:     function claimOperator() public {     //@audit-issue NC public functions not called by the contract should be declared external instead.
71:         require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
72:         operator = pendingOperator;
73:         pendingOperator = address(0);
74:         emit ChangeOperator(operator);
75:     }
~~~


## 3. Floating pragma is used
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
It is recommended to use the same solidity version for all the contracts.






