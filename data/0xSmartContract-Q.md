## Summary

### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]| Use `safeSetgov` instead of ```setGov``` function | 1 |
|[L-02]| In Market.sol, `gov` address value, critical 0 address control is missing | 1 |
|[L-03]| Very critical `gov` privileges can cause complete destruction of the project in a possible privateKey exploit| 1 |
|[L-04]| Signature Malleability of EVM's ecrecover()| 1 |
|[L-05]| Add updatable `dola` address function| 1 |

Total 5 issues

### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [NC-01 ]| Insufficient coverage | |
| [NC-02] |`0 address` check |10|
| [NC-03] | `Function writing` that does not comply with the `Solidity Style Guide` | All contracts |
| [NC-04] | Compliance with Solidity Style rules in Constant expressions | 1 |
| [NC-05] | Omissions in Events | 2 |
| [NC-06] | Need Fuzzing test| 7 |
| [NC-07] | Use a more recent version of Solidity| All contracts |
| [NC-08] |Solidity compiler optimizations can be problematic | All contracts |
| [NC-09] |Avoid whitespaces while declaring mapping variables  | 4 |
| [NC-10] |Require revert cause should be known| 1  |
| [NC-11] |NatSpec is missing | 38 |
| [NC-12] |Floating Pragma| All contracts |
| [NC-13] |For functions, follow Solidity standard naming conventions| 9 |
| [NC-14] |Missing Event for Critical Parameters Change | 16 |
| [NC-15] |Open TODOS | 1 |
| [NC-16] |Add NatSpec Mapping comment | 6 |

Total 16 issues


### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Add to _blacklist function_ |
| [S-02] | Generate perfect code headers every time |
| [S-03] | Add NatSpec comments to the variables defined in Storage |

Total 3 suggestions


## Details


### [L-01] Use `safeSetgov` instead of ```setGov``` function

**Context:**

```solidity
src/Market.sol:
  129      */
  130:     function setGov(address _gov) public onlyGov { gov = _gov; }
  131: 
  132      /**

```

**Description:**

Use a 2 structure `setGov` which is safer. 
`safeSetgov` , use it is more secure due to 2-stage ownership transfer.

**Recommendation:**
Use ``Ownable2Step.sol``
[Ownable2Step.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)

```js
 /**
     * @dev The new owner accepts the gov transfer.
     */
    function acceptGov() external {
        address sender = _msgSender();
        require(pendingGov() == sender, "Ownable2Step: caller is not the new gov");
        _transferGov(sender);
    }
}
```
### [L-02]  In Market.sol, `gov` address value, critical 0 address control is missing

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77


### Impact

With the constructor in the `market.sol` file, the initial address of `gov` is set with an argument of type address, but there is no check that prevents this address from starting with 0.
There is a risk that the `gov` variable is accidentally initialized to 0

Passing the address variable blank causes it to be set as 0 address.

The address initialized with 0 by mistake or forgetting cant be changed.

The value of `gov` is the value of the `onlyGov` modifier and should be msg.sender = gov, any function with this modifier will not work as this is not possible at 0 addresses

The biggest risk is; Until this value is found to be incorrect, the project can be used, users can trade, use the platform




```solidity

src/Market.sol:
  60  
  61:     constructor (
  62:         address _gov,
  63:         address _lender,
  64:         address _pauseGuardian,
  65:         address _escrowImplementation,
  66:         IDolaBorrowingRights _dbr,
  67:         IERC20 _collateral,
  68:         IOracle _oracle,
  69:         uint _collateralFactorBps,
  70:         uint _replenishmentIncentiveBps,
  71:         uint _liquidationIncentiveBps,
  72:         bool _callOnDepositCallback
  73:     ) {
  74:         require(_collateralFactorBps < 10000, "Invalid collateral factor");
  75:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
  76:         require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
  77:         gov = _gov;
  78:         lender = _lender;
  79:         pauseGuardian = _pauseGuardian;
  80:         escrowImplementation = _escrowImplementation;
  81:         dbr = _dbr;
  82:         collateral = _collateral;
  83:         oracle = _oracle;
  84:         collateralFactorBps = _collateralFactorBps;
  85:         replenishmentIncentiveBps = _replenishmentIncentiveBps;
  86:         liquidationIncentiveBps = _liquidationIncentiveBps;
  87:         callOnDepositCallback = _callOnDepositCallback;
  88:         INITIAL_CHAIN_ID = block.chainid;
  89:         INITIAL_DOMAIN_SEPARATOR = computeDomainSeparator();
  90:     }

```

Similar issue is also present in below function;

```solidity
src/Market.sol:
  129      */
  130:     function setGov(address _gov) public onlyGov { gov = _gov; }
  131: 
  132      /**
```


### Proof of Concept
1- Platform is accidentally initialized with 0 `gov`
2- Users start using the platform, invest capital
3- It is impossible to update the `gov` value and the project has been started, there will be a serious reputation problem and users may lose money





### Tools Used
Manual Code Review

### Recommended Mitigation Steps

Add an if block to the constructor ;

```js

error ZeroAddressError();

        if(_ gov = address(0))
            ZeroAddressError();
}
```

or add a constructor ;

```js
_gov = msg.sender;

```
### [L-03]  Very critical `gov` privileges can cause complete destruction of the project in a possible privateKey exploit

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L92-L95

### Vulnerability details
The contract’s `gov` is most impartant power role in this project. the gov is able to perform certain privileged activities.

However, `gov` privileges are numerous and there is no timelock structure in the process of using these privileges.

The `gov` is assumed to be an EOA, since the documents do not provide information on whether the `gov`  will be a multisign structure.

In parallel with the private key thefts of the project owners, which have increased recently, this vulnerability has been stated as medium.

Similar vulnerability;
Private keys stolen:

Hackers have stolen cryptocurrency worth around €552 million from a blockchain project linked to the popular online game Axie Infinity, in one of the largest cryptocurrency heists on record. Security issue : PrivateKey of the project officer was stolen:
https://www.euronews.com/next/2022/03/30/blockchain-network-ronin-hit-by-552-million-crypto-heist

This vulnerability can also be considered as a Rugpull risk.

### Proof of Concept

`gov` powers;
```solidity
10 results - 1 file

src/Market.sol:
  117      //sets the oracle to a new oracle. Only callable by governance.
  118:     function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

  123      //sets the borrow controller to a new borrow controller. Only callable by governance.
  124:     function setBorrowController(IBorrowController _borrowController) public onlyGov

  129      //sets the address of governance. Only callable by governance.
  130:     function setGov(address _gov) public onlyGov { gov = _gov; }

  135      //sets the lender to a new lender. The lender is allowed to recall dola from the contract. 
  136:     function setLender(address _lender) public onlyGov { lender = _lender; }

  141      //sets the pause guardian. The pause guardian can pause borrowing. Only callable by governance
  142:     function setPauseGuardian(address _pauseGuardian) public onlyGov  

  148      //sets the Collateral Factor requirement of the market as measured in basis points
  149:     function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

  160      //sets the Liquidation Factor of the market as denoted in basis points
  161:     function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

  171      //sets the Replenishment Incentive of the market as denoted in basis points
  172:     function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

  182      //sets the Liquidation Incentive of the market as denoted in basis points
  183:     function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

  193      //sets the Liquidation Fee of the market as denoted in basis points.
  194:     function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {


```

### Recommendation;

1- A timelock contract should be added to use `gov` privileges. In this way, users can be warned in case of a possible security weakness.

2- `gov` can be a Multisig wallet and this part is specified in the documentation


### [L-04] Signature Malleability of EVM's ecrecover()

**Context:**

```solidity
4 results - 3 files

src/DBR.sol:
  225          unchecked {
  226:             address recoveredAddress = ecrecover(
  227                  keccak256(

src/Market.sol:
  424          unchecked {
  425:             address recoveredAddress = ecrecover(
  426                  keccak256(

  488          unchecked {
  489:             address recoveredAddress = ecrecover(
  490                  keccak256(

```
**Description:**
Description: The function calls the Solidity ecrecover() function directly to verify the given signatures. However, the ecrecover() EVM opcode allows malleable (non-unique) signatures and thus is susceptible to replay attacks.

Although a replay attack seems not possible for this contract, I recommend using the battle-tested OpenZeppelin's ECDSA library.

*Recommendation:**
Use the ecrecover function from OpenZeppelin's ECDSA library for signature verification. (Ensure using a version > 4.7.3 for there was a critical bug >= 4.1.0 < 4.7.3)


### [L-05] Add updatable `dola` address function

**Description:**
The `dola`address may change in the future, so add a function that ensures that this address can be changed safely.

```solidity
src/Market.sol:
  36: contract Market {

  44:     IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);

```

### [N-01] Insufficient coverage

**Description:**
The test coverage rate of the project is 89%. Testing all functions is best practice in terms of security criteria.

```js

+-----------------------------------+------------------+------------------+------------------+-----------------+
| File                              | % Lines          | % Statements     | % Branches       | % Funcs         |
+==============================================================================================================+
| src/DBR.sol                       | 98.77% (80/81)   | 98.92% (92/93)   | 94.74% (36/38)   | 96.00% (24/25)  |
|-----------------------------------+------------------+------------------+------------------+-----------------|
| src/Fed.sol                       | 93.94% (31/33)   | 94.44% (34/36)   | 84.62% (22/26)   | 87.50% (7/8)    |
|-----------------------------------+------------------+------------------+------------------+-----------------|
| src/Market.sol                    | 96.45% (136/141) | 95.53% (171/179) | 85.00% (68/80)   | 92.11% (35/38)  |
|-----------------------------------+------------------+------------------+------------------+-----------------|
| src/escrows/GovTokenEscrow.sol    | 0.00% (0/10)     | 0.00% (0/10)     | 0.00% (0/6)      | 0.00% (0/4)     |
|-----------------------------------+------------------+------------------+------------------+-----------------|
| src/escrows/INVEscrow.sol         | 0.00% (0/20)     | 0.00% (0/25)     | 0.00% (0/10)     | 0.00% (0/5)     |
|-----------------------------------+------------------+------------------+------------------+-----------------|

```

Due to its capacity, test coverage is expected to be 100%

### [N-02] `0 address` check

**Context:**
[BorrowController.sol#L14](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14)
[BorrowController.sol#L26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)
[BorrowController.sol#L32](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32)
[BorrowController.sol#L38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38)
[DBR.sol#L36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L36)
[DBR.sol#L36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L37)
[DBR.sol#L36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L38)
[DBR.sol#L36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39)
[Fed.sol#L39](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39)
[Market.sol#L77-L80](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L80)

**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case. This is best practice or instead of suggesting that they verify address != 0x0, you could add some good NatSpec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

**Recommendation:**
like this;
`if (oracle == address(0)) revert ADDRESS_ZERO();`

### [N-03] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
All Contracts

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
within a grouping, place the view and pure functions last


### [N-04] Compliance with Solidity Style rules in Constant expressions

**Context:**

```solidity
src/DBR.sol:
  13:     uint8 public constant decimals = 18;
```

**Recommendation:**
Variables are declared as constant utilize the UPPER_CASE_WITH_UNDERSCORES format.


### [N-05] Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:

Events with no old value;

```solidity

src/DBR.sol:
  69      */
  70:     function claimOperator() public {
  71:         require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
  72:         operator = pendingOperator;
  73:         pendingOperator = address(0);
  74:         emit ChangeOperator(operator);
  75:     }
  76  

src/Oracle.sol:
  65      */
  66:     function claimOperator() public {
  67:         require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
  68:         operator = pendingOperator;
  69:         pendingOperator = address(0);
  70:         emit ChangeOperator(operator);
  71:     }

```
### [N-06] Need Fuzzing test

**Context:**
35 results - 9 files
Project uncheckeds list:

```js

7 results - 2 files

src/DBR.sol:
  172          balances[msg.sender] -= amount;
  173:         unchecked {
  174              balances[to] += amount;

  196          balances[from] -= amount;
  197:         unchecked {
  198              balances[to] += amount;

  236          require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
  237:         unchecked {
  238              address recoveredAddress = ecrecover(

  373          _totalSupply += amount;
  374:         unchecked {
  375              balances[to] += amount;

  387          balances[from] -= amount;
  388:         unchecked {
  389              _totalSupply -= amount;

src/Market.sol:
  423          require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
  424:         unchecked {
  425              address recoveredAddress = ecrecover(

  487          require(deadline >= block.timestamp, "DEADLINE_EXPIRED");
  488:         unchecked {
  489              address recoveredAddress = ecrecover(

```

**Description:**
In total 2 contracts, 7 unchecked are used, the functions used are critical. For this reason, there must be fuzzing tests in the tests of the project. Not seen in tests.

**Recommendation:**
Use should fuzzing test like Echidna.

As Alberto Cuesta Canada said:
_Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now._

https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05


### [N-07] Use a more recent version of Solidity

**Context:**
All contracts

**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used `(0.8.13)`, newer version can be used `(0.8.17)` 


### [N-08] Solidity compiler optimizations can be problematic

**Context:**
[foundry.toml#L6](https://github.com/code-423n4/2022-10-traderjoe/blob/main/foundry.toml#L6)

```js
 "src/Market.sol": {
      "lastModificationDate": 1666728684104,
      "contentHash": "6cf290b0486fc2d9b12c7c36d9303d9f",
      "sourceName": "src/Market.sol",
      "solcConfig": {
        "settings": {
          "optimizer": {
            "enabled": true,
            "runs": 200
```

**Description:**
Protocol has enabled optional compiler optimizations in Solidity.
There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. 

Therefore, it is unclear how well they are being tested and exercised.
High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. 

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported.
A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe.
It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario
A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.



**Recommendation:**
Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.
Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

### [N-09] Avoid whitespaces while declaring mapping variables 

Style Guide helps to maintain code layout consistent and make code more readable

Mappings : Avoid whitespaces while declaring mapping variables.

```solidity
src/DBR.sol:
    9: contract DolaBorrowingRights {

  24:     mapping (address => bool) public minters;
  25:     mapping (address => bool) public markets;

  26:     mapping (address => uint) public debts; 
  27:     mapping (address => uint) public dueTokensAccrued; 
  28:     mapping (address => uint) public lastUpdated; 
```


### [N-10] Require revert cause should be known

**Context:**

```solidity
src/Fed.sol:
  85      */
  86:     function expansion(IMarket market, uint amount) public {

  93:         require(globalSupply <= supplyCeiling);

```
**Description:**
Vulnerability related to description is not written to require, it must be written so that the user knows why the process is reverted.

This is an important debug analysis for programs and users that analyze transaction revert details such as Tenderly.

**Recommendation:**
```solidity
src/Fed.sol:
  85      */
  86:     function expansion(IMarket market, uint amount) public {

  93:         require(globalSupply <= supplyCeiling, "Should not exceed globalSupply ");

```


### [N-11] NatSpec is missing 

**Description:**
NatSpec is missing for the following functions , constructor and modifier:

**Context:**

```solidity
src/Market.sol:
   35  
   36: contract Market {
   37: 
   38:     address public gov;
   39:     address public lender;
   40:     address public pauseGuardian;
   41:     address public immutable escrowImplementation;
   42:     IDolaBorrowingRights public immutable dbr;
   43:     IBorrowController public borrowController;
   44:     IERC20 public immutable dola = IERC20(0x865377367054516e17014CcdED1e7d814EDC9ce4);
   45:     IERC20 public immutable collateral;
   46:     IOracle public oracle;
   47:     uint public collateralFactorBps;
   48:     uint public replenishmentIncentiveBps;
   49:     uint public liquidationIncentiveBps;
   50:     uint public liquidationFeeBps;
   51:     uint public liquidationFactorBps = 5000; // 50% by default
   52:     bool immutable callOnDepositCallback;
   53:     bool public borrowPaused;
   54:     uint public totalDebt;
   55:     uint256 internal immutable INITIAL_CHAIN_ID;
   56:     bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
   57:     mapping (address => IEscrow) public escrows; // user => escrow
   58:     mapping (address => uint) public debts; // user => debt
   59:     mapping(address => uint256) public nonces; // user => nonce
   60: 
   61:     constructor (
   62:         address _gov,
   63:         address _lender,
   64:         address _pauseGuardian,
   65:         address _escrowImplementation,
   66:         IDolaBorrowingRights _dbr,
   67:         IERC20 _collateral,
   68:         IOracle _oracle,
   69:         uint _collateralFactorBps,
   70:         uint _replenishmentIncentiveBps,
   71:         uint _liquidationIncentiveBps,
   72:         bool _callOnDepositCallback
   73:     ) {
   74:         require(_collateralFactorBps < 10000, "Invalid collateral factor");
   75:         require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
   76:         require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
   77:         gov = _gov;
   78:         lender = _lender;
   79:         pauseGuardian = _pauseGuardian;
   80:         escrowImplementation = _escrowImplementation;
   81:         dbr = _dbr;
   82:         collateral = _collateral;
   83:         oracle = _oracle;
   84:         collateralFactorBps = _collateralFactorBps;
   85:         replenishmentIncentiveBps = _replenishmentIncentiveBps;
   86:         liquidationIncentiveBps = _liquidationIncentiveBps;
   87:         callOnDepositCallback = _callOnDepositCallback;
   88:         INITIAL_CHAIN_ID = block.chainid;
   89:         INITIAL_DOMAIN_SEPARATOR = computeDomainSeparator();
   90:     }
   91:     
   92:     modifier onlyGov {
   93:         require(msg.sender == gov, "Only gov can call this function");
   94:         _;
   95:     }
   96: 
   97:     function DOMAIN_SEPARATOR() public view virtual returns (bytes32) {
   98:         return block.chainid == INITIAL_CHAIN_ID ? INITIAL_DOMAIN_SEPARATOR : computeDomainSeparator();
   99:     }
  100: 
  101:     function computeDomainSeparator() internal view virtual returns (bytes32) {
  102:         return
  103:             keccak256(
  104:                 abi.encode(
  105:                     keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
  106:                     keccak256(bytes("DBR MARKET")),
  107:                     keccak256("1"),
  108:                     block.chainid,
  109:                     address(this)
  110:                 )
  111:             );
  112:     }
```

### [N-12] Floating Pragma

Description:Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
https://swcregistry.io/docs/SWC-103


**Context:**

```solidity
8 results - 8 files

src/BorrowController.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/DBR.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/Fed.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/Market.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/Oracle.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/escrows/GovTokenEscrow.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/escrows/INVEscrow.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3  

src/escrows/SimpleERC20Escrow.sol:
  1  // SPDX-License-Identifier: UNLICENSED
  2: pragma solidity ^0.8.13;
  3 
```

**Recommendation:**
Lock the pragma version and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

### [N-13]  For functions, follow Solidity standard naming conventions

**Description:**

The above codes don't follow Solidity's standard naming convention,
internal and private functions : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)
public and external functions : only mixedCase
constant variable : UPPER_CASE_WITH_UNDERSCORES (separated by uppercase and underscore)

**Context:**

```solidity
9 results - 2 files

src/DBR.sol:
  278  
  279:     function computeDomainSeparator() internal view virtual returns (bytes32) {
  280          return

src/Market.sol:
  100  
  101:     function computeDomainSeparator() internal view virtual returns (bytes32) {
  102          return

  225      */
  226:     function createEscrow(address user) internal returns (IEscrow instance) {
  227          address implementation = escrowImplementation;

  244      */
  245:     function getEscrow(address user) internal returns (IEscrow) {
  246          if(escrows[user] != IEscrow(address(0))) return escrows[user];

  329      */
  330:     function getCollateralValueInternal(address user) internal returns (uint) {
  331          IEscrow escrow = predictEscrow(user);

  350      */
  351:     function getCreditLimitInternal(address user) internal returns (uint) {
  352          uint collateralValue = getCollateralValueInternal(user);

  359      */
  360:     function getWithdrawalLimitInternal(address user) internal returns (uint) {
  361          IEscrow escrow = predictEscrow(user);

  395      */
  396:     function borrowInternal(address borrower, address to, uint amount) internal {
  397          require(!borrowPaused, "Borrowing is paused");

  466      */
  467:     function withdrawInternal(address from, address to, uint amount) internal {
  468          uint limit = getWithdrawalLimitInternal(from);
```

### [N-14]  Missing Event for Critical Parameters Change


**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes

**Context:**

```solidity
22 results - 10 files

src/BorrowController.sol:
  25      */
  26:     function setOperator(address _operator) public onlyOperator { operator = _operator; }
  27  

src/DBR.sol:
  52      */
  53:     function setPendingOperator(address newOperator_) public onlyOperator {
  54          pendingOperator = newOperator_;

  61      */
  62:     function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {
  63          require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

src/Market.sol:
  117      */
  118:     function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
  119  

  123      */
  124:     function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }
  125  

  129      */
  130:     function setGov(address _gov) public onlyGov { gov = _gov; }
  131  

  135      */
  136:     function setLender(address _lender) public onlyGov { lender = _lender; }
  137  

  141      */
  142:     function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
  143      

  148      */
  149:     function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
  150          require(_collateralFactorBps < 10000, "Invalid collateral factor");

  160      */
  161:     function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {
  162          require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

  171      */
  172:     function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {
  173          require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

  182      */
  183:     function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {
  184          require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

  193      */
  194:     function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
  195          require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

src/Oracle.sol:
  43      */
  44:     function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }
  45  

  52      */
  53:     function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
  54  

  60      */
  61:     function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```
**Recommendation:**
Add Event-Emit

### [N-15]  Open TODOS


**Context:**
```solidity
src/escrows/INVEscrow.sol:
  34      constructor(IXINV _xINV) {
  35:         xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies
  36      }
```

**Recommendation:**
Use temporary TODOs as you work on a feature, but make sure to treat them before merging. Either add a link to a proper issue in your TODO, or remove it from the code.

### [N-16]  Add NatSpec Mapping comment

**Description:**
Add NatSpec comments describing mapping keys and values


**Context:**

```solidity
6 results - 3 files

src/BorrowController.sol:
  11:     mapping(address => bool) public contractAllowlist;

src/DBR.sol:
  19:     mapping(address => uint256) public balances;
  20:     mapping(address => mapping(address => uint256)) public allowance;
  23:     mapping(address => uint256) public nonces;
  24      mapping (address => bool) public minters;

src/Oracle.sol:
  26      mapping (address => uint) public fixedPrices;

```
**Recommendation:**

```solidity

 /// @dev    address(user) -> address(ERC0 Contract Address) -> uint256(allowance amount from user)
    mapping(address => mapping(address => uint256)) public allowance;

```
### [S-01] Add to _blacklist function_

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.gov/policy-issues/financial-sanctions/recent-actions/20220808

Some of these Projects;
 USDC 
 Aave 
 Uniswap
 Balancer
 Infura
 Alchemy 
 Opensea
 dYdX 

For this reason, every project in the Ethereum network must have a blacklist function, this is a good method to avoid legal problems in the future, apart from the current need.

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier.

### [S-02] Generate perfect code headers every time

**Description:**
I recommend using header for Solidity code layout and readability

https://github.com/transmissions11/headers

```js
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```

### [S-03] Add NatSpec comments to the variables defined in Storage

**Description:**
I recommend adding NatSpec comments explaining the variables defined in Storage, their slots, their contents and definitions.

This improves code readability and control quality


Current Code;
```solidity
src/DBR.sol:
   8  */
   9: contract DolaBorrowingRights {
  10: 
  11:     string public name;
  12:     string public symbol;
  13:     uint8 public constant decimals = 18;
  14:     uint256 public _totalSupply;
  15:     address public operator;
  16:     address public pendingOperator;
  17:     uint public totalDueTokensAccrued;
  18:     uint public replenishmentPriceBps;
  19:     mapping(address => uint256) public balances;
  20:     mapping(address => mapping(address => uint256)) public allowance;
  21:     uint256 internal immutable INITIAL_CHAIN_ID;
  22:     bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;
  23:     mapping(address => uint256) public nonces;
  24:     mapping (address => bool) public minters;
  25:     mapping (address => bool) public markets;
  26:     mapping (address => uint) public debts; // user => debt across all tracked markets
  27:     mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
  28:     mapping (address => uint) public lastUpdated; // user => last update timestamp
```

Recommendation Code;

```solidity
 /****** Slot 0 ******/
  /**
   * @notice Project name
   * @dev Value is defined in the constructor
   */
string public name;

  /****** Slot 1 ******/
  /**
   * @notice Project symbol
   * @dev Value is defined in the constructor
   */
string public symbol

  /****** Slot 2 ******/
  /**
   * @notice decimal
   */
uint8 public constant decimals = 18;

...

  /****** End of storage ******/
```

