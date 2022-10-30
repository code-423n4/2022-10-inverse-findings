# Inverse Finance GAS optimizatoins
## use an optimized version of the minimal proxy bytecode
This is the bytecode used for minimal proxy address prediction and deployment in `createEscrow()` and `predictEscrow()`:
```solidity
function createEscrow(address user) internal returns (IEscrow instance) {
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, 0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000000000000000000000)
            mstore(add(ptr, 0x14), shl(0x60, implementation))
            mstore(add(ptr, 0x28), 0x5af43d82803e903d91602b57fd5bf30000000000000000000000000000000000)
            instance := create2(0, ptr, 0x37, user)
        }
        require(instance != IEscrow(address(0)), "ERC1167: create2 failed");
        emit CreateEscrow(user, address(instance));
    }
```
It is based on the OpenZeppeling code which has been recently updated to a somewhat more [optimized version](https://github.com/OpenZeppelin/openzeppelin-contracts/commit/6b9bda872d01f8d7e8532ca2002c9a7c8b5ba24d?diff=unified).