Multiple SLOADs within function use excessive gas, gas saved by SLOAD once and storing to memory, utilising much cheaper OP code MLOAD within the function.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L353-L363

this is also present in the public view function getWithdrawalLimit(), however, as this function is not involved in any write functions, there would be no practical optimisation for gas. The reason getWithdrawalLimitInternal() provides gas optimisation is that the function is called during the withdrawInternal() function which is invoke by the public write function withdraw() and therefore with incur additional gas costs during the SLOAD calls.

suggested solution:

function getWithdrawalLimitInternal(address user) internal returns (uint) {
        IEscrow escrow = predictEscrow(user);
        uint _collateralFactorBps = collateralFactorBps;
        uint collateralBalance = escrow.balance();
        if (collateralBalance == 0) return 0;
        uint debt = debts[user];
        if (debt == 0) return collateralBalance;
        if (_collateralFactorBps == 0) return 0;
        uint minimumCollateral = (((debt * 1 ether) /
            oracle.getPrice(address(collateral), _collateralFactorBps)) *
            10000) / _collateralFactorBps;
        if (collateralBalance <= minimumCollateral) return 0;
        return collateralBalance - minimumCollateral;
    }
