## Use a 2-step ownership transform pattern
It's possible that the gov role mistakenly transfer ownership to a wrong addresss, resulting a loss of the gov role. Consider first nominate an address as the pendingGov and acceptOwnership function is called by the pendingGov to confirm the gov role transfer

### Fed.sol
Fed.sol:48 changeGov(address _gov) public

## A magic number should be documented and explained use a constant instead
### market.sol
Market.sol:150 require(_collateralFactorBps < 10000, "Invalid collateral factor");
Market.sol:162 require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol:195
Market.sol:336
Market.sol:346
Market.sol:377
Market.sol:563
Market.sol:564
Market.sol:583
Market.sol:598
Market.sol:606




 