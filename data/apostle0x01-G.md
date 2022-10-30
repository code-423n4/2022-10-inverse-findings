## [G-01] Splitting require() statements that use && saves gas

### IMPACT

Require statements including conditions with the && operator can be broken down in
multiple require statements to save gas.

### POC
	
```console
src/DBR.sol:249:            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
src/Market.sol:75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
src/Market.sol:162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
src/Market.sol:173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
src/Market.sol:184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
src/Market.sol:195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
src/Market.sol:448:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
src/Market.sol:512:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

### MITIGATION

Breakdown each condition in a separate require
```
	require(numSells == buys.length,'mismatched lengths');
	require(numSells == constructs.length, 'mismatched lengths');
```
