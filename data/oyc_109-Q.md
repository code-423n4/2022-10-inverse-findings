## [L-01] Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

```
2022-10-inverse/src/BorrowController.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/DBR.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Fed.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Market.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/Oracle.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/escrows/GovTokenEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/escrows/INVEscrow.sol::2 => pragma solidity ^0.8.13;
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::2 => pragma solidity ^0.8.13;
```

## [L-02] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
2022-10-inverse/src/DBR.sol::122 => uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
2022-10-inverse/src/DBR.sol::135 => uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
2022-10-inverse/src/DBR.sol::148 => uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
2022-10-inverse/src/DBR.sol::224 => require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
2022-10-inverse/src/DBR.sol::286 => if(lastUpdated[user] == block.timestamp) return;
2022-10-inverse/src/DBR.sol::287 => uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;
2022-10-inverse/src/DBR.sol::290 => lastUpdated[user] = block.timestamp;
2022-10-inverse/src/Market.sol::423 => require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
2022-10-inverse/src/Market.sol::487 => require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
2022-10-inverse/src/Oracle.sol::89 => uint day = block.timestamp / 1 days;
2022-10-inverse/src/Oracle.sol::124 => uint day = block.timestamp / 1 days;
```

## [L-03] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
2022-10-inverse/src/escrows/INVEscrow.sol::35 => xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
```

## [L-04] decimals() not part of ERC20 standard

decimals() is not part of the official ERC20 standard and might fail for tokens that do not implement it. While in practice it is very unlikely, as usually most of the tokens implement it, this should still be considered as a potential issue.

```
2022-10-inverse/src/Oracle.sol::85 => uint8 feedDecimals = feeds[token].feed.decimals();
2022-10-inverse/src/Oracle.sol::119 => uint8 feedDecimals = feeds[token].feed.decimals();
```

## [L-05] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

approve is subject to a known front-running attack. Consider using safeApprove() or safeIncreaseAllowance() or safeDecreaseAllowance() instead

```
2022-10-inverse/src/escrows/INVEscrow.sol::50 => _token.approve(address(xINV), type(uint).max);
```

## [L-06] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-10-inverse/src/Fed.sol::135 => dola.transfer(gov, profit);
2022-10-inverse/src/Market.sol::205 => dola.transfer(msg.sender, amount);
2022-10-inverse/src/Market.sol::280 => collateral.transferFrom(msg.sender, address(escrow), amount);
2022-10-inverse/src/Market.sol::399 => dola.transfer(to, amount);
2022-10-inverse/src/Market.sol::537 => dola.transferFrom(msg.sender, address(this), amount);
2022-10-inverse/src/Market.sol::570 => dola.transfer(msg.sender, replenisherReward);
2022-10-inverse/src/Market.sol::602 => dola.transferFrom(msg.sender, address(this), repaidDebt);
2022-10-inverse/src/escrows/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
2022-10-inverse/src/escrows/INVEscrow.sol::63 => token.transfer(recipient, amount);
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::38 => token.transfer(recipient, amount);
```

## [L-08] Events not emitted for important state changes

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

```
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/DBR.sol::53 => function setPendingOperator(address newOperator_) public onlyOperator {
2022-10-inverse/src/DBR.sol::62 => function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
2022-10-inverse/src/Market.sol::118 => function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
2022-10-inverse/src/Market.sol::124 => function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
2022-10-inverse/src/Market.sol::130 => function setGov(address _gov) public onlyGov { gov = _gov; }
2022-10-inverse/src/Market.sol::136 => function setLender(address _lender) public onlyGov { lender = _lender; }
2022-10-inverse/src/Market.sol::142 => function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
2022-10-inverse/src/Market.sol::149 => function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::161 => function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::172 => function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::183 => function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::194 => function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

## [L-09] ecrecover() not checked for signer address of zero

The ecrecover() function returns an address of zero when the signature does not match. This can cause problems if address zero is ever the owner of assets, and someone uses the permit function on address zero. If that happens, any invalid signature will pass the checks, and the assets will be stealable. In this case, the asset of concern is the vault's ERC20 token, and fortunately OpenZeppelin's implementation does a good job of making sure that address zero is never able to have a positive balance. If this contract ever changes to another ERC20 implementation that is laxer in its checks in favor of saving gas, this code may become a problem.

```
2022-10-inverse/src/DBR.sol::226 => address recoveredAddress = ecrecover(
2022-10-inverse/src/Market.sol::425 => address recoveredAddress = ecrecover(
2022-10-inverse/src/Market.sol::489 => address recoveredAddress = ecrecover(
```

## [L-19] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-10-inverse/src/DBR.sol::333 => _mint(user, amount);
2022-10-inverse/src/DBR.sol::351 => _mint(to, amount);
2022-10-inverse/src/DBR.sol::359 => function _mint(address to, uint256 amount) internal virtual {
```

## [N-1] require()/revert() statements should have descriptive reason strings

Descriptive reason strings should be used so that users can troubleshot any reverted calls

```
2022-10-inverse/src/Fed.sol::93 => require(globalSupply <= supplyCeiling);
2022-10-inverse/src/Oracle.sol::104 => revert("Price not found");
2022-10-inverse/src/Oracle.sol::143 => revert("Price not found");
2022-10-inverse/src/escrows/GovTokenEscrow.sol::67 => require(msg.sender == beneficiary);
2022-10-inverse/src/escrows/INVEscrow.sol::91 => require(msg.sender == beneficiary);
```

## [N-2] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public.

```
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/BorrowController.sol::32 => function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
2022-10-inverse/src/BorrowController.sol::38 => function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
2022-10-inverse/src/BorrowController.sol::46 => function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
2022-10-inverse/src/DBR.sol::53 => function setPendingOperator(address newOperator_) public onlyOperator {
2022-10-inverse/src/DBR.sol::62 => function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
2022-10-inverse/src/DBR.sol::70 => function claimOperator() public {
2022-10-inverse/src/DBR.sol::81 => function addMinter(address minter_) public onlyOperator {
2022-10-inverse/src/DBR.sol::90 => function removeMinter(address minter_) public onlyOperator {
2022-10-inverse/src/DBR.sol::99 => function addMarket(address market_) public onlyOperator {
2022-10-inverse/src/DBR.sol::109 => function totalSupply() public view returns (uint) {
2022-10-inverse/src/DBR.sol::146 => function signedBalanceOf(address user) public view returns (int) {
2022-10-inverse/src/DBR.sol::158 => function approve(address spender, uint256 amount) public virtual returns (bool) {
2022-10-inverse/src/DBR.sol::170 => function transfer(address to, uint256 amount) public virtual returns (bool) {
2022-10-inverse/src/DBR.sol::258 => function invalidateNonce() public {
2022-10-inverse/src/DBR.sol::300 => function onBorrow(address user, uint additionalDebt) public {
2022-10-inverse/src/DBR.sol::313 => function onRepay(address user, uint repaidDebt) public {
2022-10-inverse/src/DBR.sol::325 => function onForceReplenish(address user, uint amount) public {
2022-10-inverse/src/Fed.sol::48 => function changeGov(address _gov) public {
2022-10-inverse/src/Fed.sol::57 => function changeSupplyCeiling(uint _supplyCeiling) public {
2022-10-inverse/src/Fed.sol::66 => function changeChair(address _chair) public {
2022-10-inverse/src/Fed.sol::75 => function resign() public {
2022-10-inverse/src/Fed.sol::86 => function expansion(IMarket market, uint amount) public {
2022-10-inverse/src/Fed.sol::103 => function contraction(IMarket market, uint amount) public {
2022-10-inverse/src/Fed.sol::131 => function takeProfit(IMarket market) public {
2022-10-inverse/src/Market.sol::118 => function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
2022-10-inverse/src/Market.sol::124 => function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
2022-10-inverse/src/Market.sol::130 => function setGov(address _gov) public onlyGov { gov = _gov; }
2022-10-inverse/src/Market.sol::136 => function setLender(address _lender) public onlyGov { lender = _lender; }
2022-10-inverse/src/Market.sol::142 => function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
2022-10-inverse/src/Market.sol::149 => function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::161 => function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::172 => function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::183 => function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::194 => function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
2022-10-inverse/src/Market.sol::203 => function recall(uint amount) public {
2022-10-inverse/src/Market.sol::212 => function pauseBorrows(bool _value) public {
2022-10-inverse/src/Market.sol::267 => function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {
2022-10-inverse/src/Market.sol::370 => function getWithdrawalLimit(address user) public view returns (uint) {
2022-10-inverse/src/Market.sol::422 => function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
2022-10-inverse/src/Market.sol::486 => function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
2022-10-inverse/src/Market.sol::520 => function invalidateNonce() public {
2022-10-inverse/src/Market.sol::546 => function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {
2022-10-inverse/src/Market.sol::559 => function forceReplenish(address user, uint amount) public {
2022-10-inverse/src/Market.sol::578 => function getLiquidatableDebt(address user) public view returns (uint) {
2022-10-inverse/src/Market.sol::591 => function liquidate(address user, uint repaidDebt) public {
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
2022-10-inverse/src/Oracle.sol::66 => function claimOperator() public {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::30 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::43 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::52 => function balance() public view returns (uint) {
2022-10-inverse/src/escrows/GovTokenEscrow.sol::57 => function onDeposit() public {
2022-10-inverse/src/escrows/INVEscrow.sol::44 => function initialize(IERC20 _token, address _beneficiary) public {
2022-10-inverse/src/escrows/INVEscrow.sol::59 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/escrows/INVEscrow.sol::70 => function balance() public view returns (uint) {
2022-10-inverse/src/escrows/INVEscrow.sol::79 => function onDeposit() public {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::25 => function initialize(IERC20 _token, address) public {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::36 => function pay(address recipient, uint amount) public {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::45 => function balance() public view returns (uint) {
2022-10-inverse/src/escrows/SimpleERC20Escrow.sol::50 => function onDeposit() public {
```

## [N-3] Missing event for critical parameter change

Emitting events after sensitive changes take place, to facilitate tracking and notify off-chain clients following changes to the contract.

```
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/DBR.sol::53 => function setPendingOperator(address newOperator_) public onlyOperator {
2022-10-inverse/src/DBR.sol::62 => function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
2022-10-inverse/src/Market.sol::118 => function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
2022-10-inverse/src/Market.sol::124 => function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
2022-10-inverse/src/Market.sol::130 => function setGov(address _gov) public onlyGov { gov = _gov; }
2022-10-inverse/src/Market.sol::136 => function setLender(address _lender) public onlyGov { lender = _lender; }
2022-10-inverse/src/Market.sol::142 => function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
2022-10-inverse/src/Market.sol::149 => function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::161 => function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
2022-10-inverse/src/Market.sol::172 => function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::183 => function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
2022-10-inverse/src/Market.sol::194 => function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

## [N-4] Consider addings checks for signature malleability

Use OpenZeppelin's ECDSA contract rather than calling ecrecover() directly

```
2022-10-inverse/src/DBR.sol::226 => address recoveredAddress = ecrecover(
2022-10-inverse/src/Market.sol::425 => address recoveredAddress = ecrecover(
2022-10-inverse/src/Market.sol::489 => address recoveredAddress = ecrecover(
```

## [N-5] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

```
2022-10-inverse/src/Fed.sol::99 => @dev Markets can have more DOLA in them than they've been supplied, due to force replenishes. This call will revert if trying to contract more than have been supplied.
2022-10-inverse/src/Oracle.sol::12 => The Pessimistic Oracle introduces collateral factor into the pricing formula. It ensures that any given oracle price is dampened to prevent borrowers from borrowing more than the lowest recorded value of their collateral over the past 2 days.
2022-10-inverse/src/escrows/GovTokenEscrow.sol::16 => This specific escrow is meant as an example of how an escrow can be implemented that allows depositors to delegate votes with their collateral, unlike pooled deposit protocols.
```
