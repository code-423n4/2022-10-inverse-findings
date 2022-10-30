## Issues found

### Unsafe ERC20 Operation(s)

```
src/escrows/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
src/escrows/INVEscrow.sol::50 => _token.approve(address(xINV), type(uint).max);
src/escrows/INVEscrow.sol::63 => token.transfer(recipient, amount);
src/escrows/SimpleERC20Escrow.sol::38 => token.transfer(recipient, amount);
```

### Unspecific Compiler Version Pragma

```
src/BorrowController.sol::2 => pragma solidity ^0.8.13;
src/DBR.sol::2 => pragma solidity ^0.8.13;
src/Fed.sol::2 => pragma solidity ^0.8.13;
src/Market.sol::2 => pragma solidity ^0.8.13;
src/Oracle.sol::2 => pragma solidity ^0.8.13;
src/escrows/GovTokenEscrow.sol::2 => pragma solidity ^0.8.13;
src/escrows/INVEscrow.sol::2 => pragma solidity ^0.8.13;
src/escrows/SimpleERC20Escrow.sol::2 => pragma solidity ^0.8.13;
```

### Not remove todo tag

```
src/escrows/INVEscrow.sol::35 => // TODO: Test whether an immutable variable will persist across proxies
```
