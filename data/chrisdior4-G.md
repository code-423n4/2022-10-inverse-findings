=======================================
# += COSTS MORE GAS THAN = + FOR STATE VARIABLES
###  Using the addition operator instead of plus-equals saves 113 gas

File:DBR.sol

1.balances[msg.sender] -= amount; 
1.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L172

### Consider changing it to: 
1.balances[msg.sender] = balances[msg.sender] - amount


2.balances[to] += amount;
2.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L198

3.dueTokensAccrued[user] += accrued;
3.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L288

4.totalDueTokensAccrued += accrued;
4.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L289

5.debts[user] += additionalDebt
5.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L304

6.debts[user] += replenishmentCost;
6.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L332

7._totalSupply += amount;
7.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L360

8.balances[to] += amount;
8.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L362


====================================

File:DBR.sol

9.supplies[market] += amount;
9.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L91

10.globalSupply += amount;
10.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L92

11.supplies[market] -= amount;
11.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L110

12.globalSupply -= amount;
12.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L111

=====================================

# COMPARISONS: BOOLEAN COMPARISONS

### Comparing to a constant (true or false) is a bit more expensive than directly checking the returned boolean value. I suggest using:


if(minters[msg.sender]) instead of if(minters[msg.sender] == true) here:

1.require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
1.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L350


======================================

# BYTES CONSTANT ARE CHEAPER THAN STRING CONSTANTS

### If the string can fit into 32 bytes, then bytes32 is cheaper than string. string is a dynamically sized-type, which has current limitations in Solidity compared to a statically sized variable. This means extra gas spent upon deployment and every time the constant is read.

File: StdAssertions.t.sol

1.string constant CUSTOM_ERROR = "guh!";
1.https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/lib/forge-std/src/test/StdAssertions.t.sol#L8

### Consider changing it to:
1.bytes32 constant CUSTOM_ERROR = "guh!";

========================================