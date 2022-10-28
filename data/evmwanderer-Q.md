## Summary

Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| 1 | `latestAnswer()` is deprecated | 2 |
| 2 | Open TODOs | 1 |
| 3 | Proxy implementation contract not initialized | 3 |
| 4 | User could mint DBR for free | 1 |
| 5 | Oracle lows are observational not actual | 1 |

Code Style
| |Issue|Instances|
|-|:-|:-:|
| 1 | Rename function | 1 |

## Non-critical Issues

### 1. `latestAnswer()` is deprecated
As per Chainlink's [documentation](https://docs.chain.link/docs/data-feeds/price-feeds/api-reference/#latestanswer), the `latestAnswer` function is deprecated. 

*There are 2 instances of this issue:*
```solidity
File: src/Oracle.sol   #1

82:           uint price = feeds[token].feed.latestAnswer();
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L82

```solidity
File: src/Oracle.sol   #2

116:           uint price = feeds[token].feed.latestAnswer();
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L116


### 2. Open TODO(s)
Open questions should be resolved before deployment

*There is one instance of this issue:*
```solidity
File: src/escrows/INVEscrow.sol   #1

35:      xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L35


### 3. Proxy implementation contract not initialized
The escrow implementation contracts can be initialised by anyone calling the initialize function. Not critical in this case.

[Reference 1](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#initializing_the_implementation_contract](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#initializing_the_implementation_contract)

[Reference 2](https://blog.trailofbits.com/2020/12/16/breaking-aave-upgradeability/)

*There are 3 instances of this issue:*
```solidity
File: src/escrows/GovTokenEscrow.sol   #1

30:   function initialize(IERC20 _token, address _beneficiary) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L30

```solidity
File: src/escrows/INVEscrow.sol   #2

44:   function initialize(IERC20 _token, address _beneficiary) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L44

```solidity
File: src/escrows/SimpleERC20Escrow.sol   #3

25:   function initialize(IERC20 _token, address _beneficiary) public {
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L25

### 3. User could mint DBR for free
Due to loss of precision no cost is accrued in the following function if

$amount < 10000/ replenishmentPriceBps$

```solidity
File: src/Market.sol  

function forceReplenish(address user, uint amount) public {

uint deficit = dbr.deficitOf(user);

require(deficit > 0, "No DBR deficit");

require(deficit >= amount, "Amount > deficit");

uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;

uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;

debts[user] += replenishmentCost;

uint collateralValue = getCollateralValueInternal(user);

require(collateralValue >= debts[user], "Exceeded collateral value");

totalDebt += replenishmentCost;

dbr.onForceReplenish(user, amount);

dola.transfer(msg.sender, replenisherReward);

emit ForceReplenish(user, msg.sender, amount, replenishmentCost, replenisherReward);

}

```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L559

DBR could be minted in the called function if a user is in deficit.

```solidity
File: src/DBR.sol  

function onForceReplenish(address user, uint amount) public {

require(markets[msg.sender], "Only markets can call onForceReplenish");

uint deficit = deficitOf(user);

require(deficit > 0, "No deficit");

require(deficit >= amount, "Amount > deficit");

uint replenishmentCost = amount * replenishmentPriceBps / 10000;

accrueDueTokens(user);

debts[user] += replenishmentCost;

_mint(user, amount);

}
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L325

This is dust amount unless the collateral decimals are very low and `replenishmentPriceBps` is also very low.

### 5. Oracle's lows are observational not actual
The oracle design relies on the assumption that low prices are observed by the smart contract. There is no guarantee nor incentives that this would happen. In rarely used markets this could defy the original dampening intention. 

```solidity
File: src/Oracle.sol   #1

function getPrice(address token, uint collateralFactorBps) external returns (uint) {

if(fixedPrices[token] > 0) return fixedPrices[token];

if(feeds[token].feed != IChainlinkFeed(address(0))) {

// get price from feed

uint price = feeds[token].feed.latestAnswer();

require(price > 0, "Invalid feed price");

// normalize price

uint8 feedDecimals = feeds[token].feed.decimals();

uint8 tokenDecimals = feeds[token].tokenDecimals;

uint8 decimals = 36 - feedDecimals - tokenDecimals;

uint normalizedPrice = price * (10 ** decimals);

// potentially store price as today's low

uint day = block.timestamp / 1 days;

uint todaysLow = dailyLows[token][day];

if(todaysLow == 0 || normalizedPrice < todaysLow) {

dailyLows[token][day] = normalizedPrice;

todaysLow = normalizedPrice;

emit RecordDailyLow(token, normalizedPrice);

}

// get yesterday's low

uint yesterdaysLow = dailyLows[token][day - 1];

// calculate new borrowing power based on collateral factor

uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;

if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {

uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;

return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;

}

return normalizedPrice;

}

revert("Price not found");

}

```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L112

## Code Style

### 1. Rename function
Although this name is used in a popular library, it is not expressive enough;
`calculateEscrowAddress` would be a better name.

```solidity
File: src/Market.sol   #1

292:      function predictEscrow(address user) public view returns (IEscrow predicted)
```
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L292
