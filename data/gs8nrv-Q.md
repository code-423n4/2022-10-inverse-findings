## Setters do not check that new addresses are not address(0)

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L159
No check on `spender` different from the address(0). This are conventional checks, you can check OZ implementation (https://github.com/OpenZeppelin/openzeppelin-contracts/blob/36951d58386b9fee81b237e6c6626c9115ccef3a/contracts/token/ERC20/ERC20.sol#L316)

## No events in the BorrowwController.sol

When setting a new operator (https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26), or allowing/disallowing a new contract (https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32), there aren't any events trigger making hard for off chain tracking.