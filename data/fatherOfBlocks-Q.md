**Market.sol**
- L360/377/596/597/606 - A division is made by a value obtained in another contract that is an oracle, it should be validated before that it is != 0 and return a message if it reverts

- L8/27 - Multiple interfaces are created in the contract, there are functions that are never used, therefore they are not necessary.


**BorrowController.sol**
- L26 - The current operator can set the new operator, it should be used that newOperator != 0 && operator != newOperator


**Oracle.sol**
- L98/137 - A division is performed by a collateralFactorBps variable, it should be validated before that it is != 0 and return a message if it reverts.


**Fed.sol**
- L93 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.

- L48/57/66/75/86/103/131 - Most of the functions are public but they are not used in any part of the contract, nor is it used to inherit, therefore the correct thing would be for them to be externals.

- L131 - The takeProfit() function does the recall and the transfer only if there is a profit, this has a small problem that is that it does not inform the user if a profit is taken or not. This should be improved by either generating an event or returning a bool.


**escrows/GovTokenEscrow.sol**
- L7 - An interface is created but the transferFrom() function is never used, so it is not necessary.

- L67 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.


**escrows/INVEscrow.sol**
- L91 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.

- L45/60/91 - Most of the functions are public but they are not used in any part of the contract, nor is it used to inherit, therefore the correct thing would be for them to be externals.

- L7 - An interface is created but the transferFrom() function is never used, so it is not necessary.


**escrows/SimpleERC20Escrow.sol**
- L7 - Se crea una interfaz pero la funcion transferFrom() nunca es utilizada, por lo tanto no es necesario que este.


**DBR.sol**
- L53/62/70/81/90/99/109/146/158/170/188/215/258/300/313/325/340/349 - Most of the functions are public but they are not used in any part of the contract, nor is it used to inherit, therefore the correct thing would be for them to be externals.
