
### G-01 Add payable modifier where possible to save gas

Less opcodes are needed for functions marked payable.  Can safely save gas on onlyOperator functions by marking them payable since an operator is unlikely to accidentally send a payment to them.  This will reduce the cost of calling the functions marked payable by 20-30 gas per call.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61


### G-02 Packing of storage variables

In DBR.sol

```
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

It's best practice to sort variables from smallest to largest so they can pack better.  The 20 bytes address will pack with the uint8 if sorted like:

```
    uint8 public constant decimals = 18;  // 8 bytes
    address public operator;  // 20 bytes for addresses so this will pack with previous uint8
    address public pendingOperator;
```

The other variables can list underneath these without issue as they are all 32 byte.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L11-L16

Base DBR gas deployment cost 1411204 with deployment size 8005.  With this refactor DBR gas deployment cost 1410597 with deployment size 8002.  


### G-03 Use unchecked on arithmetic operations that have already been checked

In Fed.sol, line 110 can be unchecked to save gas because line 107 requires that supplies[market] is >= amount, so we know it won't underflow.

```
unchecked {
	supplies[market] -= amount;
}
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110

