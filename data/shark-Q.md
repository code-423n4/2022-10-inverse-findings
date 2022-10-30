## Use `uint256` instead of `uint`

To be explicit, consider replacing all instances of `uint` with `uint256`.

Here is an example:

File: `Market.sol` [Line 593](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L593)

```
uint debt = debts[user];
```

The code above could be changed to:

```
uint256 debt = debts[user];
```

Here are a few more examples:

File: `Market.sol` [Line 531-532](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L531-L532)
File: `Market.sol` [Line 563-564](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563-L564)
File: `Market.sol` [Line 596-597](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L596-L597)
File: `Market.sol` [Line 606](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606)
File: `Market.sol` [Line 614-619](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L614-L619)

## `constant` variables should be capitalized

Constants should be named with all capital letters with underscores separating words.

One instance found: File: `DBR.sol` [Line 13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)

## Insufficient NatSpec

It is recommended that Solidity contracts are fully annotated using NatSpec.

Here are some instances:
File: `Market.sol` [Line 11-14](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L11-L14)
File: `Market.sol` [Line 16-21](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L16-L21)
File: `Market.sol` [Line 23-30](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L23-L30)

## Lines are too long

Maximum suggested line length is 120 characters.

Here is an example:
File: `Oracle.sol` [Line 12-14](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12-L14)

## Contradicting comment

File: `Market.sol` [Line 166-175](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L166-L175)

The `@dev` comment states that `@param _replenishmentIncentiveBps` must be set between 1 and 10000:

```
169     @dev Must be set between 1 and 10000.
```

But the `require()` statement states that it must be less than 10000

```
173     require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
```

Meaning that the value cannot be set between 1 and 10000, but instead between 1 and 9999.

## Typos

File: `Market.sol` [Line 172](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172)

```
function setReplenismentIncentiveBps
```

Change "Replenisment" to "Replenishment".

File: `Market.sol` [Line 181](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L181)

```
@param _liquidationIncentiveBps The new liqudation incentive set in basis points. 1 = 0.01%
```

Change "liqudation" to "liquidation".

File: `DBR.sol` [Line 50](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50)

```
@notice Sets pending operator of the contract. Operator role must be claimed by the new oprator. Only callable by Operator.
```

Change "oprator" to "operator".

File: `DBR.sol` [Line 87](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87)

```
@notice Removes a minter from the set of addresses allowe to mint DBR tokens. Only callable by Operator.
```

Change "allowe" to "allowed"

File: `BorrowController.sol` [Line 36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36)

```
@param deniedContract The addres of the denied contract
```

Change "addres" to "address"
