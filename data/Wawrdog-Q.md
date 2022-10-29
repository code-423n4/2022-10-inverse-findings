---

**Order of Layout as per style guide**

---

Pragma statements
Import statements
Interfaces
Libraries
Contracts
Type declarations
State variables
Events
Modifiers
Functions

source: https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout

---

The following contracts do not follow the order of layout:

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol


---

**Order of functions as per style guide:**

---

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

source: https://docs.soliditylang.org/en/v0.8.17/style-guide.html

---

The following contracts do not conform to solidity style guide order of functions:

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol