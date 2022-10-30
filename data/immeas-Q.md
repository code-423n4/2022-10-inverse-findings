# Low

## L-1 consider two step ownership transfer for `Market` and `Fed`

As in `DBR.sol`, to prevent accidental transfer to invalid addresses.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
```javascript
File: src/Market.sol

130:    function setGov(address _gov) public onlyGov { gov = _gov; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L51
```javascript
File: src/Fed.sol

48:    function changeGov(address _gov) public {
49:        require(msg.sender == gov, "ONLY GOV");
50:        gov = _gov;
51:    }
```

## L-2 consider using `safeTransfer/From` or a `safeTransfer/From` library

Even though there might be a strict vetting of what tokens can be used as collateral, there's little cost of using `safeTransfer`. That will give you the option of using more types of collateral. Otherwise you would have to develop a new Market contract, which could impact trust.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L280
```javascript
File: src/Market.sol

280:        collateral.transferFrom(msg.sender, address(escrow), amount);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L38
```javascript
File: src/escrows/SimpleERC20Escrow.sol

38:        token.transfer(recipient, amount);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L45
```javascript
File: src/escrows/GovTokenEscrow.sol

45:        token.transfer(recipient, amount);
```

# Non-crit

## NC-1 use `1e18` instead of `1 ether`

Please use scientific notation for better code readability. `1 ether` also implies that it has something to do with native ether which it doesn't.

*There are 6 instances of this issue:*

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L315
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L326
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L597
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606
```javascript
File: src/Market.sol:

315:        return collateralBalance * oracle.viewPrice(address(collateral), collateralFactorBps) / 1 ether;

326:        return collateralBalance * oracle.getPrice(address(collateral), collateralFactorBps) / 1 ether;

360:        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

597:        uint liquidatorReward = repaidDebt * 1 ether / price;

606:            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;

```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L72
```javascript
File: src/escrows/INVEscrow.sol:

72:        uint invBalanceInXInv = xINV.balanceOf(address(this)) * xINV.exchangeRateStored() / 1 ether;
```

## NC-2 `oracle.getPrice()` is better named `updatePrice()`

`get` and `view` are very easy to mix up. Also, `get` implies no side effects while update/getAndUpdate would imply that there are changes as well.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L112
```javascript
File: src/Oracle.sol

78:     function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
    
112:    function getPrice(address token, uint collateralFactorBps) external returns (uint) {
```

## NC-3 open todos

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35
```javascript
File: src/escrows/INVEscrow.sol

35:         xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```

## NC-4 very long row

GitHub starts using a scroll bar when the length is over 164 characters, the lines below should be split when they reach that length.

*There are 4 instances of this issue:*

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12
```javascript
File: src/Oracle.sol

12:The Pessimistic Oracle introduces collateral factor into the pricing formula. It ensures that any given oracle price is dampened to prevent borrowers from borrowing more than the lowest recorded value of their collateral over the past 2 days.
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L99
```javascript
File: src/Fed.sol

99:     @dev Markets can have more DOLA in them than they've been supplied, due to force replenishes. This call will revert if trying to contract more than have been supplied.
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L16
```javascript
File: src/escrows/GovTokenEscrow.sol

16: This specific escrow is meant as an example of how an escrow can be implemented that allows depositors to delegate votes with their collateral, unlike pooled deposit protocols.
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L25
```javascript
File: src/escrows/INVEscrow.sol

25: This escrow allows user to deposit INV collateral directly into the xINV contract, earning APY and allowing them to delegate votes on behalf of the xINV collateral
```

## NC-5 misspelled comment

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L589
```javascript
File: src/Market.sol

589:    @param repaidDebt Th amount of user user debt to liquidate.
```