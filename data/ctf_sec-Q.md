## User can transfer DBR to zero address.

Please add the check to make sure the receiver is not address(0)

```solidity
require(to != address(0), "cannot transfer token to address(0)");
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188

## User can perform self-transfer using transfer and transferFrom in DBR.

Please add the check to disable self transfer

```solidity
require(from != to, "self transfer detected");
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188

## User can transfer 0 amount for DBR

Please add the check that transfer 0 amount is not allowed.

```solidity
require(amount > 0, "amount should larger than 0");
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188

## Two step ownership transfer is missing.

Two step transfer is implemented in DBR.sol and in Oracle.sol

but is missing in the rest of the contract including Fed.sol, Market.sol and BorrowList.sol that involved in governance role.

```solidity
function setPendingOperator(address newOperator_) public onlyOperator {
	pendingOperator = newOperator_;
}
```

```solidity
function claimOperator() public {
	require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
	operator = pendingOperator;
	pendingOperator = address(0);
	emit ChangeOperator(operator);
}
```

Affected line of code:

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26


```solidity
    function setOperator(address _operator) public onlyOperator { operator = _operator; }
```
            
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L132

```solidity
    function setGov(address _gov) public onlyGov { gov = _gov; }
```
            

Please implment two step transfer to make sure the pending operator has intention and is capable to claim the operator and manage the contract.

## Missing event emission in crucial state change.

Please emit when change crucical setting in Fed.sol, Market.sol, BorrowList.sol and Oracle.sol and in DBR.sol

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26


```solidity
    function setOperator(address _operator) public onlyOperator { operator = _operator; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32


```solidity
    function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38


```solidity
    function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L120


```solidity
    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L126


```solidity
    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L132


```solidity
    function setGov(address _gov) public onlyGov { gov = _gov; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L138


```solidity
    function setLender(address _lender) public onlyGov { lender = _lender; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L144


```solidity
    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L151


```solidity
    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L163


```solidity
    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L174


```solidity
    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L185


```solidity
    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L196


```solidity
    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44


```solidity
    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
```
            

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53


```solidity
    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61


```solidity
    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```