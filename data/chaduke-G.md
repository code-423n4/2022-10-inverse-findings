
0) Current design will create an escrow contract for each user-market pair, so if there are 1M users (if Inverse is very successful), then there might be up to 1M * K (K be the number of markets), not to mention future upgrade of the escrow contract. So, the deployment gas fee is high. 

Consider the deployment of  only ONE escrow contract but can be used by multilple user-market pairs will save a lot of gas. 

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

7 https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L284
cache block.timestamp as it will be accessed multiple times in this function

8. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L328-L329
Eliminte Line 328 as it is implied by line 329

9. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L70
Cache pendingOperator as it will accessed twice

10. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L233-L235
Define it as a precomputed constant

11. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L284-L292
cache block.timestamp

12. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L112
cache feeds[token] as it will be accessed multiple times, caches dailylows[token] 

13. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L390
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L422
move the check of *require(!borrowPaused, "Borrowing is paused");" to earlier entry functions such as L290 and L422 to save gas




