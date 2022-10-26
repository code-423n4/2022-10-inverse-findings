**Market.sol**
- L74/75/76/93/150/162/173/184/195/204/214/216/236/390/392/396/423/448/462/487/512/533/561/562/567/592/594/595 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L76/214 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS 

- L75/162/173/184/195/561/592 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L313/314/324/325/335/336/345/346/354/355/461/462/463/464 - When a variable is only used once, it doesn't make much sense to create a variable, it could be used directly in the function that is needed.

- L438/502/521 - It is less expensive to do ++i or --i, rather than i++, i-- or i - 1.


**BorrowController.sol**
- L18 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**Oracle.sol**
- L36/67/83/117 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L79/83/97/113/117/135/136 - It is less expensive to validate that uint != 0 than to validate uint > 0


**Fed.sol**
- L49/58/67/76/87/88/89/93/104/105/107 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L89 - When we want to validate conditions that are bools instead of validating (condition != true or condition == false) it is less expensive to validate (condition or !condition)

- L133 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L124 -  As it is validated with the if that supply >= marketValue, the operation marketValue - supply can be unchecked.


**escrows/GovTokenEscrow.sol**
- L31/44/67 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**escrows/INVEscrow.sol**
- L31/44/67 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L62 - As it is validated with the if that invBalance < amount, the operation amount - invBalance can be unchecked.

- L81 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L71/72/73 - When a variable is only used once, it doesn't make much sense to create a variable, it could be used directly in the function that is needed.


**escrows/SimpleERC20Escrow.sol**
- L26/37 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**DBR.sol**
- L45/63/71/171/195/224/249/301/303/314/326/328/329/350/373 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L63/328 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L350 - When we want to validate conditions that are bools instead of validating (condition != true or condition == false) it is less expensive to validate (condition or !condition)

- L111/171/172/195/196 - As it is validated with the if that balanceOf(from) >= amount, the operation balances[from] -= amount can be unchecked.

- L121/122/134/135/147/148 - When a variable is only used once, it doesn't make much sense to create a variable, it could be used directly in the function that is needed.

- L63/326 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS 


