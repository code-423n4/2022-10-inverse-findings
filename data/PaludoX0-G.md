
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.solL#97
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.solL#136
It is more probable that newBorrowingPower < twoDayLow then twoDayLow < 0 and check is false, therefore the following if statement
use less gas in most of the cases when it's false
if( newBorrowingPower > twoDayLow && twoDayLow > 0)


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.solL#92/93
calculate globalSupply at #L90 before minting, so if globalSupply exceed supplyCeiling it'll revert before consuming other gas.


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183//src/Market.solL#293
No need to cache escrowImplementation since it is used once


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183//src/Market.solL#591 and 592
You can melt the two operation to one:
BEFORE
        uint liquidatorReward = repaidDebt * 1 ether / price;
        liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;
AFTER
        uint liquidatorReward = (repaidDebt * 1 ether / price)*(1+liquidatorReward * liquidationIncentiveBps / 10000);


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.solL#328
Actually function dbr.onForceReplenish is called only by function forceReplenish market.sol#L559
forceReplenish perform already following check require(deficit > 0, "No DBR deficit"); 
therefore there's no need to do this check again in onForceReplenish: remove check and add a note for future developements that calling function shall perform this check


https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.solL#24
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.solL#25
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.solL#26
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.solL#27
Use "private" instead of "public", variables are not called by inheriting contracts or by any other contract.
It saves gas either on deployment and during calls.
