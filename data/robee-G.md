## More efficient Struct packing of StdStorage in the contract Test.sol


        The following structs could change the order of their stored elements to decrease memory uses.
        and number of occupied slots. Therefore will save gas at every store and load from memory.
        
In Test.sol, StdStorage is optimized to: 6 slots from: 7 slots.
The new order of types (you choose the actual variables):
        1. mapping
        2. mapping
        3. bytes32[]
        4. uint256
        5. bytes32
        6. address
        7. bytes4






## Unnecessary equals boolean


Boolean variables can be checked within conditionals directly without the use of equality operators to true/false.

### Code instances:

        BorrowController.sol, 47: if(msgSender == tx.origin) return true;
        DBR.sol, 350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");



## Unused state variables

Unused state variables are gas consuming at deployment (since they are located in storage) and are 
a bad code practice. Removing those variables will decrease deployment gas cost and improve code quality. 
This is a full list of all the unused storage variables we found in your code base. 

### Code instances:

        Script.sol, IS_SCRIPT
        DBR.sol, decimals



## Storage double reading. Could save SLOAD

Reading a storage variable is gas costly (SLOAD). In cases of multiple read of a storage variable in the same scope, caching the first read (i.e saving as a local variable) can save gas and decrease the
 overall gas uses. The following is a list of functions and the storage variables that you read twice: 

### Code instances:

        Market.sol: gov is read twice in pauseBorrows
        Market.sol: lender is read twice in recall



## Rearrange state variables

You can change the order of the storage variables to decrease memory uses.

### Code instances:

In DBR.sol,rearranging the storage fields can optimize to: 9 slots from: 10 slots.
The new order of types (you choose the actual variables):
        1. string
        2. string
        3. uint256
        4. uint
        5. uint
        6. uint256
        7. bytes32
        8. address
        9. uint8
        10. address

In Market.sol,rearranging the storage fields can optimize to: 17 slots from: 18 slots.
The new order of types (you choose the actual variables):
        1. IDolaBorrowingRights
        2. IBorrowController
        3. IERC20
        4. IERC20
        5. IOracle
        6. uint
        7. uint
        8. uint
        9. uint
        10. uint
        11. uint
        12. uint256
        13. bytes32
        14. address
        15. bool
        16. bool
        17. address
        18. address
        19. address




## Short the following require messages

The following require messages are of length more than 32 and we think are short enough to short
them into exactly 32 characters such that it will be placed in one slot of memory and the require 
function will cost less gas. 
The list: 

### Code instances:

        Solidity file: DBR.sol, In line 63, Require message length to shorten: 34, The message: replenishment price must be over 0
        Solidity file: DBR.sol, In line 326, Require message length to shorten: 38, The message: Only markets can call onForceReplenish



## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instances:

        Market.sol, 605: change 'liquidationFeeBps > 0' to 'liquidationFeeBps != 0'
        Market.sol, 184: change '_liquidationIncentiveBps > 0' to '_liquidationIncentiveBps != 0'
        Market.sol, 195: change '_liquidationFeeBps > 0' to '_liquidationFeeBps != 0'
        Market.sol, 162: change '_liquidationFactorBps > 0' to '_liquidationFactorBps != 0'
        DBR.sol, 373: change 'balance > 0' to 'balance != 0'
        Market.sol, 162: change 'liquidationFactorBps > 0' to 'liquidationFactorBps != 0'
        Market.sol, 184: change 'liquidationIncentiveBps > 0' to 'liquidationIncentiveBps != 0'
        DBR.sol, 63: change 'newReplenishmentPriceBps_ > 0' to 'newReplenishmentPriceBps_ != 0'
        Market.sol, 173: change '_replenishmentIncentiveBps > 0' to '_replenishmentIncentiveBps != 0'
        Market.sol, 195: change 'liquidationFeeBps > 0' to 'liquidationFeeBps != 0'
        Market.sol, 75: change '_liquidationIncentiveBps > 0' to '_liquidationIncentiveBps != 0'
        DBR.sol, 123: change 'balance > 0' to 'balance != 0'
        Market.sol, 75: change 'liquidationIncentiveBps > 0' to 'liquidationIncentiveBps != 0'
        DBR.sol, 195: change 'balance > 0' to 'balance != 0'
        Market.sol, 592: change 'repaidDebt > 0' to 'repaidDebt != 0'
        DBR.sol, 171: change 'balance > 0' to 'balance != 0'
        Market.sol, 607: change 'balance > 0' to 'balance != 0'
        Market.sol, 173: change 'replenishmentIncentiveBps > 0' to 'replenishmentIncentiveBps != 0'
        Test.sol, 735: change 'a > 0' to 'a != 0'



## Use unchecked to save gas for certain additive calculations that cannot overflow


You can use unchecked in the following calculations since there is no risk to overflow:

### Code instance:

        Test.sol (L#33) - vm.warp(block.timestamp + time);



## Consider inline the following functions to save gas


    You can inline the following functions instead of writing a specific function to save gas.
    (see https://github.com/code-423n4/2021-11-nested-findings/issues/167 for a similar issue.)

    
        Script.sol, addressFromLast20Bytes, { return address(uint160(uint256(bytesValue))); }



## Inline one time use functions


The following functions are used exactly once. Therefore you can inline them and save gas and improve code clearness.
    

### Code instances:

        Market.sol, getWithdrawalLimitInternal
        Market.sol, createEscrow



## Upgrade pragma to at least 0.8.4


Using newer compiler versions and the optimizer gives gas optimizations
and additional safety checks are available for free.

The advantages of versions 0.8.* over <0.8.0 are:

        1. Safemath by default from 0.8.0 (can be more gas efficient than library based safemath.)
        2. Low level inliner : from 0.8.2, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra jump instructions and additional stack operations needed for function calls.
        3. Optimizer improvements in packed structs: Before 0.8.3, storing packed structs, in some cases used an additional storage read operation. After EIP-2929, if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means additional cost of 100 gas alongside the same unnecessary stack operations and extra deploy time costs.
        4. Custom errors from 0.8.4, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.
    
### Code instances:

        Test.sol
        Script.sol
        console2.sol
        console.sol
        Vm.sol



## Do not cache msg.sender


We recommend not to cache msg.sender since calling it is 2 gas while reading a variable is more.


### Code instances:

        https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L32
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L46
        https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L27


