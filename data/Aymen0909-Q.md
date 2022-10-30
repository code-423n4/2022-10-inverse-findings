# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1     | `changeSupplyCeiling` should check that `_supplyCeiling >= globalSupply` when setting a new ceiling | Low | 1 |
| 2     | Change of operator in `BorrowController` should use two-step transfer pattern | Low | 1 |
| 3     | `immutable` state variables lack zero value (address or uint) checks | Low | 5 |
| 4     | `require()`/`revert()` statements should have descriptive reason strings | NC | 1 |
| 5     | Event should be emitted in setters | NC | 8 |
| 6     | Duplicated `require()` checks should be refactored to a modifier | NC | 7 |
| 7     | `public` functions not called by the contract should be declared `external` instead  | NC | 40 |


## Findings

### 1- `changeSupplyCeiling` should check that `_supplyCeiling >= globalSupply` when setting a new ceiling :

The `changeSupplyCeiling` function inside `Fed` contract is called by the governance to set a new ceiling, but the function doesn't check that the new ceiling is lower than the current `globalSupply`, so if a new supplyCeiling greater than the current global supply is set this can temporarily block the `expansion` function and thus the supply to the borrow market.

#### Risk : Low 

#### Proof of Concept

The `changeSupplyCeiling` function below doesn't check the new value for the supply ceiling (called `_supplyCeiling`) : 

```
function changeSupplyCeiling(uint256 _supplyCeiling) public {
    require(msg.sender == gov, "ONLY GOV");
    supplyCeiling = _supplyCeiling;
}
```

So if the new set `supplyCeiling` is lower than the current `globalSupply` this will block the calls to the `expansion` function because of the following check :
```
require(globalSupply <= supplyCeiling);
```

The consequence of this is that DOLA tokens cannot be minted to the borrow markets until a new voting occurs to set a new `supplyCeiling` (which could take some time depending on the voting period), or the `chair` will be forced to call `contraction` function to decrease the current `globalSupply` but this can only work if the borrow market has sufficient DOLA balance.

#### Mitigation
Add a check inside the `changeSupplyCeiling` function to avoid this issue, the function should be modified as follow :

```
function changeSupplyCeiling(uint256 _supplyCeiling) public {
    require(msg.sender == gov, "ONLY GOV");
    require(globalSupply <= _supplyCeiling, "Invalid supply ceiling");
    supplyCeiling = _supplyCeiling;
}
```

### 2- change of operator in `BorrowController` should use two-step transfer pattern :

The change of operator in the `BorrowController` contract is done through a simple setter function but in the `Oracle` contract this is done with a 2-step transfer, the operator in both cases has a big role in setting/changing critical protocol parameters. So i recommend to also use the 2-step transfer pattern in the `BorrowController` contract to have more security when changing the operator.

#### Risk : Low

#### Proof of Concept

Instances include:

File: src/BorrowController.sol

[function setOperator(address _operator)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)

#### Mitigation
Consider using 2-step transfer pattern when setting a new operator in the `BorrowController` contract.

### 3- Immutable state variables lack zero value (address or uint) checks  :

Constructors should check that the values written in an immutable state variables variable are not zero (for uint) or the zero address (for addresses).

#### Risk : Low 

#### Proof of Concept

Instances include:

File: src/Fed.sol

[dbr = _dbr;](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L37)

[dola = _dola;](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L38)

File: src/Market.sol

[escrowImplementation = _escrowImplementation;](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80)

[dbr = _dbr;](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L81)

[collateral = _collateral;](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L82)

#### Mitigation
Add non-zero address checks in the constructors for the instances aforementioned.

### 4- `require()`/`revert()` statements should have descriptive reason strings :

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:
 
File: src/Fed.sol [Line 93](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93)

```
require(globalSupply <= supplyCeiling);
```

#### Mitigation
Add reason strings to the aforementioned require statements for better comprehension.

### 5- Event should be emitted in setters :

Setters should emit an event so that Dapps and users can detect important changes to critical protocol parameters like : fees, collateral factor... 

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: src/DBR.sol

[function setReplenishmentPriceBps(uint newReplenishmentPriceBps_)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62-L65)

File: src/Market.sol

[function setCollateralFactorBps(uint _collateralFactorBps)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149-L152)

[function setLiquidationFactorBps(uint _liquidationFactorBps)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161-L164)

[function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172-L175)

[function setLiquidationIncentiveBps(uint _liquidationIncentiveBps)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183-L186)

[function setLiquidationFeeBps(uint _liquidationFeeBps)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194-L197)

File: src/Oracle.sol

[function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53)

[function setFixedPrice(address token, uint price)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61)

#### Mitigation
Emit an event in all the aforementioned setters.

### 6- Duplicated `require()` checks should be refactored to a modifier :

`require()` checks statement used multiple times inside a contract should be refactored to a modifier for better readability.

#### Risk : NON CRITICAL

#### Proof of Concept

There are 3 instances of this issue in `Fed.sol`:

```
File: src/Fed.sol

49      require(msg.sender == gov, "ONLY GOV");
58      require(msg.sender == gov, "ONLY GOV");
67      require(msg.sender == gov, "ONLY GOV");
```

Those checks should be replaced by a `onlyGov` modifier as follow :

```
modifier onlyGov(){
    require(msg.sender == gov, "ONLY GOV");
    _;
}
```

There are another 3 instances of this issue in `Fed.sol`:

```
File: src/Fed.sol

76      require(msg.sender == chair, "ONLY CHAIR");
87      require(msg.sender == chair, "ONLY CHAIR");
104     require(msg.sender == chair, "ONLY CHAIR");
```

Those checks should be replaced by a `onlyChair` modifier as follow :

```
modifier onlyChair(){
    require(msg.sender == chair, "ONLY CHAIR");
    _;
}
```

### 7- `public` functions not called by the contrat should be declared `external` instead  :

#### Impact - NON CRITICAL

#### Proof of Concept
There are many instances across the contracts :

File: src/BorrowController.sol

[function setOperator(address _operator) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)

[function allow(address allowedContract) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32)

[function deny(address deniedContract) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38)

[function borrowAllowed(address msgSender, address, uint) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46)

File: src/DBR.sol

[function setPendingOperator(address newOperator_) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53)

[function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62)

[function claimOperator() public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70)

[function addMinter(address minter_) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81)

[function removeMinter(address minter_) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90)

[function addMarket(address market_) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99)

[function totalSupply() public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109)

[function signedBalanceOf(address user) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146)

[function approve(address spender, uint256 amount) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158)

[function onBorrow(address user, uint additionalDebt) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300)

[function onForceReplenish(address user, uint amount) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325)

File: src/Fed.sol

[function changeGov(address _gov) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48)

[function changeSupplyCeiling(uint _supplyCeiling) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57)

[function changeChair(address _chair) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66)

[function resign() public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75)

[function expansion(IMarket market, uint amount) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103)

[function contraction(IMarket market, uint amount) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L120)

[function takeProfit(IMarket market) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131)

File: src/Market.sol

[function setOracle(IOracle _oracle) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118)

[function setBorrowController(IBorrowController _borrowController)](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124)

[function setGov(address _gov) public ](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130)

[function setLender(address _lender) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136)

[function setPauseGuardian(address _pauseGuardian) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142)

[function setCollateralFactorBps(uint _collateralFactorBps) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149)

[function setLiquidationFactorBps(uint _liquidationFactorBps) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161)

[function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172)

[function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183)

[function setLiquidationFeeBps(uint _liquidationFeeBps) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194)

[function recall(uint amount) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203)

[function pauseBorrows(bool _value) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212)

[function forceReplenish(address user, uint amount) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559)

[function liquidate(address user, uint repaidDebt) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591)

File: src/Market.sol

[function setPendingOperator(address newOperator_) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44)

[function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53)

[function setFixedPrice(address token, uint price) public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61)

[function claimOperator() public](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66)

#### Mitigation

Declare the functions aforementioned external if they are not intended to be called inside the contract in the future.
