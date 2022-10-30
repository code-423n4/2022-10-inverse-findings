# Immutable DOLA can be change to constant to safe GAS
inside `Market.sol` there is an immutable variable which can be replaced by constant to safe gas.

```
File: Market.sol
44:     IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
```

# Refactor `toggle`-like function from two function implementation into one function
inside `BorrowController.sol` there are two functions
```
File: BorrowController.sol
28:     /**
29:     @notice Allows a contract to use the associated market.
30:     @param allowedContract The address of the allowed contract
31:     */
32:     function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
33: 
34:     /**
35:     @notice Denies a contract to use the associated market
36:     @param deniedContract The addres of the denied contract
37:     */
38:     function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```

both function can be flatten into one function which will safe gas, for example
```
function setContractAllow(address _contract, bool _status) public onlyOperator { contractAllowlist[_contract] = _status; }
```

# Use unchecked for increments to safe gass
```
File: Market.sol
520:     function invalidateNonce() public {
521:         nonces[msg.sender]++;
522:     }
```

# Splitting require() statements that use && saves gas
```
File: DBR.sol
249:             require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

File: Market.sol
75:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

162:         require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

173:         require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

195:         require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

448:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

512:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

```