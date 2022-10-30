### [L-01] Missing events for admin functions that change critical parameters
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61

```solidity
    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53

```solidity
    function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
```
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L161-L197

```solidity
    function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
        liquidationFactorBps = _liquidationFactorBps;
    }

    /**
    @notice sets the Replenishment Incentive of the market as denoted in basis points.
     The Replenishment Incentive is the percentage paid out to replenishers on a successful forceReplenish call, denoted in basis points.
    @dev Must be set between 1 and 10000.
    @param _replenishmentIncentiveBps The new replenishment incentive set in basis points. 1 = 0.01%
    */
    function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
        replenishmentIncentiveBps = _replenishmentIncentiveBps;
    }

    /**
    @notice sets the Liquidation Incentive of the market as denoted in basis points.
     The Liquidation Incentive is the percentage paid out to liquidators of a borrower's debt when successfully liquidated.
    @dev Must be set between 0 and 10000 - liquidation fee.
    @param _liquidationIncentiveBps The new liqudation incentive set in basis points. 1 = 0.01% 
    */
    function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
        liquidationIncentiveBps = _liquidationIncentiveBps;
    }

    /**
    @notice sets the Liquidation Fee of the market as denoted in basis points.
     The Liquidation Fee is the percentage paid out to governance of a borrower's debt when successfully liquidated.
    @dev Must be set between 0 and 10000 - liquidation factor.
    @param _liquidationFeeBps The new liquidation fee set in basis points. 1 = 0.01%
    */
    function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
        liquidationFeeBps = _liquidationFeeBps;
    }
```

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L212-L219

```solidity
    function pauseBorrows(bool _value) public {
        if(_value) {
            require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
        } else {
            require(msg.sender == gov, "Only governance can unpause");
        }
        borrowPaused = _value;
    }
```

### [L-02] Immutable addresses should be 0-checked
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L80
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L82

It would be good to add more validations for the immutable addresses because they can't be changed once after the contract is deployed.

### [L-03] Possible admin attack vectors
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61

```solidity
    function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

Currently, it's possible to change the token price by operators and they can liquidate any active positions by setting a low collateral price.

### [N-01] Use two-phase ownership transfers
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130

```solidity
    function setGov(address _gov) public onlyGov { gov = _gov; }
```

### [N-02] Open TODOs
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L35

```solidity
    constructor(IXINV _xINV) {
        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
    }
```