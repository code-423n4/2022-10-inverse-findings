## QA Report - low risk

### Missing checks for address(0x0) when assigning values to address state variables
___
Check for address(0x0) is missing for `operator`:

[BorrowController.sol: L14](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L14)
```solidity
        operator = _operator;
```
Also [DBR.sol: L39](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L39) and [Oracle.sol: L32](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L32)
___
Check for address(0x0) is missing for `gov`:

[Fed.sol: L39](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L39)
```solidity
        gov = _gov;
```
Also [Fed.sol: L50](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L50) and [Market.sol: L77](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L77)
___
Check for address(0x0) is missing for `chair`:

[Fed.sol: L40](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L40)
```solidity
        chair = _chair;
```
Also [Fed.sol: L68](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L68)
___
Check for address(0x0) is missing for `beneficiary`:

[GovTokenEscrow.sol: L34](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L34)
```solidity
        beneficiary = _beneficiary;
```
Also [INVEscrow.sol: L48](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L48)
___
Checks for address(0x0) are also missing for `lender`, `pauseGuardian`, and `escrowImplementation`:

[Market.sol: L78-80](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L78-L80)
```solidity
        lender = _lender;
        pauseGuardian = _pauseGuardian;
        escrowImplementation = _escrowImplementation;
```
___
___

## QA Report - non-critical

### Typos
___
[DBR.sol: L60](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L60)
```solidity
    @param newReplenishmentPriceBps_ The new replen
```
Change `replen` to `replenishment price` or whatever was intended
___
[Market.sol: L157](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L157)
```solidity
     At 5000, 50% of of a borrower's underwater debt can be liquidated. Only callable by governance.
```
Remove repeated word `of`
___
[Market.sol: L201](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L201)
```solidity
    @param amount The amount od DOLA to recall to the the lender.
```
Remove repeated word `the`
___
[Market.sol: L589](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L589)
```solidity
    @param repaidDebt Th amount of user user debt to liquidate.
```
Change `Th` to `The` and remove repeated word `user`
___
[BorrowController.sol: L36](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L36)
```solidity
    @param deniedContract The addres of the denied contract
```
Change `addres` to `address`
___
[DBR.sol: L50](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L50)
```solidity
    @notice Sets pending operator of the contract. Operator role must be claimed by the new oprator. Only callable by Operator.
```
Change `oprator` to `operator`
___
[DBR.sol: L87](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L87)
```solidity
    @notice Removes a minter from the set of addresses allowe to mint DBR tokens. Only callable by Operator.
```
Change `allowe` to `allowed`
___
[DBR.sol: L128](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L128)
```solidity
    @notice Get the DBR deficit of an address. Will return 0 if th user has zero DBR or more.
```
Change `th` to `the`
___
[Market.sol: L146](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L146)
```solidity
    @dev Collateral factor mus be set below 100%
```
Change `mus` to `must`
___
[Market.sol: L181](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L181)
```solidity
    @param _liquidationIncentiveBps The new liqudation incentive set in basis points. 1 = 0.01% 
```
Change `liqudation` to `liquidation`
___
[Market.sol: L575](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L575)
```solidity
    @notice View function for getting the amount of liquidateable debt a user holds.
```
Change `liquidateable` to `liquidatable`
___
___

### Long single-line comments 
In theory, comments over 80 characters should wrap using multi-line comment syntax. Even if somewhat longer comments (up to 120 characters) are acceptable and a scroll bar is provided, there are cases where very long comments interfere with readability. Below are five instances of extra-long comments (or groups of comments) whose readability could be improved, as shown:
___
[Fed.sol: L99](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L99)
```solidity
    @dev Markets can have more DOLA in them than they've been supplied, due to force replenishes. This call will revert if trying to contract more than have been supplied.
```
Suggestion:
```solidity
    @dev Markets can have more DOLA in them than they've been supplied, due to force replenishes. 
     This call will revert if trying to contract more than have been supplied.
```
___
[Oracle.sol: L9-15](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L9-L15)
```solidity
/**
@title Oracle
@notice Oracle used by markets. Can use both fixed price feeds and Chainlink-style feeds for prices.
The Pessimistic Oracle introduces collateral factor into the pricing formula. It ensures that any given oracle price is dampened to prevent borrowers from borrowing more than the lowest recorded value of their collateral over the past 2 days.
This has the advantage of making price manipulation attacks more difficult, as an attacker needs to log artificially high lows.
It has the disadvantage of reducing borrow power of borrowers to a 2-day minimum value of their collateral, where the value must have been seen by the oracle.
*/
```
Suggestion:
```solidity
/**
@title Oracle
@notice Oracle used by markets. Can use both fixed price feeds and Chainlink-style feeds for prices.
 The Pessimistic Oracle introduces collateral factor into the pricing formula. 
 It ensures that any given oracle price is dampened to prevent borrowers from
 borrowing more than the lowest recorded value of their collateral over the past 2 days. 
 This has the advantage of making price manipulation attacks more difficult,
 as an attacker needs to log artificially high lows. 
 It has the disadvantage of reducing borrow power of borrowers to a 2-day minimum
 value of their collateral, where the value must have been seen by the oracle.
*/
```
___
[GovTokenEscrow.sol: L16](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L16)
```solidity
 This specific escrow is meant as an example of how an escrow can be implemented that allows depositors to delegate votes with their collateral, unlike pooled deposit protocols.
```
Suggestion:
```solidity
 This specific escrow is meant as an example of how an escrow can be implemented that
 allows depositors to delegate votes with their collateral, unlike pooled deposit protocols.
```
___
[INVEscrow.sol: L25](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L25)
```solidity
 This escrow allows user to deposit INV collateral directly into the xINV contract, earning APY and allowing them to delegate votes on behalf of the xINV collateral
```
Suggestion:
```solidity
 This escrow allows user to deposit INV collateral directly into the xINV contract,
 earning APY and allowing them to delegate votes on behalf of the xINV collateral.
```
___
[INVEscrow.sol: L62](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L62)
```solidity
        if(invBalance < amount) xINV.redeemUnderlying(amount - invBalance); // we do not check return value because next call will fail if this fails anyway
```
Suggestion:
```solidity
        // We do not check return value below because next call will fail if this fails anyway
        if(invBalance < amount) xINV.redeemUnderlying(amount - invBalance);
```
___
___

### Open TODOs and other open items
Comments that appear to refer to open items should be addressed.
___
[GovTokenEscrow.sol: L56](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L56)
```solidity
    /* Uncomment if Escrow contract should handle on deposit callbacks. This function should remain callable by anyone to handle direct inbound transfers.
```
Similarly for [SimpleERC20Escrow.sol: L49](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/SimpleERC20Escrow.sol#L49)
___
[INVEscrow.sol: L35](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L35)
```solidity
        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```
___
___

### Missing NatSpec
NatSpec statements are entirely missing for all `constructors` and the four `functions` referenced below:

[DBR.sol: L262-264](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L262-L264)
```solidity
    function DOMAIN_SEPARATOR() public view virtual returns (bytes32) {
        return block.chainid == INITIAL_CHAIN_ID ? INITIAL_DOMAIN_SEPARATOR : computeDomainSeparator();
    }
```
Similary, [Market.sol: L97-99](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L97-L99)
___
[DBR.sol: L266-277](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L266-L277)
```solidity
    function computeDomainSeparator() internal view virtual returns (bytes32) {
        return
            keccak256(
                abi.encode(
                    keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
                    keccak256(bytes(name)),
                    keccak256("1"),
                    block.chainid,
                    address(this)
                )
            );
    }
```
Similary, [Market.sol: L101-112](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L101-L112)
___
___

### Missing individual NatSpec statements
The following `functions` are missing `@return`statements
___
[Market.sol: L221-226](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L221-L226)
```solidity
    /**
    @notice Internal function for creating an escrow for users to deposit collateral in.
    @dev Uses create2 and minimal proxies to create the escrow at a deterministic address
    @param user The address of the user to create an escrow for.
    */
    function createEscrow(address user) internal returns (IEscrow instance) {
```
Missing: `@return`

Similarly for the following functions:

[Market.sol: L240-245](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L240-L245)

[Market.sol: L287-292](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L287-L292)

[Market.sol: L308-312](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L308-L312)

[Market.sol: L318-323](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L318-L323)

[Market.sol: L329-334](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L329-L334)

[Market.sol: L339-344](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L339-L344)

[Market.sol: L348-353](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L348-L353)

[Market.sol: L365-370](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L365-L370)

[Market.sol: L574-578](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L574-L578)

The following `functions` are missing `@param`statements

[Oracle.sol: L73-78](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L73-L78)
```solidity
    /**
    @notice Gets the price of a specific token in DOLA
    @param token The address of the token to get price of
    @return The price of the token in DOLA, adjusted for token and feed decimals
    */
    function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
```
Missing: `@param collateralFactorBps`

Similarly for [Oracle.sol: L107-112](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L107-L112)
___
___

### Missing `require` revert string
A `require` message should be included to enable users to understand the reason for failure.

Recommendation: Add a brief (<= 32 char) explanatory message to each of the `requires` referenced below. Consider using Custom Error codes (which would save both deployment and runtime cost and therefore be cheaper than revert strings).

___
[Fed.sol: L93](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L93)
```solidity
        require(globalSupply <= supplyCeiling);
```
___
[GovTokenEscrow.sol: L67](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/GovTokenEscrow.sol#L67)
```solidity
        require(msg.sender == beneficiary);
```
Similarly for [INVEscrow.sol: L91](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L91)
___
___

### Event is missing `indexed` fields
Each `event` referenced below should use three `indexed` fields if there are at least three fields

___
[DBR.sol: L381-382](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L381-L382)
```solidity
    event Transfer(address indexed from, address indexed to, uint256 amount);
    event Approval(address indexed owner, address indexed spender, uint256 amount);
```
___
[Market.sol: L616-619](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L616-L619)
```solidity
    event Withdraw(address indexed account, address indexed to, uint amount);
    event Repay(address indexed account, address indexed repayer, uint amount);
    event ForceReplenish(address indexed account, address indexed replenisher, uint deficit, uint replenishmentCost, uint replenisherReward);
    event Liquidate(address indexed account, address indexed liquidator, uint repaidDebt, uint liquidatorReward);
```
___
___
