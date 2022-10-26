# Multiply Before Dividing To Avoid Rounding Errors
In Solidity, it is required to always multiply before dividing to prevent rounding related issues to happen.

### Market.sol
Here are the followed recommendations:

`getWithdrawalLimitInternal` function - https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360

    uint minimumCollateral = debt * 1 ether * 10000 / (oracle.getPrice(address(collateral), collateralFactorBps) * collateralFactorBps);

`getWithdrawalLimit` function - https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377

    uint minimumCollateral = debt * 1 ether * 10000 / (oracle.viewPrice(address(collateral), collateralFactorBps) * collateralFactorBps);

`liquidate` function - https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606

    uint liquidationFee = repaidDebt * 1 ether * liquidationFeeBps / (price * 10000);