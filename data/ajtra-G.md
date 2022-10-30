# Index
1. Require instead of &&
2. I = I + (-) X is cheaper in gas cost than I += (-=) X
3. Require / Revert strings longer than 32 bytes cost extra gas
4. Don't compare boolean expressions to boolean literals
5. ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
6. Remove local variables that are used once
7. Cache storage variables in a local variable when is used more than once
8. Optimize Market.forceReplenish function
9. Internal functions only called once can be inlined to save gas
10. Function that revert when are called by normal users can be marked payable
11. State variables that never change should be declared immutable or constant
12. Operatos >= cost less gas than operator >

# Details
## 1. Require instead of &&
### Description
Split of conditions of an require sentence in different requires sentences can save gas

### Lines in the code
[DBR.sol#L249](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L249)
[Market.sol#L75](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L75)
[Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162)
[Market.sol#L173](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L173)
[Market.sol#L184](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L184)
[Market.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L195)
[Market.sol#L448](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L448)
[Market.sol#L512](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L512)

## 2. I = I + (-) X is cheaper in gas cost than I += (-=) X
### Description
In the following example (optimizer = 10000) it's possible to demostrate that I = I + X cost less gas than I += X in state variable.

```solidity
contract Test_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a = a + 1;
        return a;
    }
}

contract Test_Without_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a += 1;
        return a;
    }
}
```
* Test_Optimization cost is 26324 gas
* Test_Without_Optimization cost is 26440 gas

With this optimization it's possible to **save 116 gas**


### Lines in the code
[DBR.sol#L172](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L172)
[DBR.sol#L174](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L174)
[DBR.sol#L196](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L196)
[DBR.sol#L198](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L198)
[DBR.sol#L288](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L288)
[DBR.sol#L289](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L289)
[DBR.sol#L304](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L304)
[DBR.sol#L316](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L316)
[DBR.sol#L332](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L332)
[DBR.sol#L360](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360)
[DBR.sol#L362](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L362)
[DBR.sol#L374](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L374)
[DBR.sol#L376](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L376)
[Fed.sol#L91](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L91)
[Fed.sol#L92](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L92)
[Fed.sol#L110](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L110)
[Fed.sol#L111](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L111)
[Market.sol#L395](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L395)
[Market.sol#L397](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L397)
[Market.sol#L534](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L534)
[Market.sol#L535](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L535)
[Market.sol#L565](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L565)
[Market.sol#L568](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L568)
[Market.sol#L598](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L598)
[Market.sol#L599](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L599)
[Market.sol#L600](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L600)
	
## 3. Require / Revert strings longer than 32 bytes cost extra gas
### Description
Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

### Lines in the code
[DBR.sol#L63](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L63)
[DBR.sol#L326](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L326)
[Market.sol#L76](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L76)
[Market.sol#L214](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L214)

## 4. Don't compare boolean expressions to boolean literals
### Description
if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)

### Lines in the code
```diff
- require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
+ require(minters[msg.sender] || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```
[DBR.sol#L350](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L350)
```diff
- require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
+ require(!market.borrowPaused(), "CANNOT EXPAND PAUSED MARKETS");
```
[Fed.sol#L89](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L89)

## 5. ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
### Description
**abi.encode** will apply [ABI encoding rules](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html). Therefore all elementary types are padded to 32 bytes and dynamic arrays include their length. 
Therefore it is possible to also decode this data again (with abi.decode) when the type are known.

**abi.encodePacked** will only use the only use the minimal required memory to encode the data. E.g. an address will only use 20 bytes and for dynamic arrays only the elements will be stored without length. 
For more info see the [Solidity docs for packed mode](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html?highlight=encodepacked#non-standard-packed-mode)

For the input of the keccak method it is important that you can ensure that the resulting bytes of the encoding are unique. So if you always encode the same types and arrays 
always have the same length then there is no problem. But if you switch the parameters that you encode or encode multiple dynamic arrays you might have conflicts.

For example:

abi.encodePacked(address(0x0000000000000000000000000000000000000001), uint(0)) encodes to the same as abi.encodePacked(uint(0x0000000000000000000000000000000000000001000000000000000000000000), address(0))

and

abi.encodePacked(uint[](1,2), uint[](3)) encodes to the same as abi.encodePacked(uint[](1), uint[](2,3))

Therefore these examples will also generate the same hashes even so they are different inputs.

On the other hand you require less memory and therefore in most cases abi.encodePacked uses less gas than abi.encode.



### Lines in the code
[DBR.sol#L232](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L232)
[DBR.sol#L269](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L269)
[Market.sol#L104](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L104)
[Market.sol#L431](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L431)
[Market.sol#L495](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L495)

## 6. Remove local variables that are used once
### Description
When a local variable is used once it's possible to remove it and use directly the storage variable.

### Remove balanceOf.debt
```diff
-       uint debt = debts[user];
-       uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+       uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
```	
[DBR.sol#L121-L122](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L121-L122)

### Remove deficitOf.debt
```diff
-       uint debt = debts[user];
-       uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+       uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
```	
[DBR.sol#L134-L135](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L134-L135)

### Remove signedBalanceOf.debt	
```diff
-       uint debt = debts[user];
-       uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+       uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
```	
[DBR.sol#L159-L160](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L159-L160)

### Remove accrueDueTokens.debt
```diff
-       uint debt = debts[user];
        if(lastUpdated[user] == block.timestamp) return;
-       uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
+       uint accrued = (block.timestamp - lastUpdated[user]) * debts[user] / 365 days;
```	
[DBR.sol#L285-L287](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L285-L287)

### Remove onForceReplenish.replenishmentCost
```diff	
-       uint replenishmentCost = amount * replenishmentPriceBps / 10000;
        accrueDueTokens(user);
-       debts[user] += replenishmentCost;
+       debts[user] += (amount * replenishmentPriceBps / 10000);

```	
[DBR.sol#L330-L332](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L330-L332)
	
	
```diff
    function createEscrow(address user) internal returns (IEscrow instance) {
-       address implementation = escrowImplementation;
        /// @solidity memory-safe-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, 0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000000000000000000000)
-           mstore(add(ptr, 0x14), shl(0x60, implementation))
+           mstore(add(ptr, 0x14), shl(0x60, escrowImplementation))
            mstore(add(ptr, 0x28), 0x5af43d82803e903d91602b57fd5bf30000000000000000000000000000000000)
            instance := create2(0, ptr, 0x37, user)
        }
        require(instance != IEscrow(address(0)), "ERC1167: create2 failed");
        emit CreateEscrow(user, address(instance));
    }
```	
[Market.sol#L226-L238](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L226-L238)
	
```diff	
    function predictEscrow(address user) public view returns (IEscrow predicted) {
-       address implementation = escrowImplementation;
-       address deployer = address(this);
        /// @solidity memory-safe-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, 0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000000000000000000000)
-           mstore(add(ptr, 0x14), shl(0x60, implementation))
+           mstore(add(ptr, 0x14), shl(0x60, escrowImplementation))
            mstore(add(ptr, 0x28), 0x5af43d82803e903d91602b57fd5bf3ff00000000000000000000000000000000)
-           mstore(add(ptr, 0x38), shl(0x60, deployer))
+           mstore(add(ptr, 0x38), shl(0x60, address(this)))
            mstore(add(ptr, 0x4c), user)
            mstore(add(ptr, 0x6c), keccak256(ptr, 0x37))
            predicted := keccak256(add(ptr, 0x37), 0x55)
        }
    }
```	
[Market.sol#L292-L306](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L292-L306)
		
```diff
    function getCollateralValue(address user) public view returns (uint) {
-       IEscrow escrow = predictEscrow(user);
-       uint collateralBalance = escrow.balance();
-       return collateralBalance * oracle.viewPrice(address(collateral), collateralFactorBps) / 1 ether;
+       return predictEscrow(user).balance() * oracle.viewPrice(address(collateral), collateralFactorBps) / 1 ether;
    }
```		
[Market.sol#L312-L316](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L312-L316)

### Remove Market.getCollateralValueInternal
```diff
    function getCollateralValueInternal(address user) internal returns (uint) {
-       IEscrow escrow = predictEscrow(user);
-       uint collateralBalance = escrow.balance();
-       return collateralBalance * oracle.getPrice(address(collateral), collateralFactorBps) / 1 ether;
+       return predictEscrow(user).balance() * oracle.getPrice(address(collateral), collateralFactorBps) / 1 ether;
    }
```		
[Market.sol#L323-L327](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L323-L327)
		
```diff
    function getCreditLimit(address user) public view returns (uint) {
-       uint collateralValue = getCollateralValue(user);
-       return collateralValue * collateralFactorBps / 10000;
+       return getCollateralValue(user) * collateralFactorBps / 10000;
    }
```	
[Market.sol#L334-L337](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L334-L337)

```diff
    function getCreditLimitInternal(address user) internal returns (uint) {
-       uint collateralValue = getCollateralValueInternal(user);
-       return collateralValue * collateralFactorBps / 10000;
+       return getCollateralValueInternal(user) * collateralFactorBps / 10000;
    }
```
[Market.sol#L344-L347](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L344-L347)

```diff
    function withdrawInternal(address from, address to, uint amount) internal {
        uint limit = getWithdrawalLimitInternal(from);
        require(limit >= amount, "Insufficient withdrawal limit");
-       IEscrow escrow = getEscrow(from);
-       escrow.pay(to, amount);
+		getEscrow(from).pay(to, amount);
        emit Withdraw(from, to, amount);
    }
```
[Market.sol#L463-L464](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L463-L464)

```diff
    function repay(address user, uint amount) public {
-       uint debt = debts[user];
-       require(debt >= amount, "Insufficient debt");
+		require(debts[user] >= amount, "Insufficient debt");
        debts[user] -= amount;
        totalDebt -= amount;
        dbr.onRepay(user, amount);
        dola.transferFrom(msg.sender, address(this), amount);
        emit Repay(user, msg.sender, amount);
    }
```
[Market.sol#L532-L533](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L532-L533)

```diff
-	uint credit = getCreditLimit(user);
-   if(credit >= debt) return 0;
+   if(getCreditLimit(user) >= debt) return 0;
```
[Market.sol#L581-L582](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L581-L582)

```diff
-	uint supply = supplies[market];
-   require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
+   require(amount <= supplies[market], "AMOUNT TOO BIG"); // can't burn profits
```
[Fed.sol#L106-L107](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L106-L107)

## 7. Cache storage variables in a local variable when is used more than once
### Description
To avoid access to the storage mutiple times, it's a beter practice to store the variable in a local variable and then use it.

### Market.getWithdrawalLimitInternal cache collateralFactorBps varible due to it's used three times inside the contract.
[Market.sol#L359-L360](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L359-L360)

### Market.getWithdrawalLimit cache collateralFactorBps varible due to it's used three times inside the contract.
[Market.sol#L376-L377](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L376-L377)


## 8. Optimize Market.forceReplenish function
### Description
Move the calculation of replenisherReward after the require where it's going to be use to save gas when the require's condition is not true.

```diff
    function forceReplenish(address user, uint amount) public {
        uint deficit = dbr.deficitOf(user);
        require(deficit > 0, "No DBR deficit");
        require(deficit >= amount, "Amount > deficit");
        uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
-       uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;
        debts[user] += replenishmentCost;
        uint collateralValue = getCollateralValueInternal(user);
        require(collateralValue >= debts[user], "Exceeded collateral value");
        totalDebt += replenishmentCost;
        dbr.onForceReplenish(user, amount);
+       uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;
        dola.transfer(msg.sender, replenisherReward);
        emit ForceReplenish(user, msg.sender, amount, replenishmentCost, replenisherReward);
    }
```

### Lines in the code
[Market.sol#L559-L573](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L559-L573)

## 9. Internal functions only called once can be inlined to save gas
### Description
When inlined the code is saving between 20 and 40 gas for each instance due to JUMP instruction.

### Lines in the code
[DBR.sol#L372](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L372)
[Market.sol#L226](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L226)
[Market.sol#L353](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L353)


## 10. Function that revert when are called by normal users can be marked payable
### Description
When a function is using a modifier that guarantee to revert if a normal user tries to pay the function allow to mark the 
function as payable and save gas when the legitimate caller call the function due to the complier do not include checks 
for whether a payment is provided.


### Lines in the code
[Oracle.sol#L44](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44)
[Oracle.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53)
[Oracle.sol#L61](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61)
[DBR.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53)
[DBR.sol#L62](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L62)
[DBR.sol#L81](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L81)
[DBR.sol#L90](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L90)
[DBR.sol#L99](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L99)
[Market.sol#L118](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L118)
[Market.sol#L124](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L124)
[Market.sol#L130](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130)
[Market.sol#L136](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L136)
[Market.sol#L142](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L142)
[Market.sol#L149](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L149)
[Market.sol#L161](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L161)
[Market.sol#L172](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L172)
[Market.sol#L183](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L183)
[Market.sol#L194](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L194)
[BorrowController.sol#L26](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26)
[BorrowController.sol#L32](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32)
[BorrowController.sol#L38](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)

## 11. State variables that never change should be declared immutable or constant
### Description
Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) 
and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

### Lines in the code
[DBR.sol#L11](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L11)
[DBR.sol#L12](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L12)
[SimpleERC20Escrow.sol#L17](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L17)
[SimpleERC20Escrow.sol#L18](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L18)
[INVEscrow.sol#L29](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L29)
[INVEscrow.sol#L30](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L30)
[INVEscrow.sol#L31](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L31)
[GovTokenEscrow.sol#L20](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L20)
[GovTokenEscrow.sol#L21](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L21)
[GovTokenEscrow.sol#L22](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L22)
	
## 12. Operatos >= cost less gas than operator >
### Description
When use >= the evm use only LT and when use > the evm use GT and ISZERO what allow to save 3 gas for each instance.

### Lines in the code
[Oracle.sol#L96](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L96)
[Oracle.sol#L97](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L97)
[Oracle.sol#L135](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L135)
[Oracle.sol#L136](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L136)
[DBR.sol#L110](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L110)
[DBR.sol#L123](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L123)
