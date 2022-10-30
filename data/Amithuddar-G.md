1)X = X + Y OR X = X - Y CAN BE USED INSTEAD OF X += Y OR X -= Y

x = x + y or x = x - y costs less gas than x += y or x -= y. For example, v += 27 can be changed to v = v + 27 in the following code.

File: 2022-10-inverse\src\DBR.sol
  174,26:             balances[to] += amount;
  198,26:             balances[to] += amount;
  288,32:         dueTokensAccrued[user] += accrued;
  289,31:         totalDueTokensAccrued += accrued;
  304,21:         debts[user] += additionalDebt;
  332,21:         debts[user] += replenishmentCost;
  360,22:         _totalSupply += amount;
  362,26:             balances[to] += amount;
  
File: 2022-10-inverse\src\DBR.sol
  172,30:         balances[msg.sender] -= amount;
  196,24:         balances[from] -= amount;
  316,21:         debts[user] -= repaidDebt;
  374,24:         balances[from] -= amount;
  376,26:             _totalSupply -= amount;

File: 2022-10-inverse\src\Fed.sol
  91,26:         supplies[market] += amount;
  92,22:         globalSupply += amount;

File: 2022-10-inverse\src\Fed.sol
  110,26:         supplies[market] -= amount;
  111,22:         globalSupply -= amount;  

File: 2022-10-inverse\src\Market.sol
  534,21:         debts[user] -= amount;
  535,19:         totalDebt -= amount;
  599,21:         debts[user] -= repaidDebt;
  600,19:         totalDebt -= repaidDebt;
  
File: 2022-10-inverse\src\Market.sol
  395,25:         debts[borrower] += amount;
  397,19:         totalDebt += amount;
  565,21:         debts[user] += replenishmentCost;
  568,19:         totalDebt += replenishmentCost;
  598,26:         liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

2)Splitting require() statements that use && saves gas
  

File: 2022-10-inverse\src\DBR.sol
  249,52:             require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

File: 2022-10-inverse\src\Market.sol
  75,46:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
  162,43:         require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
  173,48:         require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
  184,46:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
  195,40:         require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");
  448,52:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
  512,52:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

3)STATE VARIABLES CAN BE PACKED INTO FEWER STORAGE SLOTS
If variables occupying the same slot are both written the same function or by the constructor,
avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper


File: 2022-10-inverse\src\DBR.sol
  13,5:     uint8 public constant decimals = 18;
  220,9:         uint8 v,


216     address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s  
		  
4)++x is more efficient than x++


File: 2022-10-inverse\src\DBR.sol
  239,46:                                 nonces[owner]++,
  259,27:         nonces[msg.sender]++;
  
File: 2022-10-inverse\src\Market.sol
  438,45:                                 nonces[from]++,
  502,45:                                 nonces[from]++,
  521,27:         nonces[msg.sender]++;    
 
5)Multiple address mappings can be combined into a single mapping of an address to a struct

Multiple address mappings can be combined into a single mapping of an address to a struct,
where appropriate saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

 
File: 2022-10-inverse\src\DBR.sol
  19,5:     mapping(address => uint256) public balances;
  20,5:     mapping(address => mapping(address => uint256)) public allowance;
  20,24:     mapping(address => mapping(address => uint256)) public allowance;
  23,5:     mapping(address => uint256) public nonces;
  24,5:     mapping (address => bool) public minters;
  25,5:     mapping (address => bool) public markets;
  26,5:     mapping (address => uint) public debts; // user => debt across all tracked markets
  27,5:     mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
  28,5:     mapping (address => uint) public lastUpdated; // user => last update timestamp

File: 2022-10-inverse\src\Market.sol
  57,5:     mapping (address => IEscrow) public escrows; // user => escrow
  58,5:     mapping (address => uint) public debts; // user => debt
  59,5:     mapping(address => uint256) public nonces; // user => nonce

File: 2022-10-inverse\src\Oracle.sol
  25,5:     mapping (address => FeedData) public feeds;
  26,5:     mapping (address => uint) public fixedPrices;
  27,5:     mapping (address => mapping(uint => uint)) public dailyLows; // token => day => price
  27,25:     mapping (address => mapping(uint => uint)) public dailyLows; // token => day => price 

6)Use assembly to check for address(0)
Impact

Saves 6 gas per instance if using assembly to check for zero address  

File: 2022-10-inverse\src\DBR.sol
  73,27:         pendingOperator = address(0);
  249,41:             require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
  291,29:         emit Transfer(user, address(0), accrued);
  364,23:         emit Transfer(address(0), to, amount);
  378,29:         emit Transfer(from, address(0), amount);

File: 2022-10-inverse\src\Market.sol
  236,37:         require(instance != IEscrow(address(0)), "ERC1167: create2 failed");
  246,37:         if(escrows[user] != IEscrow(address(0))) return escrows[user];
  391,50:         if(borrowController != IBorrowController(address(0))) {
  448,41:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
  512,41:             require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

File: 2022-10-inverse\src\Oracle.sol
  69,27:         pendingOperator = address(0);
  80,48:         if(feeds[token].feed != IChainlinkFeed(address(0))) {
  114,48:         if(feeds[token].feed != IChainlinkFeed(address(0))) {

File: 2022-10-inverse\src\escrows\GovTokenEscrow.sol
  31,27:         require(market == address(0), "ALREADY INITIALIZED");

File: 2022-10-inverse\src\escrows\INVEscrow.sol
  45,27:         require(market == address(0), "ALREADY INITIALIZED");

File: 2022-10-inverse\src\escrows\SimpleERC20Escrow.sol
  26,27:         require(market == address(0), "ALREADY INITIALIZED");

  