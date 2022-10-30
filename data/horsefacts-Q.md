# QA

## Low

### `DBR`: Underflow for very large balances in `DBR#signedBalanceOf`

A comment on `DBR#signedBalanceOf` notes that the function "will revert if a user has a balance of more than 2^255-1 DBR", but it's possible for this function to underflow rather than revert for very large balances:

[`DBR#signedBalanceOf`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L146-L150)

```solidity
/**
 @notice Get the signed DBR balance of an address.
 @dev This function will revert if a user has a balance of more than 2^255-1 DBR
 @param user Address of the user.
 @return Returns a signed int of the user's balance
 */
function signedBalanceOf(address user) public view returns (int) {
  uint debt = debts[user];
  uint accrued = ((block.timestamp - lastUpdated[user]) * debt) / 365 days;
  return int(balances[user]) - int(dueTokensAccrued[user]) - int(accrued);
}

```

See the unit test below for an example:

```solidity
function test_signedBalanceOf_underflow() public {
  vm.startPrank(operator);
  dbr.mint(user, 2**255 + 1);
  vm.stopPrank();

  // Zero deficit
  assertEq(dbr.deficitOf(user), 0);
  // Positive balance
  assertEq(
    dbr.balanceOf(user),
    57896044618658097711785492504343953926634992332820282019728792003956564819969
  );
  // Negative signed balance
  assertEq(
    dbr.signedBalanceOf(user),
    -57896044618658097711785492504343953926634992332820282019728792003956564819967
  );
}

```

This is an extremely unlikely situation in the course of normal operation, and even so, there is limited impact since the signed balance is not used internally for any calculations. However, consider using a safe conversion helper that reverts on attempts to convert unsigned integers greater than `type(int256).max`.

### `Fed`: Use two-step transfers for `Fed` governance

Transferring ownership over the `Fed` contract is performed in a single step.

[`Fed`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L44-L52)

```solidity
/**
    @notice Change the governance of the Fed contact. Only callable by governance.
    @param _gov The address of the new governance contract
    */
function changeGov(address _gov) public {
  require(msg.sender == gov, "ONLY GOV");
  gov = _gov;
}

```

If ownership is accidentally transferred to an incorrect address, it would be possible for governance to lose access to the contract. Consider using a two-step ownership transfer similar to that in `Oracle`.

### `Market`: Markets cannot support tokens with missing return values

The `IERC20` interface used in `Market` notes that callers should assume all failed transfers revert:

[`IERC20`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L4-L9):

```solidity
// Caution. We assume all failed transfers cause reverts and ignore the returned bool.
interface IERC20 {
  function transfer(address, uint) external returns (bool);

  function transferFrom(address, address, uint) external returns (bool);

  function balanceOf(address) external view returns (uint);
}

```

However, it does not account for another nonstandard ERC20 scenario: tokens with missing return values. Tokens like `USDT`, `BNB`, and `OMG` have [missing return values](https://github.com/d-xo/weird-erc20#missing-return-values) and transfers will revert if they are used as collateral for a market.

I consider this low risk since allowed tokens require approval by governance, but ensure you do due diligence on the token contracts for any new collateral types before adding them. Alternatively, use a safe transfer helper to abstract over inconsistend ERC20 implementations.

### `Market`: Governance can frontrun liquidations to change incentives

Since governance has control over liquidation incentive BPS and liquidation fee BPS parameters, and liquidations read these parameters at the time of liquidation, it's possible for governance to observe a pending liquidation transaction, frontrun it with a change to the fee parameters, and reduce the liquidation incentive/increase the protocol fee.

[`Market#setLiquidationIncentiveBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L183-L186)

```solidity
function setLiquidationIncentiveBps(uint _liquidationIncentiveBps)
  public
  onlyGov
{
  require(
    _liquidationIncentiveBps > 0 &&
      _liquidationIncentiveBps + liquidationFeeBps < 10000,
    "Invalid liquidation incentive"
  );
  liquidationIncentiveBps = _liquidationIncentiveBps;
}

```

[`Market#setLiquidationFeeBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L194-L197)

```solidity
function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
  require(
    _liquidationFeeBps > 0 &&
      _liquidationFeeBps + liquidationIncentiveBps < 10000,
    "Invalid liquidation fee"
  );
  liquidationFeeBps = _liquidationFeeBps;
}

```

I consider this low risk since it requires malicious governance, but consider enforcing a timelock on changes to these parameters.

### `Oracle`: Price calculations may revert for tokens with more than 18 decimals

The price oracle performs a calculation to normalize the decimals of a price feed/token pair and return the value of `1e18` wei of the feed's token in DOLA. Currently, Chainlink token/USD price feeds use 8 decimals, and token/ETH price feeds use 18 decimals.

[`Oracle#viewPrice`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L78-L106)

```solidity
    // normalize price
    uint8 feedDecimals = feeds[token].feed.decimals();
    uint8 tokenDecimals = feeds[token].tokenDecimals;
    uint8 decimals = 36 - feedDecimals - tokenDecimals;
    uint normalizedPrice = price * (10**decimals);

```

It's possible for `36 - feedDecimals - tokenDecimals` to revert for tokens with more than 18 decimals: an 8 decimal price feed can support tokens up to 28 decimals, and an 18 decimal feed can only support tokens with 18 or fewer decimals.

I consider this low risk, since tokens must be approved, you are unlikely to use 18 decimal price feeds, and 28+ decimal tokens are rare, but be aware of this possibility.

(By the way, note that the `EthFeed` used in the project test suite uses 18 decimals, but the real world Chainlink ETH/USD price feed uses 8. It is probably more representative of reality to use 8 decimals in the tests).

## QA

### Emit events from permissioned functions

Throughout the codebase, there are a number of authenticated/permissioned functions that change protocol parameters, update authorization, or make other state changes without emitting events. Events are useful both as a hook for your own security monitoring tools, and for users at large to observe the operation of the system. Additionally, they make it significantly easier for third-party monitoring tools to observe your protocol. Consider adding events to the functions listed below:

`BorrowController`:

- [`setOperator`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26)
- [`allow`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32)
- [`deny`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38)

`DBR`:

- [`setPendingOperator`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53)
- [`setReplenishmentPriceBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L62)
- [`onBorrow`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L300)
- [`onRepay`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L313)
- [`onForceReplenish`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L325)

`Fed`:

- [`changeGov`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48)
- [`changeSupplyCeiling`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L57)
- [`changeChair`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L66)
- [`resign`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L75)

`Market`:

- [`setOracle`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L118)
- [`setBorrowController`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L124)
- [`setGov`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130)
- [`setLender`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L136)
- [`setPauseGuardian`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L142)
- [`setCollateralFactorBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L149)
- [`setLiquidationFactorBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L161)
- [`setReplenismentIncentiveBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L172)
- [`setLiquidationIncentiveBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L183)
- [`pauseBorrows`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L212)

`Oracle`:

- [`setPendingOperator`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44)
- [`setFeed`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53)
- [`setFixedPrice`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61)

### Finalize license

The `UNLICENSED` SPDX identifier is an invalid, Solidity-specific SPDX identifier for closed source, all-rights-reserved code. If you intend to release this codebase under an OSS license, make sure you update these IDs.

[`DBR`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L1-L2):

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

```

### Lock version pragma

Files throughout the codebase have a floating version pragma. It's a best practice to fix a specific version before deployment, to ensure contracts are tested with a known version and avoid accidental upgrades to an unexpected version.

[`DBR`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L1-L2):

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

```

### Inconsistent use of `uint` and `uint256`

Both `uint` and `uint256` are used to declare variables in `DBR` and `Market`. Consider using one or the other for consistency.

[`DBR`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L11-L28)

```solidity
    string public name;
    string public symbol;
    uint8 public constant decimals = 18;
    uint256 public _totalSupply;
    address public operator;
    address public pendingOperator;
    uint public totalDueTokensAccrued;
    uint public replenishmentPriceBps;
    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowance;
    uint256 internal immutable INITIAL_CHAIN_ID;
    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
    mapping(address => uint256) public nonces;
    mapping (address => bool) public minters;
    mapping (address => bool) public markets;
    mapping (address => uint) public debts; // user => debt across all tracked markets
    mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
    mapping (address => uint) public lastUpdated; // user => last update timestamp
```

[`Market`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L54-L59)

```solidity
    uint public totalDebt;
    uint256 internal immutable INITIAL_CHAIN_ID;
    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
    mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce
```

### `Market`: Misspelling in `setReplenismentIncentiveBps`

The function [`setReplenismentIncentiveBps`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L172) is misspelled and should be `setReplenishmentIncentiveBps`. (This function is not called by any other contracts and the misspelling has no functional impact, just a typo).