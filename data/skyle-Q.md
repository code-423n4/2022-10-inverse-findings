## Use a 2-step ownership transform pattern
It's possible that the gov role mistakenly transfer ownership to a wrong addresss, resulting a loss of the gov role. Consider first nominate an address as the pendingGov and acceptOwnership function is called by the pendingGov to confirm the gov role transfer

### Fed.sol
Fed.sol:48 changeGov(address _gov) public


 