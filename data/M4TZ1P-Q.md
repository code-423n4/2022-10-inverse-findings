# Need Input validation for unchangeable **variables** in constructor

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L61-L90](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L61-L90)
[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L36-L42](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L36-L42)

The unchangeable values must be valid when deploying the contract. In `Market.sol` and `Fed.sol` contracts, there are unchangeable variables including immutable variables. 

The following variables need to be validated.

`Market.sol`: `gov`, `escrowImplementation`, `dbr`, `collateral`, `callOnDepositCallback`

`Fed.sol`: `dbr`, `dola`

# datatype are inconsistent

The contract are using both `uint` and `uint256`. uint is alias for uint256. However, for readability and consistency of your code, select one of them to write code. 
I recommend you to use `uint256`for consistency with other uint data types which specify size, and readability. Also sometime in the future, the meaning of `uint` could change to uint512 or something.

# SimpleERC20Escrow.initialize has unused parameter.

```solidity
function initialize(IERC20 _token, address) public {
        require(market == address(0), "ALREADY INITIALIZED");
        market = msg.sender;
        token = _token;
    }
```

`initialize()` has two parameter (IERC20, address) but address is not used and has no name.

Remove address parameter.