- Contracts are unlicensed.
	- It is recommended to add liscense for the source code.

- Use of floating pragma
	- Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

- Split interfaces from contracts.
	- Interfaces are defined within contracts. And Interfaces like IERC20 are defined in multiple contracts. It is recommended to separate the interfaces from the contracts.

- Add NatSpec tag.
	- Add `@dev` tag. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L4


- Remove unnecessary code: 
	- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L56-L60
	- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L49-L53
