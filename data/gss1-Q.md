## Impact
Due to underflow, the function getPrice(), viewPrice() could be stuck. 

## Proof of Concept
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L87
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L121

```solidity
uint8 feedDecimals = feeds[token].feed.decimals();
uint8 tokenDecimals = feeds[token].tokenDecimals;
uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

feedDecimals is a return value of Chainlink.
tokenDecimals is a value set by setFeed().
The only operator can call setFeed().

When the worst case(feedDecimals + tokenDecimals > 36) happens, it would generate underflow and because of solidity version, it is reverted.

## Tools Used
vs code

## Recommended Mitigation Steps

In setFeed(), we can add a require statement.