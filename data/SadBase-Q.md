## lack of checks for address(0x0) when assigning state variables

### Fed.sol

```solidity
constructor (IDBR _dbr, IDola _dola, address _gov, address _chair, uint _supplyCeiling) {
dbr = _dbr;
dola = _dola;
gov = _gov;
chair = _chair;
supplyCeiling = _supplyCeiling;
}
```

Both variable, `_gov` and `_chair` lack of checks for address(0x0) when assigning state variables

```solidity
function changeGov(address _gov) public {
require(msg.sender == gov, "ONLY GOV");
gov = _gov;
}
```

```solidity
function changeSupplyCeiling(uint _supplyCeiling) public {
require(msg.sender == gov, "ONLY GOV");
supplyCeiling = _supplyCeiling;
}
```

```solidity
function changeChair(address _chair) public {
require(msg.sender == gov, "ONLY GOV");
chair = _chair;
}
```

