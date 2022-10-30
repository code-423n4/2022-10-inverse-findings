## Cache feed decimals in `Oracle.FeedData`
Feed decimals is accessed by `viewPrice` and `getPrice`. As feed decimals cannot change in a Chainlink feed, it can be cached in a new field in `FeedData` to save gas.

## Recommended Implementation
```sol
struct FeedData {
    IChainlinkFeed feed;
    uint8 feedDecimals;
    uint8 tokenDecimals;
}
...
function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, feed.decimals(), tokenDecimals); }
...
uint8 feedDecimals = feeds[token].feedDecimals();
```
### Gas Savings
**-103812 (36.146%)** in Forge gas comparison with original implementation.

## Move `require` above state update in `Market.borrowInternal`
### Recommended Implementation
```sol
require(credit >= debts[borrower] + amount, "Exceeded credit limit");
debts[borrower] += amount;
```
### Gas Savings
**18289 (-5.962%)** in Forge gas comparison with original implementation.