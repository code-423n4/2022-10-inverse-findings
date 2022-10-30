## Loss Of Precision

This issue is about arithmetic computation that could have been done more percise. 
The following are places in the codebase in which you multiplied after the divisions. 
Doing the multiplications at start lead to more accurate calculations. 
This is a list of places in the code that this appears (Solidity file, line number, actual line): 

### Code instances:

        Market.sol, 605, uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
        Market.sol, 376, uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
        Market.sol, 359, uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;



## Mult instead div in compares


To improve algorithm precision instead using division in comparison use multiplication in the following scenario:
        
        Instead a < b / c use a * c < b. 
    
In all of the big and trusted contracts this rule is maintained.
    
### Code instance:

        Market.sol, 595, require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");



## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:
        
        
### Code instances:

        Market.setLiquidationFeeBps (_liquidationFeeBps)
        Oracle.setFeed (feed)



## safeApprove of openZeppelin is deprecated


You use safeApprove of openZeppelin although it's deprecated.
(see https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38)
You should change it to increase/decrease Allowance as OpenZeppilin says.
        
### Code instance:

        Deprecated safeApprove in INVEscrow.sol line 49: _token.approve(address(xINV), type(uint).max);



## Require with empty message

The following requires are with empty messages. 
This is very important to add a message for any require. So the user has enough information to know the reason of failure.
### Code instances:

        Solidity file: Fed.sol, In line 93 with Empty Require message.
        Solidity file: GovTokenEscrow.sol, In line 67 with Empty Require message.
        Solidity file: INVEscrow.sol, In line 91 with Empty Require message.



## Require with not comprehensive message

The following requires has a non comprehensive messages. 
This is very important to add a comprehensive message for any require. Such that the user has enough 
information to know the reason of failure: 

### Code instances:

        Solidity file: DBR.sol, In line 328 with Require message: No deficit
        Solidity file: Fed.sol, In line 104 with Require message: ONLY CHAIR
        Solidity file: Fed.sol, In line 49 with Require message: ONLY GOV
        Solidity file: Fed.sol, In line 58 with Require message: ONLY GOV
        Solidity file: Fed.sol, In line 76 with Require message: ONLY CHAIR
        Solidity file: Fed.sol, In line 67 with Require message: ONLY GOV
        Solidity file: Fed.sol, In line 87 with Require message: ONLY CHAIR



## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.    

### Code instances:

        Market.sol.forceReplenish user
        GovTokenEscrow.sol.delegate delegatee
        Market.sol.liquidate user
        Oracle.sol.getPrice token
        DBR.sol.approve spender
        DBR.sol.setPendingOperator newOperator_
        Oracle.sol.setPendingOperator newOperator_
        Market.sol.setLender _lender
        BorrowController.sol.allow allowedContract
        Oracle.sol.setFixedPrice token
        Market.sol.setGov _gov
        DBR.sol.transferFrom to
        DBR.sol.accrueDueTokens user
        GovTokenEscrow.sol.initialize _beneficiary
        DBR.sol.onRepay user
        Fed.sol.changeGov _gov
        DBR.sol.transferFrom from
        Market.sol.withdrawOnBehalf from
        DBR.sol.removeMinter minter_
        DBR.sol.onForceReplenish user
        DBR.sol.addMinter minter_
        BorrowController.sol.deny deniedContract
        INVEscrow.sol.initialize _beneficiary
        Fed.sol.changeChair _chair
        INVEscrow.sol.delegate delegatee
        Market.sol.repay user
        GovTokenEscrow.sol.pay recipient
        DBR.sol.permit spender
        Market.sol.setPauseGuardian _pauseGuardian
        Oracle.sol.setFeed token
        Market.sol.deposit user
        DBR.sol.onBorrow user
        BorrowController.sol.setOperator _operator
        INVEscrow.sol.pay recipient
        DBR.sol.addMarket market_
        DBR.sol.mint to
        SimpleERC20Escrow.sol.pay recipient
        Market.sol.borrowOnBehalf from
        DBR.sol.transfer to



## Solidity compiler versions mismatch


The project is compiled with different versions of solidity, which is not recommended because it can lead to undefined behaviors.
        
### Code instance:

        



## Use safe math for solidity version <8

You should use safe math for solidity version <8 since there is no default over/under flow check it suchversions of solidity.
### Code instances:

        The contract Test.sol doesn't use safe math and is of solidity version < 8
        The contract console.sol doesn't use safe math and is of solidity version < 8
        The contract console2.sol doesn't use safe math and is of solidity version < 8
        The contract Script.sol doesn't use safe math and is of solidity version < 8
        The contract Vm.sol doesn't use safe math and is of solidity version < 8



## Not verified owner


        owner param should be validated to make sure the owner address is not address(0).
        Otherwise if not given the right input all only owner accessible functions will be unaccessible.
        
        
### Code instance:

        DBR.sol.permit owner



## Two Steps Verification before Transferring Ownership

The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.
It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership. 
A similar issue was reported in a previous contest and was assigned a severity of medium: [code-423n4/2021-06-realitycards-findings#105](https://github.com/code-423n4/2021-06-realitycards-findings/issues/105) 

### Code instance:

        Market.sol



## Never used parameters

Those are functions and parameters pairs that the function doesn't use the parameter. In case those functions are external/public this is even worst since the user is required to put value that never used and can misslead him and waste its time. 

### Code instances:

        Market.sol: function setGov parameter _gov isn't used. (setGov is public)
        Oracle.sol: function setFeed parameter tokenDecimals isn't used. (setFeed is public)
        Market.sol: function setOracle parameter _oracle isn't used. (setOracle is public)
        Oracle.sol: function setPendingOperator parameter newOperator_ isn't used. (setPendingOperator is public)
        Market.sol: function setBorrowController parameter _borrowController isn't used. (setBorrowController is public)
        BorrowController.sol: function allow parameter allowedContract isn't used. (allow is public)
        Market.sol: function setPauseGuardian parameter _pauseGuardian isn't used. (setPauseGuardian is public)
        Oracle.sol: function setFeed parameter token isn't used. (setFeed is public)
        Market.sol: function setLender parameter _lender isn't used. (setLender is public)
        BorrowController.sol: function deny parameter deniedContract isn't used. (deny is public)
        Oracle.sol: function setFixedPrice parameter price isn't used. (setFixedPrice is public)
        BorrowController.sol: function setOperator parameter _operator isn't used. (setOperator is public)
        Oracle.sol: function setFixedPrice parameter token isn't used. (setFixedPrice is public)
        Oracle.sol: function setFeed parameter feed isn't used. (setFeed is public)



## Dangerous usage of tx.origin


Use of tx.origin for authorization may be abused by a MITM malicious contract forwarding calls from the legitimate user who interacts with it. Use msg.sender instead.
        
### Code instance:

        BorrowController.sol, 47: if(msgSender == tx.origin) return true;



## Open TODOs

Open TODOs can hint at programming or architectural errors that still need to be fixed. 
These files has open TODOs:

### Code instance:

Open TODO in INVEscrow.sol line 34 :         xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies




## Check transfer receiver is not 0 to avoid burned money


Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.


### Code instances:

        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L399
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L176
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L291
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L537
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L378
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L200
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L364
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L45
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L63
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L38
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L205
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L570
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L602
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L135
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L280



## Add a timelock

To give more trust to users: functions that set key/critical variables should be put behind a timelock.

### Code instances:

        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L53
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L183
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L124
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L136


## Must approve 0 first


Some tokens (like USDT) do not work when changing the allowance from an existing non-zero allowance value.
They must first be approved by zero and then the actual allowance must be approved.

### Code instance:

approve without approving 0 first INVEscrow.sol, 49,         _token.approve(address(xINV), type(uint).max);




## Unsafe Cast

use openzeppilin's safeCast in:
 
### Code instances:

        https://github.com/code-423n4/2022-10-inverse/tree/main/lib/forge-std/src/Script.sol#L17 : unsafe cast uint8(nonce)
        https://github.com/code-423n4/2022-10-inverse/tree/main/lib/forge-std/src/Script.sol#L17 : unsafe cast uint16(nonce)
        https://github.com/code-423n4/2022-10-inverse/tree/main/lib/forge-std/src/Script.sol#L17 : unsafe cast uint32(nonce)
        https://github.com/code-423n4/2022-10-inverse/tree/main/lib/forge-std/src/Script.sol#L17 : unsafe cast uint24(nonce)
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L146 : unsafe cast int(accrued)



## Div by 0


Division by 0 can lead to accidentally revert,
(An example of a similar issue - https://github.com/code-423n4/2021-10-defiprotocol-findings/issues/84)

### Code instances:

        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L137 collateralFactorBps might be 0
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L597 price might be 0
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L606 price might be 0
        https://github.com/code-423n4/2022-10-inverse/tree/main/lib/forge-std/src/Test.sol#L765 a, b, absB might be 0
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L98 collateralFactorBps might be 0


## approve return value is ignored

Some tokens don't correctly implement the EIP20 standard and their approve function returns void instead of a success boolean. 
Calling these functions with the correct EIP20 function signatures will always revert.
Tokens that don't correctly implement the latest EIP20 spec, like USDT, will be unusable in the mentioned contracts as they revert the transaction because of the missing return value.
We recommend using OpenZeppelin’s SafeERC20 versions with the safeApprove function that handle the return value check as well as non-standard-compliant tokens.
The list of occurrences in format (solidity file, line number, actual line)
### Code instance:

INVEscrow.sol, 49,         _token.approve(address(xINV), type(uint).max);




## transfer return value of a general ERC20 is ignored

Need to use safeTransfer instead of transfer. As there are popular tokens, such as USDT that transfer/trasnferFrom method doesn’t return anything. The transfer return value has to be checked (as there are some other tokens that returns false instead revert), that means you must 
 1. Check the transfer return value
Another popular possibility is to add a whiteList.
Those are the appearances (solidity file, line number, actual line):

### Code instances:

        Fed.sol, 135 (takeProfit), dola.transfer(gov, profit);
        GovTokenEscrow.sol, 45 (pay), token.transfer(recipient, amount);
        SimpleERC20Escrow.sol, 38 (pay), token.transfer(recipient, amount);
        Market.sol, 280 (deposit), collateral.transferFrom(msg.sender, address(escrow), amount);
        INVEscrow.sol, 63 (pay), token.transfer(recipient, amount);
