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