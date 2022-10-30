### L-01 NO EVENT IS RAISED

All important functions wherever some change is happening need to raise events

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
53: function setPendingOperator(address newOperator_) public onlyOperator {
62: function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
```

### L-02 FLOATING PRAGMA VERSION SHOULD NOT BE USED

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

This is applicable in all the smart contracts

```
pragma solidity ^0.8.13;
```



### N-01 INTERFACES SHOULD BE MOVED TO SEPARATE FILES

Most of the other interfaces in this project are in their own file in the interfaces directory. The interfaces below do not follow this pattern

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
interface IMarket { 
    function recall(uint amount) external;
    function totalDebt() external view returns (uint);
    function borrowPaused() external view returns (bool);
}

interface IDola { 
    function mint(address to, uint amount) external;
    function burn(uint amount) external;
    function balanceOf(address user) external view returns (uint);
    function transfer(address to, uint amount) external returns (bool);
}

interface IDBR {
    function markets(address) external view returns (bool);
}
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
interface IERC20 {
    function transfer(address,uint) external returns (bool);
    function transferFrom(address,address,uint) external returns (bool);
    function balanceOf(address) external view returns (uint);
}

interface IOracle {
    function getPrice(address,uint) external returns (uint);
    function viewPrice(address,uint) external view returns (uint);
}

interface IEscrow {
    function initialize(IERC20 _token, address beneficiary) external;
    function onDeposit() external;
    function pay(address recipient, uint amount) external;
    function balance() external view returns (uint);
}

interface IDolaBorrowingRights {
    function onBorrow(address user, uint additionalDebt) external;
    function onRepay(address user, uint repaidDebt) external;
    function onForceReplenish(address user, uint amount) external;
    function balanceOf(address user) external view returns (uint);
    function deficitOf(address user) external view returns (uint);
    function replenishmentPriceBps() external view returns (uint);
}

interface IBorrowController {
    function borrowAllowed(address msgSender, address borrower, uint amount) external returns (bool);
}
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
interface IChainlinkFeed {
    function decimals() external view returns (uint8);
    function latestAnswer() external view returns (uint);
}
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol

```
interface IERC20 {
    function transfer(address,uint) external returns (bool);
    function transferFrom(address,address,uint) external returns (bool);
    function balanceOf(address) external view returns (uint);
    function delegate(address delegatee) external;
    function delegates(address delegator) external view returns (address delegatee);
}
```


https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol

```
interface IERC20 {
    function transfer(address,uint) external returns (bool);
    function transferFrom(address,address,uint) external returns (bool); 
    function balanceOf(address) external view returns (uint);
    function approve(address, uint) external view returns (uint);
    function delegate(address delegatee) external;
    function delegates(address delegator) external view returns (address delegatee);
}

interface IXINV {
    function balanceOf(address) external view returns (uint);
    function exchangeRateStored() external view returns (uint);
    function mint(uint mintAmount) external returns (uint);
    function redeemUnderlying(uint redeemAmount) external returns (uint);
    function syncDelegate(address user) external;
}
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol

```
interface IERC20 {
    function transfer(address,uint) external returns (bool);
    function transferFrom(address,address,uint) external returns (bool);
    function balanceOf(address) external view returns (uint);
}
```



### N-02 EVENTS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fields

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
381: event Transfer(address indexed from, address indexed to, uint256 amount);
382: event Approval(address indexed owner, address indexed spender, uint256 amount);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
140: event Expansion(IMarket indexed market, uint amount);
141: event Contraction(IMarket indexed market, uint amount);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```
614: event Deposit(address indexed account, uint amount);
615: event Borrow(address indexed account, uint amount);
616: event Withdraw(address indexed account, address indexed to, uint amount);
617: event Repay(address indexed account, address indexed repayer, uint amount);
618: event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);
619: event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
620: event CreateEscrow(address indexed user, address escrow);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```
147: event RecordDailyLow(address indexed token, uint price);
```


### N-03 REQUIRE() or REVERT() STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```
93: require(globalSupply <= supplyCeiling);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol

```
67: require(msg.sender == beneficiary);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol

```
91: require(msg.sender == beneficiary);
```


### N-04 OPEN TODO

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol

```
35:  xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```



### N-05 NATSPEC IS INCOMPLETE

This is applicable throughout the contracts. 1 such instance is:

```
16: contract SimpleERC20Escrow { // missing @params
```



### N-06 TYPOS

"th" is a typo instead of "the"

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```
128: @notice Get the DBR deficit of an address. Will return 0 if th user has zero DBR or more.
```
