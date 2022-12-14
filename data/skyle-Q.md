## Use a 2-step ownership transform pattern
It's possible that the gov role mistakenly transfer ownership to a wrong addresss, resulting a loss of the gov role. Consider first nominate an address as the pendingGov and acceptOwnership function is called by the pendingGov to confirm the gov role transfer

### Fed.sol
Fed.sol:48 changeGov(address _gov) public

## A magic number should be documented and explained use a constant instead
### market.sol
Market.sol:150 require(_collateralFactorBps < 10000, "Invalid collateral factor");
Market.sol:162 require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol:195
Market.sol:336
Market.sol:346
Market.sol:377
Market.sol:563
Market.sol:564
Market.sol:583
Market.sol:598
Market.sol:606

## Use a const instead of duplicating the same string
### 
Oracle.sol:83
Oracle.sol:104
Oracle.sol:117
Oracle.sol:143
Market.sol:74
Market.sol:150
Market.sol:448
Market.sol:512
Fed.sol:49
Fed.sol:58
Fed.sol:67

## inconsistent error message
The naming convention of the error message is not consistent for  different contracts. Some contracts use all upper latter error messages, while others use Pascal Case.
Market.sol:392 require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
Market.sol:487 require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
Market.sol:512 require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
Fed.sol:88 require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
Fed.sol:89 require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");

## Open TODOs
###lines
INVEscrow.sol:35

## missing event for admin function like changing gov and chair
Fed.sol the function changeGov and ChangeChair are missing events for admin functionality. Uses can't monitor changes done to the contract 
###lines
Fed.sol:48
Fed.sol:66

## missing check for return values
 Market.recall(uint256) (src/Market.sol#203-206) ignores return value by dola.transfer(msg.sender,amount) (src/Market.sol#205)
Market.deposit(address,uint256) (src/Market.sol#278-285) ignores return value by collateral.transferFrom(msg.sender,address(escrow),amount) (src/Market.sol#280)
Market.borrowInternal(address,address,uint256) (src/Market.sol#389-401) ignores return value by dola.transfer(to,amount) (src/Market.sol#399)
Market.repay(address,uint256) (src/Market.sol#531-539) ignores return value by dola.transferFrom(msg.sender,address(this),amount) (src/Market.sol#537)
Market.forceReplenish(address,uint256) (src/Market.sol#559-572) ignores return value by dola.transfer(msg.sender,replenisherReward) (src/Market.sol#570)
Market.liquidate(address,uint256) (src/Market.sol#591-612) ignores return value by dola.transferFrom(msg.sender,address(this),repaidDebt) (src/Market.sol#602)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#unchecked-transfer