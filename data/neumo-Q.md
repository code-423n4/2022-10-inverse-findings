## 1.- Lack of zero address checks:
| File          | Function      | Variable  | Line |
| ------------- |-------------| -----|  -----|
| **Market.sol**      | constructor | _gov | [77](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77) | 
|       | constructor | _lender | [78](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L78) | 
|       | constructor | _pauseGuardian | [79](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L79) | 
|       | constructor | _escrowImplementation | [80](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80) | 
|       | constructor | _dbr | [81](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L81) | 
|       | constructor | _collateral | [82](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L82) | 
|       | constructor | _oracle | [83](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L83) | 
|       | setOracle | _oracle | [118](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118) | 
|       | setGov | _gov | [130](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130) | 
|       | setLender | _lender | [136](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136) | 
|       | setPauseGuardian | _pauseGuardian | [142](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142) | 
|       | createEscrow | user | [234](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L234) | 
|       | borrowInternal | borrower | [395](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395) | 
|       | withdrawInternal | from | [463](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L463) | 
|       | withdrawInternal | to | [464](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L464) | 
|       | repay | user | [536](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L536) | 
|       | forceReplenish | user | [565](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565) | 
|       | liquidate | user | [593](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L593) | 
| **Fed.sol**      | constructor | _dbr | [37](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L37) | 
|       | constructor | _dola | [38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L38) | 
|       | constructor | _gov | [39](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39) | 
|       | constructor | _chair | [40](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L40) | 
| **DBR.sol**      | constructor | _operator | [39](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39) | 
|       | addMinter | minter_ | [82](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L82) | 
|       | addMarket | market_ | [100](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L100) | 
| **BorrowController.sol**      | constructor | _operator | [14](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14) | 
|       | setOperator | _operator | [26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26) | 
| **Oracle.sol**      | constructor | _operator | [32](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32) | 
| **GovTokenEscrow.sol**      | initialize | _token | [33](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L33) | 
|       | initialize | _beneficiary | [34](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L34) | 
|       | pay | recipient | [45](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L45) | 
|       | delegate | delegatee | [68](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L68) | 
| **INVEscrow.sol**      | constructor | _xINV | [35](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35) | 
|       | initialize | _token | [47](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L47)  | 
|       | initialize | _beneficiary | [48](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L48)  | 
|       | pay | recipient | [63](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L63)  | 
|       | delegate | delegatee | [92](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L92)  | 
| **SimpleERC20Escrow.sol**      | initialize | token | [28](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L28) | 
|       | pay | recipient | [38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L38) | 

## 2.- In BorrowControler.sol, function setOperator should be a two step process.
Function **setOperator** changes right away the operator of the contract with the address passed as parameter. In case there's an error in the address passed, the contract will not have operator, which can have disastrous consequences, because functions **allow** and **deny** will not be usable. Consider turning the change of operator into a two step process, for example with two functions: **setPendingOperator** and **claimOperator**.
## 3.- In Fed.sol, function changeGov should be a two step process.
Function **changeGov** changes right away the governor of the contract with the address passed as parameter. In case there's an error in the address passed, the contract will not have governor, which can have disastrous consequences, because functions **changeSupplyCeiling** and **changeChair** will not be usable. Furthermore, function **takeProfit** will send the profit to a wrong address. Consider turning the change of governor into a two step process, for example with two functions: **setPendingGov** and **claimGov**.
## 4.- In Market.sol, function setGov should be a two step process.
Function **setGov** changes right away the governor of the contract with the address passed as parameter. In case there's an error in the address passed, the contract will not have governor, which can have disastrous consequences, because functions **setLender**, **setPauseGuardian**, **setCollateralFactorBps**, **setLiquidationFactorBps**, **setReplenismentIncentiveBps**, **setLiquidationIncentiveBps**, **setLiquidationFeeBps** and **pauseBorrows** (when unpausing)  will not be usable. Furthermore, function **liquidate** will send the fee to a wrong address. Consider turning the change of governor into a two step process, for example with two functions: **setPendingGov** and **claimGov**.
## 5.- Centralization risk.
Functions only callable by centralized addresses can have a big impact in user funds.
| File          | Function      | Line |
| ------------- |-----|  -----|
| **Oracle.sol**      | setFeed | [53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53) | 
|       | setFixedPrice | [61](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61) | 
| **BorrowController.sol**      | deny | [38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38) | 
| **DBR.sol**      | addMinter | [81](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81) | 
|       | mint | [349](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L349) | 
| **Fed.sol**      | changeChair | [66](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66) | 
| **Market.sol**      | setCollateralFactorBps | [149](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149) | 
|     | setLiquidationFactorBps | [161](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161) | 
|     | setReplenismentIncentiveBps | [172](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172) | 
|     | setLiquidationIncentiveBps | [183](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183) | 
|     | setLiquidationFeeBps | [194](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194) | 
|     | pauseBorrows | [212](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212) | 
## 6.- Immutable variables persist across proxies.
In **INVEscrow.sol** there is a [comment](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35) that says `// TODO: Test whether an immutable variable will persist across proxies`. When the escrow implementation is deployed, the constructor substitutes in the contract bytecode all instances of `xINV` whith the address supplied, so it is not stored in the `storage`. This comment can be deleted, because proxies will all see the `xINV` address supplied at implementation deployment.
## 7.- escrowImplementation cannot be modified.
If a bug is found in an already deployed `escrowImplementation` there is no way of changing it because the escrows are not upgradeable and also because there is no function that allows this in the `Market.sol` contract. Consider creating an `onlyGov` function `setEscrowImplementation` to allow the change (this would introduce a new centralization risk, though).
## 8.- replenishmentIncentiveBps can be set to 0 at construction but not in the setter.
In the [Market.sol constructor](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L85), variable `replenishmentIncentiveBps` can be set to zero, but the setter, `setReplenismentIncentiveBps` [does not allow zero](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173) as a possible value (the minimum would be then a 0.01% incentive). Consider using the same constraints in both locations, for coherence.
