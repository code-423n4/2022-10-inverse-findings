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




==============================================================================================





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





==============================================================================================


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


==============================================================================================


### Lines are Too Long
Usually, lines in source code are limited to 80 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

### There are 4 instances of this issue:

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L16

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L25

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L12

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L99



==============================================================================================



### commented code should be resolved

### Lines of commented

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L56-L60
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L49-L53


### Uncomment because the Escrow contract handles deposit callbacks

### Tools used
Done Manually



==============================================================================================





### External Call To User-Supplied Address

Contract: GovTokenEscrow
Function name: initialize(address,address)
PC address: 916

### Impact
A call to a user-supplied address is executed.
An external message call to an address specified by the caller is executed. Note that the callee account might contain arbitrary code and could re-enter any function within this contract. Reentering the contract in an intermediate state may lead to unexpected behaviour. Make sure that no state modifications are executed after this call and/or reentrancy guards are in place.

In file: [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L35)

_token.delegate(_token.delegates(_beneficiary))

Initial State:

Account: [CREATOR], balance: 0x2, nonce:0, storage:{}
Account: [ATTACKER], balance: 0x0, nonce:0, storage:{}

Transaction Sequence:

Caller: [CREATOR], calldata: , value: 0x0
Caller: [SOMEGUY], function: initialize(address,address), txdata: 0x485cc955000000000000000000000000deadbeefdeadbeefdeadbeefdeadbeefdeadbeef0000000000000000000000000000000000000000000000000000000000000000, value: 0x0

### Tools used
Manual test

==============================================================================================



### Multiple Calls in a Single Transaction

Contract: GovTokenEscrow
Function name: initialize(address,address)
PC address: 916

### Impact
Multiple calls are executed in the same transaction.
This call is executed following another call within the same transaction. It is possible that the call never gets executed if a prior call fails permanently. This might be caused intentionally by a malicious callee. If possible, refactor the code such that each transaction only executes one external call or make sure that all callees can be trusted (i.e. they’re part of your own codebase).
--------------------
In file: [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L35)

_token.delegate(_token.delegates(_beneficiary))

--------------------
Initial State:

Account: [CREATOR], balance: 0x218, nonce:0, storage:{}
Account: [ATTACKER], balance: 0x0, nonce:0, storage:{}

Transaction Sequence:

Caller: [CREATOR], calldata: , value: 0x0
Caller: [CREATOR], function: initialize(address,address), txdata: 0x485cc95500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000, value: 0x0

### Tools used
Manual test


==============================================================================================



### External Call To User-Supplied Address

Contract: GovTokenEscrow
Function name: delegate(address)
PC address: 1151

### Impact
A call to a user-supplied address is executed.
An external message call to an address specified by the caller is executed. Note that the callee account might contain arbitrary code and could re-enter any function within this contract. Reentering the contract in an intermediate state may lead to unexpected behaviour. Make sure that no state modifications are executed after this call and/or reentrancy guards are in place.
--------------------
In file: [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L68)

token.delegate(delegatee)

--------------------
Initial State:

Account: [CREATOR], balance: 0xc000040007bc7d, nonce:0, storage:{}
Account: [ATTACKER], balance: 0x161074001, nonce:0, storage:{}

Transaction Sequence:

Caller: [CREATOR], calldata: , value: 0x0
Caller: [ATTACKER], function: initialize(address,address), txdata: 0x485cc955000000000000000000000000deadbeefdeadbeefdeadbeefdeadbeefdeadbeef000000000000000000000000deadbeefdeadbeefdeadbeefdeadbeefdeadbeef, value: 0x0
Caller: [ATTACKER], function: delegate(address), txdata: 0x5c19a95c0000000000000000000000001e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e, value: 0x0

### Tools used
Manual test


==============================================================================================



### External Call To User-Supplied Address

Contract: GovTokenEscrow
Function name: pay(address,uint256)
PC address: 1623

### Impact
A call to a user-supplied address is executed.
An external message call to an address specified by the caller is executed. Note that the callee account might contain arbitrary code and could re-enter any function within this contract. Reentering the contract in an intermediate state may lead to unexpected behaviour. Make sure that no state modifications are executed after this call and/or reentrancy guards are in place.
--------------------
In file: [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L45)

token.transfer(recipient, amount)

--------------------
Initial State:

Account: [CREATOR], balance: 0x4000000007b2b5, nonce:0, storage:{}
Account: [ATTACKER], balance: 0x820080, nonce:0, storage:{}

Transaction Sequence:

Caller: [CREATOR], calldata: , value: 0x0
Caller: [ATTACKER], function: initialize(address,address), txdata: 0x485cc955000000000000000000000000deadbeefdeadbeefdeadbeefdeadbeefdeadbeef0000000000000000000000000000000000000000000000000000000000000000, value: 0x0
Caller: [ATTACKER], function: pay(address,uint256), txdata: 0xc4076876000000000000000000000000010101010101010101010101010101021e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e1e, value: 0x0

### Tools used
Manual test