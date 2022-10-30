1. Should use the latest solidity version 0.8.17. 
Right now, 0.8.13 is used. The latest solidity version contains more bug-fixed and updated functionality.

2. Wrong value range. Should include 10000 like what is written in the comment section.
require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

should be changed to 

require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps <= 10000, "Invalid replenishment incentive");

(See https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L169)

Also, apply to 

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195

