# Cache Storage Variables in Memory Variable In Function
When a storage variable from a contract is used at least 2 times in the same function, it is possible to reduce gas consumption by caching it into a memory variable. This way we avoid additional SLOAD.

## Market.sol
The function `getWithdrawalLimitInternal` calls the variable `collateralFactorBps` 3 times, so it can be cached : https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353:L363

    function getWithdrawalLimitInternal(address user) internal returns (uint) {
        IEscrow escrow = predictEscrow(user);
        uint collateralBalance = escrow.balance();
        if(collateralBalance == 0) return 0;
        uint debt = debts[user];
        if(debt == 0) return collateralBalance;

        uint _collateralFactorBps = collateralFactorBps;
        if(_collateralFactorBps == 0) return 0;
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), _collateralFactorBps) * 10000 / _collateralFactorBps;
        if(collateralBalance <= minimumCollateral) return 0;
        return collateralBalance - minimumCollateral;
    }

The `liquidate` function calls the variable `liquidationFeeBps` 2 times when condition is passed (which is most likely to happen) : https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605:L610

    uint _liquidationFeeBps = liquidationFeeBps;
    if (_liquidationFeeBps > 0) {
        uint liquidationFee = repaidDebt * 1 ether / price * _liquidationFeeBps / 10000;
        if(escrow.balance() >= liquidationFee) {
            escrow.pay(gov, liquidationFee);
        }
    }

# Tight Variable Packing
In Solidity, we can pack multiple contract variables into the same 32 bytes slot in storage. With this pattern, we can greatly optimize gas consumption when storing or loading statically-sized variables.

## Market.sol
Firstly, I would recommend to change the order of the contract variables' declaration to be more clear and to be able to use this pattern, below is the recommendation:

    uint256 internal immutable INITIAL_CHAIN_ID;
    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
    address public immutable escrowImplementation;
    bool immutable callOnDepositCallback;

    IDolaBorrowingRights public immutable dbr;
    IBorrowController public borrowController;
    IOracle public oracle;

    IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
    IERC20 public immutable collateral;

    uint public totalDebt;
    mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce

    address public gov;
    address public lender;
    address public pauseGuardian;

    uint16 public collateralFactorBps;
    uint16 public replenishmentIncentiveBps;
    uint16 public liquidationIncentiveBps;
    uint16 public liquidationFeeBps;
    uint24 public liquidationFactorBps = 5000; // 50% by default
    bool public borrowPaused;

In this contract, we can see that there are 5 `uint` variables that will never exceed the value of 10000: `collateralFactorBps`, `replenishmentIncentiveBps`, `liquidationIncentiveBps`, `liquidationFeeBps` and `liquidationFactorBps`. Therefore, we can pack the first 4 into a `uint16` variable type (uint16 max value = 65535) and the last on can be a `uint24` (max value = 16777215).

Bytes used per type:
- `uint16` = 2 bytes
- `uint24` = 3 bytes
- `address` = 20 bytes
- `bool` = 1 byte

With these new types and the new declaration's order, we are able to pack 7 variables into the same 32 bytes storage slot (20 + 2 + 2 + 2 + 2 + 3 + 1 = 32 bytes) ! This will lead to a much lower gas consumption when these variables are called or modified.

By doing so, we must also change the type in functions' arguments where these previous `uint` variables were changed.
### setCollateralFactorBps
https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Market.sol#L149

    function setCollateralFactorBps(uint16 _collateralFactorBps) public onlyGov

### setReplenismentIncentiveBps
https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Market.sol#L172

    function setReplenismentIncentiveBps(uint16 _replenishmentIncentiveBps) public onlyGov

### setLiquidationIncentiveBps
https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Market.sol#L183

    function setLiquidationIncentiveBps(uint16 _liquidationIncentiveBps) public onlyGov

### setLiquidationFeeBps
https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Market.sol#L194

    function setLiquidationFeeBps(uint16 _liquidationFeeBps) public onlyGov

### setLiquidationFactorBps
https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Market.sol#L161

    function setLiquidationFactorBps(uint24 _liquidationFactorBps) public onlyGov

### Interface
The `IOracle` interface defined in the `Market` contract must be modified too in consequences : https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Market.sol#L11:L14

    interface IOracle {
        function getPrice(address,uint16) external returns (uint);
        function viewPrice(address,uint16) external view returns (uint);
    }


## Oracle.sol
As the second argument of the functions `getPrice` and `viewPrice` has changed in the interface from a `uint` to an `uint16`, we must also change this directly at the function declaration.

    function viewPrice(address token, uint16 collateralFactorBps) external view returns (uint)

    function getPrice(address token, uint16 collateralFactorBps) external returns (uint)

- https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Oracle.sol#L78
- https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/Oracle.sol#L112