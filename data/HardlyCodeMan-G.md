## Gas

### Storage Optimisation
Changing order of storage slots 20 000 gas saved at deployment per slot saved. Ethereum storage is composed of slots of 32 bytes, the problem is that writing is expensive. (Up to 20000gas using “cold” writing) https://docs.soliditylang.org/en/v0.8.13/internals/layout_in_storage.html

#### Effected contracts
- DBS.sol
- Market.sol

### DBR.sol
Potential for minor contract storage optimisation in [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L11-L28). Constant decimals is of type uint8 taking up 1 byte of storage is surrounded by a string and uint256 both taking up 32bytes or an entire storage slot each. Constant decimals can be put together with ether address operator or pendingOperator both of size bytes 20 to share a single slot utilizing 21 of 32 bytes rather than having a slot occupying 1 of 32 bytes as is the current case for constant decimals.

#### Original

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

| size    | slot    | type    |
|:-------:|:-------:| -------:|
| 32      | 0       | string  |
| 32      | 1       | string  |
| 1       | 2       | uint8   |
| 32      | 3       | uint256 |
| 20      | 4       | address |
| 20      | 5       | address |
| 32      | 6       | uint    |
| 32      | 7       | uint    |
| 32      | 8       | mapping |
| 32      | 9       | mapping |
| 32      | 10      | uint256 |
| 32      | 11      | bytes32 |
| 32      | 12      | mapping |
| 32      | 13      | mapping |
| 32      | 14      | mapping |
| 32      | 15      | mapping |
| 32      | 16      | mapping |
| 32      | 17      | mapping |

#### Modified

```solidity
    uint8 public constant decimals = 18;
    address public operator;
    address public pendingOperator;
    string public name;
    string public symbol;
    uint256 public _totalSupply;
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

| size    | slot    | type    |
|:-------:|:-------:| -------:|
| 1       | 0       | uint8   |
| 20      | 0       | address |
| 20      | 1       | address |
| 32      | 2       | string  |
| 32      | 3       | string  |
| 32      | 4       | uint256 |
| 32      | 5       | uint    |
| 32      | 6       | uint    |
| 32      | 7       | mapping |
| 32      | 8       | mapping |
| 32      | 9       | uint256 |
| 32      | 10      | bytes32 |
| 32      | 11      | mapping |
| 32      | 12      | mapping |
| 32      | 13      | mapping |
| 32      | 14      | mapping |
| 32      | 15      | mapping |
| 32      | 16      | mapping |

### Market.sol
Potential for minor contract storage optimisation in [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L38-L59). Similar to the above in DBR.sol, Market.sol has 2 bool variables in storage each taking up 1 byte and in the current configuration sharing the 1 storage slot and utilising 2 of 32 bytes of storage. Rearranging the 2 bool variables to fit in another storage slot will save 1 storage slot, in this case the bool variabls can be packed again with an address to take up 22 of 32 bytes of storage in the single slot.

#### Original

```solidity
    address public gov;
    address public lender;
    address public pauseGuardian;
    address public immutable escrowImplementation;
    IDolaBorrowingRights public immutable dbr;
    IBorrowController public borrowController;
    IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
    IERC20 public immutable collateral;
    IOracle public oracle;
    uint public collateralFactorBps;
    uint public replenishmentIncentiveBps;
    uint public liquidationIncentiveBps;
    uint public liquidationFeeBps;
    uint public liquidationFactorBps = 5000; // 50% by default
    bool immutable callOnDepositCallback;
    bool public borrowPaused;
    uint public totalDebt;
    uint256 internal immutable INITIAL_CHAIN_ID;
    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
    mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce

```

| size    | slot    | type    |
|:-------:|:-------:| -------:|
| 20      | 0       | address |
| 20      | 1       | address |
| 20      | 2       | address |
| 20      | 3       | address |
| 20      | 4       | address |
| 20      | 5       | address |
| 20      | 6       | address |
| 20      | 7       | address |
| 20      | 8       | address |
| 32      | 9       | uint    |
| 32      | 10      | uint    |
| 32      | 11      | uint    |
| 32      | 12      | uint    |
| 32      | 13      | uint    |
| 1       | 14      | bool    |
| 1       | 14      | bool    |
| 32      | 15      | uint    |
| 32      | 16      | uint256 |
| 32      | 17      | bytes32 |
| 32      | 18      | mapping |
| 32      | 19      | mapping |
| 32      | 20      | mapping |

#### Modified

```solidity
    bool immutable callOnDepositCallback;
    bool public borrowPaused;
    address public gov;
    address public lender;
    address public pauseGuardian;
    address public immutable escrowImplementation;
    IDolaBorrowingRights public immutable dbr;
    IBorrowController public borrowController;
    IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
    IERC20 public immutable collateral;
    IOracle public oracle;
    uint public collateralFactorBps;
    uint public replenishmentIncentiveBps;
    uint public liquidationIncentiveBps;
    uint public liquidationFeeBps;
    uint public liquidationFactorBps = 5000; // 50% by default
    uint public totalDebt;
    uint256 internal immutable INITIAL_CHAIN_ID;
    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
    mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce

```

| size    | slot    | type    |
|:-------:|:-------:| -------:|
| 1       | 0       | bool    |
| 1       | 0       | bool    |
| 20      | 0       | address |
| 20      | 1       | address |
| 20      | 2       | address |
| 20      | 3       | address |
| 20      | 4       | address |
| 20      | 5       | address |
| 20      | 6       | address |
| 20      | 7       | address |
| 20      | 8       | address |
| 32      | 9       | uint    |
| 32      | 10      | uint    |
| 32      | 11      | uint    |
| 32      | 12      | uint    |
| 32      | 13      | uint    |
| 32      | 14      | uint    |
| 32      | 15      | uint256 |
| 32      | 16      | bytes32 |
| 32      | 17      | mapping |
| 32      | 18      | mapping |
| 32      | 19      | mapping |