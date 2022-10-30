# QA Report
## NC Found [3]

### [NC-01] Pragma is not Forced (Floating Pragma)

https://swcregistry.io/docs/SWC-103

#### Findings [7]:

*/src/Oracle.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```

*/src/escrows/INVEscrow.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/escrows/INVEscrow.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```

*/src/escrows/SimpleERC20Escrow.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/escrows/SimpleERC20Escrow.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```

*/src/escrows/GovTokenEscrow.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/escrows/GovTokenEscrow.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```

*/src/Fed.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Fed.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```

*/src/Market.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```

*/src/BorrowController.sol*
Line(s): [2](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/BorrowController.sol#L2)
```solidity
2:	pragma solidity ^0.8.13;
```


### [NC-02] Require Statements Missing Error Message

Error messages help with troubleshooting, some require statements are missing error messages.

#### Findings [2]:

*/src/escrows/INVEscrow.sol*
Line(s): [91](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/escrows/INVEscrow.sol#L91)
```solidity
91:	require(msg.sender == beneficiary);
```

*/src/escrows/GovTokenEscrow.sol*
Line(s): [67](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/escrows/GovTokenEscrow.sol#L67)
```solidity
67:	require(msg.sender == beneficiary);
```


### [NC-03] Underscore Notation Improves Readability

Solidity allows for the use of _ between every 3 digits of large numbers.

#### Findings [23]:

*/src/Oracle.sol*
Line(s): [95](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L95), [98](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L98), [134](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L134), [137](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Oracle.sol#L137)
```solidity
95:	uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
98:	uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
134:	uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
137:	uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
```

*/src/Market.sol*
Line(s): [51](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L51), [74](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L74), [75](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L75), [76](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L76), [150](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L150), [162](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L162), [173](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L173), [184](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L184), [195](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L195), [336](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L336), [346](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L346), [360](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L360), [377](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L377), [563](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L563), [564](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L564), [583](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L583), [595](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L595), [598](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L598), [606](https://github.com/code-423n4/2022-10-inverse/blob/main/./src/Market.sol#L606)
```solidity
51:	uint public liquidationFactorBps = 5000;
74:	require(_collateralFactorBps < 10000, "Invalid collateral factor");
75:	require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
76:	require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
150:	require(_collateralFactorBps < 10000, "Invalid collateral factor");
162:	require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
173:	require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
184:	require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
195:	require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
336:	return collateralValue * collateralFactorBps / 10000;
346:	return collateralValue * collateralFactorBps / 10000;
360:	uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
377:	uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
563:	uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;
564:	uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;
583:	return debt * liquidationFactorBps / 10000;
595:	require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");
598:	liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
606:	uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
```