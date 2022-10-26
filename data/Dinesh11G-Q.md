### Unsafe ERC20 Operation(s)

#### Impact
Issue Information: ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

To circumvent ERC20's approve functions race-condition vulnerability use OpenZeppelin's SafeERC20 library's safe{Increase|Decrease}Allowance functions.

In case the vulnerability is of no danger for your implementation, provide enough documentation explaining the reasonings.

Example
🤦 Bad:

IERC20(token).transferFrom(msg.sender, address(this), amount);
🚀 Good (using OpenZeppelin's SafeERC20):

import {SafeERC20} from "openzeppelin/token/utils/SafeERC20.sol";

// ...

IERC20(token).safeTransferFrom(msg.sender, address(this), amount);

#### Findings:
```
2022-10-inverse/src/Fed.sol::135 => dola.transfer(gov, profit);
2022-10-inverse/src/Market.sol::205 => dola.transfer(msg.sender, amount);
2022-10-inverse/src/Market.sol::280 => collateral.transferFrom(msg.sender, address(escrow), amount);
2022-10-inverse/src/Market.sol::399 => dola.transfer(to, amount);
2022-10-inverse/src/Market.sol::537 => dola.transferFrom(msg.sender, address(this), amount);
2022-10-inverse/src/Market.sol::570 => dola.transfer(msg.sender, replenisherReward);
2022-10-inverse/src/Market.sol::602 => dola.transferFrom(msg.sender, address(this), repaidDebt);
2022-10-inverse/src/escrows/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
2022-10-inverse/src/escrows/INVEscrow.sol::50 => _token.approve(address(xINV), type(uint).max);
2022-10-inverse/src/escrows/INVEscrow.sol::63 => token.transfer(recipient, amount);
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::38 => token.transfer(recipient, amount);
2022-10-inverse/src/test/mocks/BorrowContract.sol::12 => WETH9(weth_).approve(address(market), type(uint).max);
```
#### Tools used
Manual











### Unspecific Compiler Version Pragma

#### Impact
Issue Information: Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

Example
🤦 Bad:

pragma solidity ^0.8.0;
🚀 Good:

pragma solidity 0.8.4;

#### Findings:
```
2022-10-inverse/src/BorrowController.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/DBR.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Fed.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Market.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Oracle.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/escrows/GovTokenEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/escrows/INVEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/test/BorrowController.t.sol::2 => pragma solidity ^0.8.13;
```
#### Tools used
Manual


### Title: 
Authorization through tx.origin

### Description:
tx.origin is a global variable in Solidity which returns the address of the account that sent the transaction. Using the variable for authorization could make a contract vulnerable if an authorized account calls into a malicious contract. A call could be made to the vulnerable contract that passes the authorization check since tx.origin returns the original sender of the transaction which in this case is the authorized account.

Contract: BorrowController
Function name: borrowAllowed(address,address,uint256)
PC address: 876
Estimated Gas Usage: 924 - 1019

### Impact:
Use of tx.origin as a part of authorization control.
The tx.origin environment variable has been found to influence a control flow decision. Note that using tx.origin as a security control might cause a situation where a user inadvertently authorizes a smart contract to perform an action on their behalf. It is recommended to use msg.sender instead.

In file: BorrowController.sol:47

if(msgSender == tx.origin) return true

Initial State:

Account: [CREATOR], balance: 0x1, nonce:0, storage:{}
Account: [ATTACKER], balance: 0x0, nonce:0, storage:{}

Transaction Sequence:

Caller: [CREATOR], calldata: 000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 value: 0x0
Caller: [ATTACKER], function: borrowAllowed(address,address,uint256), txdata: 0xda3d454c000000000000000000000000000000000000000000408000020080400100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000, value: 0x0

### Tools Used:
Manual