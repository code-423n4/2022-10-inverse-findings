1. Gas optimization
The setOperator, allow, deny and borrowAllowed functions of the BorrowController contract can be modified as external to save gas consumption when calling.
The market contract's setOracle, setBorrowController, setGov, setLender, setLiquidationFactorBps, setReplenismentIncentiveBps, setLiquidationIncentiveBps, setLiquidationFeeBps, recall, pauseBorrows, depositAndBorrow, getWithdrawalLimit, borrowOnBehalf, withdrawOnBehalf, invalidateNonce, repayAndWithdraw, forceReplenish, and getLiquidatable functions can be modified to be external when calling the savingDebte and liquidatable functions. gas consumption.
The initialize, pay and delegate functions of the GovTokenEscrow contract can be modified to be external to save gas consumption when calling.
The setPendingOperator, claimOperator, addMinter, removeMinter, addMarket, addMarket, approve, transfer, transferFrom, permit and delegate functions of the DolaBorrowingRights contract can be modified to be external to save gas consumption when calling.

2. Missing zero address judgment
The constructor, setOperator, allow, deny and borrowAllowed functions of the BorrowController contract do not determine whether the input address is not a 0 address.
In the setPendingOperator, setFeed and setFixedPrice functions of the Oracle contract, it is not determined whether the operator address is the 0 address.


3. Unused parameters
In the borrowAllowed function of the BorrowController contract, the last two input parameters are not used.

4. Permission change proposal
In the BorrowController contract, to avoid entering wrong addresses when changing permissions, it is recommended to add a pendingOpeator. When calling the setOperator function, first set the pendingOpeator to newOperator. Then add an acceptOperator function, and the newOperator address calls acceptOperator to accept operator permissions. 
The same is true for other functions of permission address change in this project.

5. Missing event trigger
In the BorrowController contract, setOperator, allow, and deny functions are recommended to trigger the corresponding events to monitor the real-time status of the contract off-chain.
The setPendingOperator, setFeed and setFixedPrice functions in the BorrowController contract are suggested to trigger the corresponding events to monitor the real-time status of the contract off-chain.

6. Fixed compiler version
It is recommended to fix the compiler version to help ensure that the contract is not deployed with the latest compiler that may have undiscovered bugs.

7. The return value of the token transfer function is not judged.
The code on lines 205, 281, 400, 538, 571, and 603 in the Market contract does not judge the return value.


