### Internal Function Called Only Once Can be Inlined

#### Issue
Certain function is defined even though it is called only once.
Inline it instead to where it is called to avoid usage of extra gas.

#### PoC
Total of 2 instances found.

1. createEscrow() of Market.sol
This function called only once at line 247
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L226
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L247

2. getWithdrawalLimitInternal() of Market.sol
This function called only once at line 461
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L353
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L461

#### Mitigation
I recommend to not define above functions and instead inline it at place it is called.

&ensp;
### Boolean Comparisons

#### Issue
It is more gas expensive to compare boolean with "variable == true" or "variable == false" than 
directly checking the returned boolean value.

#### PoC
```
./DBR.sol:350:        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

#### Mitigation
Simply check by returned boolean value.
Change it to
```
require(!somethinghere)
```

&ensp;
### Use require instead of &&

#### Issue
When there are multiple conditions in require statement, break down the require statement into
multiple require statements instead of using && can save gas.

#### PoC
```
./DBR.sol:249:            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
./Market.sol:75:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
./Market.sol:162:        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
./Market.sol:173:        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
./Market.sol:184:        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
./Market.sol:195:        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
./Market.sol:448:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
./Market.sol:512:            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```

#### Mitigation
Break down into several require statement.

&ensp;
