## L-01 NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Proof of Concept

This issue exists on all In-scope contracts

For instance:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L2

```
File: src/DBR.sol

2: pragma solidity ^0.8.13;
```

### Recommended Mitigation Steps

Lock the pragma version

-----------------

## L-02 RACE CONDITION IN APPROVE()

Using approve() opens yourself and users of the token up to frontrunning.

### Proof of Concept

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L50

```
File: src/escrows/INVEscrow.sol#L50

50: _token.approve(address(xINV), type(uint).max);
```

### Recommendation:

Add increaseAllowance and decreaseAllowance methods in ERC20 contract

-----------

## L-03 SAFETRANSFERFROM() IS RECOMMENDED INSTEAD OF TRANSFERFROM()

ERC20 standard allows transfer function of some contracts to return bool or return nothing.  
Some tokens such as USDT return nothing.  
This could lead to funds stuck in the contract without possibility to retrieve them.  
Using safeTransferFrom of SafeERC20.sol is recommended instead.  

### Proof of Concept

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

280: collateral.transferFrom(msg.sender, address(escrow), amount);
537: dola.transferFrom(msg.sender, address(this), amount);
602: dola.transferFrom(msg.sender, address(this), repaidDebt);
```

------------------

## N-01 EVENT IS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fields

_There are 12 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

381: event Transfer(address indexed from, address indexed to, uint256 amount);
382: event Approval(address indexed owner, address indexed spender, uint256 amount);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
File: src/Fed.sol

140: event Expansion(IMarket indexed market, uint amount);
141: event Contraction(IMarket indexed market, uint amount);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

614: event Deposit(address indexed account, uint amount);
615: event Borrow(address indexed account, uint amount);
616: event Withdraw(address indexed account, address indexed to, uint amount);
617: event Repay(address indexed account, address indexed repayer, uint amount)
618: event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);
619: event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
620: event CreateEscrow(address indexed user, address escrow);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L147

```
File: src/Oracle.sol

147: event RecordDailyLow(address indexed token, uint price);
```

---------------

## N-02 REQUIRE() OR REVERT() STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93

```
File: src/Fed.sol

93: require(globalSupply <= supplyCeiling);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67

```
File: src/escrows/GovTokenEscrow.sol

67: require(msg.sender == beneficiary);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L91

```
File: src/escrows/INVEscrow.sol

91: require(msg.sender == beneficiary);
```

------------

## N-03 OPEN TODOS

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35

```
File: src/escrows/INVEscrow.sol

35: xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```

--------

## N-04 INTERFACES SHOULD BE MOVED TO SEPARATE FILES

_There are 13 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
File: src/Fed.sol

4: interface IMarket
10: interface IDola
17: interface IDBR
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
File: src/Market.sol

5: interface IERC20
11: interface IOracle
16: interface IEscrow
23: interface IDolaBorrowingRights
32: interface IBorrowController
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
File: src/Oracle.sol

4: interface IChainlinkFeed
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol

```
File: src/escrows/GovTokenEscrow.sol

5: interface IERC20
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol

```
File: src/escrows/INVEscrow.sol

5: interface IERC20
14: interface IXINV
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol

```
File: src/escrows/SimpleERC20Escrow.sol

5: interface IERC20
```

-------------

## N-05 MISSING EVENTS
 
_There are 2 instances of this issue_

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
File: src/DBR.sol

53-55: function setPendingOperator(address newOperator_) public onlyOperator {
        pendingOperator = newOperator_;
    }
62-65: function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
        require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");
        replenishmentPriceBps = newReplenishmentPriceBps_;
    }
```