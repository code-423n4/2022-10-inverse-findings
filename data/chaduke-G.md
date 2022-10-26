1) 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L162
change 
 require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
to
    if(_liquidationFactorBps > 10000) revert InvalidLiquidationFactor();
    if(_liquidationFactorBps == 0) revert InvalidLiquidationFactor();
Spliting a complex condition statement into multiple revert-with-custom-error statements can save gas. 

Similar suggetion to go line 195. 


2) 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L324
IEscrow escrow = predictEscrow(user);

This line can be cached into a state mapping so that there is no need to calculate the escrow address for each user all the time. 
mapping(address => IEscrow) EscrowAddress;

3) 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L11
change from zero to non-zero takes more gas (https://ethereum.stackexchange.com/questions/99136/why-does-zero-to-non-zero-in-storage-take-higher-gas). Therefore, to save gas, one can change
   mapping(address => bool) public contractAllowlist;
to 
   mapping(address => uint) public contractAllowlist;
 
and use 2 to code true and 1 and 0 to code false.
Therefore the new implementation would be
```
 function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = 2; }
function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = 1; }

function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
        if(msgSender == tx.origin) return true;
        return contractAllowlist[msgSender] == 2;
    }

```
 4. Rearrange the storage variable locations for all files so that all variables
   can be packed into 32-bytes slots. For example, Line 13 
   in DBR.sol needs to move to the end.

5. Reimplement
mapping (address => bool) public minters;
mapping (address => bool) public markets;
as

mapping (address => uint) public minters;
mapping (address => uint) public markets;

and encode 1 as false and 2 as true since changing 0 to 1 is more expensive then change
from non-zero to non-zero.

6. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L196
Move line 196 inside unchecked{} since underflow is impossible due to previous check


