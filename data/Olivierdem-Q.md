1. TODO left in the code. Line 35 of INVEscrow.sol.

2. Use of unsafe transfer function for erc20 in GovTokenEscrow

3. Unify naming convention for variable names. In DBR, some uint are uint256, some are just uint. Explicitly name all uint 'uint256' for uniformity and clarity.

4. Emit event when updating variable affecting the protocol (ex: setReplenishmentPriceBps() in DBR.sol, or setGov() and pauseBorrows() in Market.sol)

5. It's better practice to divide before multiply. See. https://github.com/crytic/slither/wiki/Detector-Documentation#divide-before-multiply. But Market.sol does a number of multiplication then division, in getWithdeawLimitInternal() for exemple.