# Cache Storage Variables in Memory Variable In Function
When a storage variable from a contract is used at least 2 times in the same function, it is possible to reduce gas consumption by caching it into a memory variable. This way we avoid additional SLOAD.

### Market.sol
The function `getWithdrawalLimitInternal` calls the variable `collateralFactorBps` 3 times, so it can be cached : https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353:L363

    function getWithdrawalLimitInternal(address user) internal returns (uint) {
        IEscrow escrow = predictEscrow(user);
        uint collateralBalance = escrow.balance();
        if(collateralBalance == 0) return 0;
        uint debt = debts[user];
        if(debt == 0) return collateralBalance;

        uint _collateralFactorBps = collateralFactorBps;
        if(_collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), _collateralFactorBps) * 10000 / _collateralFactorBps;
        if(collateralBalance <= minimumCollateral) return 0;
        return collateralBalance - minimumCollateral;
    }

The `liquidate` function calls the variable `liquidationFeeBps` 2 times when condition is passed (which is most likely to happen) : https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605:L610

    uint _liquidationFeeBps = liquidationFeeBps;
    if (_liquidationFeeBps > 0) {
        uint liquidationFee = repaidDebt * 1 ether / price * _liquidationFeeBps / 10000;
        if(escrow.balance() >= liquidationFee) {
            escrow.pay(gov, liquidationFee);
        }
    }