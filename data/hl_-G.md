### [G-01] Boolean comparison - use !x instead of x == false

To compare a constant (true or false) is slightly more costly than directly checking the returned boolean value.

```

require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");

```

should be

```

require(minters[msg.sender] || msg.sender == operator, "ONLY MINTERS OR OPERATOR");

```

[https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L350](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L350)