## `x += y` costs more gas than `x = x + y`

Here is an example:

File: `DBR.sol` [Line 304](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304)

```
debts[user] += additionalDebt;
```

The above could be changed to:

```
debts[user] = debts[user] + additionalDebt;
```

Here are some more instances found:

File: `Market.sol` [Line 395](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395)
File: `Market.sol` [Line 397](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397)
File: `Market.sol` [Line 534-535](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534-L535)
File: `Market.sol` [Line 565](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565)
File: `Market.sol` [Line 568](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568)
File: `Market.sol` [Line 598-600](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598-L600)

## Split `require()` statements that use `&&` operator

Splitting `require()` statements up saves around 3 gas for every `&&`

Here is an example:

File: `Market.sol` [Line 184](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)

```
require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
```

The above should be changed to:

```
require(_liquidationIncentiveBps > 0, "Invalid liquidation incentive");
require(_liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
```

Here are some more instances found:

File: `Market.sol` [Line 75](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
File: `Market.sol` [Line 162](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)
File: `Market.sol` [Line 173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)

## > costs less gas than >= (same for <, <=)

The reason `>` and `<` costs less gas is because in the EVM, there is no opcode for `>=` or `<=`

Here is an example:

File: `Market.sol` [Line 582](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L582)

```
if(credit >= debt) return 0;
```

The above can be replaced with

```
if(credit > debt - 1) return 0;
```

Here are some additional instances found:

File: `Fed.sol` [Line 123](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L123)
File: `Fed.sol` [Line 107](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L107)
File: `Market.sol` [Line 378](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L378)
File: `Market.sol` [Line 396](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L396)

## `abi.encodePacked()` is more gas-efficient than `abi.encode()`

The reason `abi.encode()` isn't as gas-efficient is because it pads extra null bytes at the end of the calldata while `abi.encodePacked()` does not.

Here is an example:

File: `Market.sol` [Line 104-110](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L104-L110)

```
abi.encode(
    keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
    keccak256(bytes("DBR MARKET")),
    keccak256("1"),
    block.chainid,
    address(this)
)
```

The above could be replaced to:

```
abi.encodePacked(
    keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
    keccak256(bytes("DBR MARKET")),
    keccak256("1"),
    block.chainid,
    address(this)
)
```

## External visibility costs less gas that public visibility

Functions not called internally can have its visibility set to external.

Here is an example:

File: `Market.sol` [Line 118](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118)

```
function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }
```

The function above could have its visibility set to external:

```
function setOracle(IOracle _oracle) external onlyGov { oracle = _oracle; }
```

Here are a few instances of this issue:

File: `Market.sol` [Line 124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124)
File: `Market.sol` [Line 130](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130)
File: `Market.sol` [Line 136](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136)
File: `Market.sol` [Line 142](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142)
File: `Market.sol` [Line 149](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149)
