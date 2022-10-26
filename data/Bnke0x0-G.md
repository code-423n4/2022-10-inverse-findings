
### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gsset (20000 gas)
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::20 => address public market;
2022-10-inverse/escrows/GovTokenEscrow.sol::21 => IERC20 public token;
2022-10-inverse/escrows/GovTokenEscrow.sol::22 => address public beneficiary;
2022-10-inverse/escrows/INVEscrow.sol::29 => address public market;
2022-10-inverse/escrows/INVEscrow.sol::30 => IERC20 public token;
2022-10-inverse/escrows/INVEscrow.sol::31 => address public beneficiary;
2022-10-inverse/escrows/SimpleERC20Escrow.sol::17 => address public market;
2022-10-inverse/escrows/SimpleERC20Escrow.sol::18 => IERC20 public token;
2022-10-inverse/src/BorrowController.sol::10 => address public operator;
2022-10-inverse/src/Fed.sol::30 => address public gov;
2022-10-inverse/src/Fed.sol::31 => address public chair;
2022-10-inverse/src/Fed.sol::32 => uint public supplyCeiling;
2022-10-inverse/src/Fed.sol::33 => uint public globalSupply;
2022-10-inverse/src/GovTokenEscrow.sol::20 => address public market;
2022-10-inverse/src/GovTokenEscrow.sol::21 => IERC20 public token;
2022-10-inverse/src/GovTokenEscrow.sol::22 => address public beneficiary;
2022-10-inverse/src/Oracle.sol::23 => address public operator;
2022-10-inverse/src/Oracle.sol::24 => address public pendingOperator;
```



### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::20 => address public market;
2022-10-inverse/escrows/GovTokenEscrow.sol::22 => address public beneficiary;
2022-10-inverse/escrows/INVEscrow.sol::29 => address public market;
2022-10-inverse/escrows/INVEscrow.sol::31 => address public beneficiary;
2022-10-inverse/escrows/SimpleERC20Escrow.sol::17 => address public market;
2022-10-inverse/src/BorrowController.sol::10 => address public operator;
2022-10-inverse/src/Fed.sol::30 => address public gov;
2022-10-inverse/src/Fed.sol::31 => address public chair;
2022-10-inverse/src/GovTokenEscrow.sol::20 => address public market;
2022-10-inverse/src/GovTokenEscrow.sol::22 => address public beneficiary;
2022-10-inverse/src/Oracle.sol::23 => address public operator;
2022-10-inverse/src/Oracle.sol::24 => address public pendingOperator;
```


### [G03] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
2022-10-inverse/escrows/INVEscrow.sol::81 => if(invBalance > 0) {
2022-10-inverse/src/Fed.sol::133 => if(profit > 0) {
2022-10-inverse/src/Oracle.sol::79 => if(fixedPrices[token] > 0) return fixedPrices[token];
2022-10-inverse/src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::96 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
2022-10-inverse/src/Oracle.sol::97 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
2022-10-inverse/src/Oracle.sol::113 => if(fixedPrices[token] > 0) return fixedPrices[token];
2022-10-inverse/src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::135 => uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
2022-10-inverse/src/Oracle.sol::136 => if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
```

### [G04] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function


#### Findings:
```
2022-10-inverse/src/Fed.sol::49 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::58 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::67 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::76 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::87 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::88 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
2022-10-inverse/src/Fed.sol::105 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");


2022-10-inverse/src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::104 => revert("Price not found");
2022-10-inverse/src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::143 => revert("Price not found");
```

### [G05] Use custom errors rather than `revert()`/`require()` strings to save deployment gas

#### Impact
Checks that involve constants should come before checks that involve state variables
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::31 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/escrows/GovTokenEscrow.sol::44 => require(msg.sender == market, "ONLY MARKET");
2022-10-inverse/escrows/INVEscrow.sol::45 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/escrows/INVEscrow.sol::60 => require(msg.sender == market, "ONLY MARKET");
2022-10-inverse/escrows/SimpleERC20Escrow.sol::26 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/escrows/SimpleERC20Escrow.sol::37 => require(msg.sender == market, "ONLY MARKET");
2022-10-inverse/src/BorrowController.sol::18 => require(msg.sender == operator, "Only operator");
2022-10-inverse/src/Fed.sol::49 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::58 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::67 => require(msg.sender == gov, "ONLY GOV");
2022-10-inverse/src/Fed.sol::76 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::87 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::88 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
2022-10-inverse/src/Fed.sol::89 => require(market.borrowPaused() != true, "CANNOT EXPAND PAUSED MARKETS");
2022-10-inverse/src/Fed.sol::104 => require(msg.sender == chair, "ONLY CHAIR");
2022-10-inverse/src/Fed.sol::105 => require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
2022-10-inverse/src/Fed.sol::107 => require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
2022-10-inverse/src/GovTokenEscrow.sol::31 => require(market == address(0), "ALREADY INITIALIZED");
2022-10-inverse/src/GovTokenEscrow.sol::44 => require(msg.sender == market, "ONLY MARKET");
2022-10-inverse/src/Oracle.sol::36 => require(msg.sender == operator, "ONLY OPERATOR");
2022-10-inverse/src/Oracle.sol::67 => require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
2022-10-inverse/src/Oracle.sol::83 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::104 => revert("Price not found");
2022-10-inverse/src/Oracle.sol::117 => require(price > 0, "Invalid feed price");
2022-10-inverse/src/Oracle.sol::143 => revert("Price not found");
```



### [G06] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
2022-10-inverse/src/BorrowController.sol::26 => function setOperator(address _operator) public onlyOperator { operator = _operator; }
2022-10-inverse/src/BorrowController.sol::32 => function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }
2022-10-inverse/src/BorrowController.sol::38 => function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
2022-10-inverse/src/Oracle.sol::44 => function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
2022-10-inverse/src/Oracle.sol::53 => function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
2022-10-inverse/src/Oracle.sol::61 => function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```


### [G07] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done in some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value check 
#### Findings:
```
2022-10-inverse/escrows/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
2022-10-inverse/escrows/INVEscrow.sol::63 => token.transfer(recipient, amount);
2022-10-inverse/escrows/SimpleERC20Escrow.sol::38 => token.transfer(recipient, amount);
2022-10-inverse/src/Fed.sol::135 => dola.transfer(gov, profit);
2022-10-inverse/src/GovTokenEscrow.sol::45 => token.transfer(recipient, amount);
```


### [G08] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
2022-10-inverse/src/Oracle.sol::25 => mapping (address => FeedData) public feeds;
2022-10-inverse/src/Oracle.sol::26 => mapping (address => uint) public fixedPrices;
2022-10-inverse/src/Oracle.sol::27 => mapping (address => mapping(uint => uint)) public dailyLows; // token => day => price
```


### [G09] Using `bools` for storage incurs overhead


#### Findings:
```
2022-10-inverse/src/BorrowController.sol::11 => mapping(address => bool) public contractAllowlist;
```

