## Table of Contents 

- [G-01] <x> += <y> costs more gas than <x> = <x> + <y> for state variables
- [G-02] Boolean comparison , use !x instead of x == false 

### [G-01] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

Using the addition operator instead of plus-equals saves 113 gas. _totalSupply() 

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L376

### [G-02] Boolean comparison , use !x instead of x == false

Comparing to a constant (true or false) is a bit more expensive than directly checking the returned boolean value.

```
require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

should be 

```
require(minters[msg.sender] || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L350
