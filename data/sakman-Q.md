## 1. Events not emmited

_src/escrows/INVEscrow.sol:_ [L44](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L44)

_src/Fed.sol:_ [L48](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L48)
[L57](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L57)
[L75](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L75)

_src/Market.sol:_ [L149](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L149)
[L161](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L161)
[L183](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L183)
[L194](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L194)
[L212](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L212)

_src/DBR.sol:_ [L53](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L53)
[L62](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L62)

_src/escrows/GovTokenEscrow.sol:_ [L30](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L30)

_src/escrows/SimpleERC20Escrow.sol:_ [L25](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L25)

## 2. Event is missing `indexed` fields

_src/DBR.sol:_ [L381](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L381)
[L382](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L382)

_src/Market.sol:_ [L616](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L616)
[L617](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L617)
[L619](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L619)

## 3. Use `external` instead of `public` for the following functions

_src/escrows/GovTokenEscrow.sol:_ [L30](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L30)
[L43](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L43)
[L52](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L52)

_src/Market.sol:_ [L161](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L161)
[L172](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L172)
[L183](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L183)
[L194](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L194)
[L212](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L212)
[L559](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L559)
[L575](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L575)
[L578](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L578)
[L591](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L591)

_src/escrows/INVEscrow.sol:_ [L44](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L44)
[L59](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L59)
[L79](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L79)

_src/Oracle.sol:_ [L66](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L66)

_src/Fed.sol:_ [L48](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L48)
[L57](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L57)
[L66](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L66)
[L75](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L75)
[L131](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L131)

_src/escrows/SimpleERC20Escrow.sol:_ [L25](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L25)
[L36](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L36)
[L45](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L45)

_src/DBR.sol:_ [L53](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L53)
[L81](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L81)
[L90](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L90)
[L99](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L99)
[L146](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L146)
[L158](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L158)
[L188](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L188)
[L215](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L215)
[L258](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L258)
[L300](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L300)
[L325](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L325)

_src/BorrowController.sol:_ [L46](https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L46)

## 4. Only libraries, abstract contracts and interfaces should use multiple compiler versions

_src/escrows/INVEscrow.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L2)

_src/escrows/SimpleERC20Escrow.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L2)

_src/Fed.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L2)

_src/BorrowController.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L2)

_src/Oracle.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L2)

_src/Market.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L2)

_src/escrows/GovTokenEscrow.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L2)

_src/DBR.sol:_ [L2](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L2)

