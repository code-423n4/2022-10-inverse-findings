### Unspecific Compiler Version Pragma

#### Impact
Issue Information: [L003](https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l003---unspecific-compiler-version-pragma)

#### Findings:
```
BorrowController.sol::2 => pragma solidity ^0.8.13;
DBR.sol::2 => pragma solidity ^0.8.13;
Fed.sol::2 => pragma solidity ^0.8.13;
Market.sol::2 => pragma solidity ^0.8.13;
Oracle.sol::2 => pragma solidity ^0.8.13;
escrows/GovTokenEscrow.sol::2 => pragma solidity ^0.8.13;
escrows/INVEscrow.sol::2 => pragma solidity ^0.8.13;
escrows/SimpleERC20Escrow.sol::2 => pragma solidity ^0.8.13;
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)
