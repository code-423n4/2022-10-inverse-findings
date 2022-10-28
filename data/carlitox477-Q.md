# \[LOW\] Oracle#constructor: Allow setting operator address to zero
This would let unusable next functions:
* [setPendingOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44)
* [setFeed](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53)
* [setFixedPrice](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61)

# \[LOW\] Oracle#setPendingOperator: Allow to set pending operator to current address operator
This can be avoided by a check

Setting the pending operator to current operator would allow to emit a nonsense event ```ChangeOperator``` when ```claimOperator``` is called


# \[LOW\] BorrowController should set operator in two steps
Good practise, avoid setting it to zero address

# \[LOW\] GovTokenEscrow: beneficiary can be set to zero address
Giving the lack of zero address check, ```beneficiary``` can be set to address zero with no way of change it value, leaving [delegate](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L66) function useless.

# \[Non-critical\] GovTokenEscrow: delegate, balance, pay and initialize visibility should be external
Giving they are not used by any other function in the contract the visibility of next function should be external:
* [initialize](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L30)
* [pay](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L43)
* [balance](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L66)


# \[LOW\] INVEscrow#constructor: _xINV can be set to zero address
Giving the lack of zero address check, ```_xINV``` can be set to address zero with no way of change it value, leaving [initialize](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L51); [pay](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L62); [balance](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L72); [onDeposit](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L82) and [delegate](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L93) functions useless, leading to contract redeployment.

Add next line to the [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L34-L36):

```solidity
if(_INVX == _INVX(address(0))) revert ERROR_ADDRESS_ZERO_SETTING()
```

# \[Non-critical\] delegate, balance, pay and initialize visibility should be external
Giving they are not used by any other function in the contract the visibility of next function should be external:
* [initialize](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L44)
* [pay](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L59)
* [balance](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L70)
* [onDeposit](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L79)
* [delegate](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L90)

# \[LOW\] INVEscrow: beneficiary can be set to zero address
Giving the lack of zero address check, ```beneficiary``` can be set to address zero with no way of change it value, leaving [delegate](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L91) function useless.

# \[LOW\] SimpleERC20Escrow: _token can be set to zero address
Giving the [lack of zero address check](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L25-L29), ```_token``` can be set to address zero with no way of change it value, leaving [pay](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L38) and [balance](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L46) functions useless, leading to redeployment.

# \[LOW\] BorrowController#constructor: Lack of check to set operator
Contract creator can create a contract with operator set to zero address.

If address zero is set, next functions would be affected:
* [allow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32)
* [deny](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)

Recommended code:

```solidity
constructor(address _operator) {
    if(_operator == address(0)) revert ERROR_INVALID_OPERATOR();
    operator = _operator;
}
```

# \[LOW\] BorrowController#setOperator Lack of zero address check to set operator
Contract operator can set new operator to to zero address.

If address zero is set, next functions would be affected:
* [allow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32)
* [deny](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)

Recommended code:

```solidity
function setOperator(address _operator) public onlyOperator {
    if(_operator == address(0)) revert ERROR_INVALID_OPERATOR();
    operator = _operator;
    }

```
Functions affected:
* [allow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32)
* [deny](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)

# \[LOW\] BorrowController should provide methods to set operator in two steps
Is a good practise to set important roles values in two steps. So a state variable ```pendingOperator``` should be added and [this function](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26) should be replace for:

```solidity
modifier onlyPendingOperator {
        if(msg.sender != pendingOperator) revert ERROR_NOT_PENDING_OPERATOR()
        _;
    }

function setPendingOperator(address _pendingOperator) external onlyOperator {
        pendingOperator = _pendingOperator;
    }

function claimOperator(address _pendingOperator) external onlyPendingOperator {
    pendingOperator = address(0);
    operator = msg.sender;
}
```

# \[Non-critical\] BorrowController: setOperator, allow, deny, borrowAllowed visibility should be external
Giving they are not used by any other function in the contract the visibility of next function should be external:
* [setOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26)
* [allow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32)
* [deny](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)
* [borrowAllowed](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L46)

# \[LOW\] DolaBorrowingRights#constructor allows setting operator to address zero
This would leave next function useless:
* [setPendingOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53)
* [setReplenishmentPriceBps](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L62)
* [addMinter](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L81)
* [removeMinter](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L90)
* [addMarket](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L99)

As a consequence, the contract will need to be redeployed.

Mitigation steps: add next line to the [constructor](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L30-L42)
```solidity
if (_operator == address(0)) revert ERROR_ADDRESS_ZERO_ASIGNEMENT();
```

# \[Non-Critical\] DolaBorrowingRights has multiple functions that can emit meaningless event:
This functions are:
* [claimOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L70-L75): If ```pendingOperator == operator```. This condition should be checked to avoid a meaningless ```ChangeOperator``` event emission 
* [addMinter](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L81-L84): If ``` minters[minter_] == true```. This condition should be checked to avoid a meaningless ```AddMinter``` event emission 
* [removeMinter](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L90-L93): If ``` minters[minter_] == false```. This condition should be checked to avoid a meaningless ```RemoveMinter``` event emission 
* [addMarket](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L99-L102): If ```markets[market_] == true```. This condition should be checked to avoid a meaningless ```AddMarket``` event emission 


# \[Non-critical\]DolaBorrowingRights has multiple function that are public and should be external
* [setPendingOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53)
* [setReplenishmentPriceBps](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L62)
* [claimOperator](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L70)
* [addMinter](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L81)
* [removeMinter](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L90)
* [addMarket](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L99)
* [totalSupply](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L109)
* [signedBalanceOf](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L146)
* [approve](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L158)
* [transfer](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170)
* [transferFrom](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188)
* [permit](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L215)
* [invalidateNonce](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L258)
* [onBorrow](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L300)
* [onRepay](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L313)
* [onForceReplenish](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L325)
* [burn](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L340)
* [mint](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L349)


# \[LOW\] DolaBorrowingRights#setReplenishmentPriceBps should check it is lower/equal to 10.000
```replenishmentPriceBps``` seems to be used just to calculate a porcentage for ```amount``` in function ```onForceReplenish```:
```solidity
uint replenishmentCost = amount * replenishmentPriceBps / 10000;
```
However if ```replenishmentPriceBps > 10000``` the replenishmentCost will be a multiple of amount. 

For this reason, ```setReplenishmentPriceBps``` should check that ```newReplenishmentPriceBps_``` is lower/equal than **10.000**

# \[LOW\] Fed#constructor: should check zero address values.
Oherwise next function may become useless
* [changeGov](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L49)
* [changeChair](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L67)
* [expansion](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L88-L90)
* [contraction](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L104-L105)
* [getProfit](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L121)
* [takeProfit](https://github.com/InverseFinance/FiRM-code4rena/blob/2f39bf457e83690477269bfa586717bbed8a8258/src/Fed.sol#L134-L135)

# \[LOW\] #Fed: governance change should be done in two steps
This in order to follow best practises and avoid setting ```gov``` to zero address. So, replace ```changeGov``` for:
```solidity
address pendingGov;

function setPendingGov(address _pendigGov) public {
    require(msg.sender == gov, "ONLY GOV");
    pendingGov = _pendigGov;
}

function claimGov() public {
    require(msg.sender == pendingGov, "ONLY GOV");
    gov = msg.sender;
    pendingGov = addres(0);
}

```

# \[LOW\] Market#setGov shoulf check non zero address assignment
Otherwise every function with ```onlyGov``` modifer will become inaccesible.

# \[LOW\] Market: governance change should be done in two steps
This in order to follow best practises and avoid setting ```gov``` to zero address. So, replace ```setGov``` for:
```solidity
address pendingGov;

function setPendingGov(address _pendigGov) public {
    require(msg.sender == gov, "ONLY GOV");
    pendingGov = _pendigGov;
}

function claimGov() public {
    require(msg.sender == pendingGov, "ONLY GOV");
    gov = msg.sender;
    pendingGov = addres(0);
}

```