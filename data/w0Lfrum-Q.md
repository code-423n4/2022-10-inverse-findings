# QA Report: Inverse Finance

## Summary

The overall code quality for the Inverse Finance contracts is great. The code is well-commented. Unit tests are provided. The logic is split into the corresponding files. The logic is clear when referring to the docs/information given in the code4rena contest page.

There can be improvements in the code by rectifying non-critical issues like specifying the pragma compiler version. Also, making the use of custom error messages will enhance code clarity along with saving gas fees.

It would also be better if the interfaces are inherited from separate files onto the contract files instead of placing both the interfaces and contracts in the same .sol file.

## Non-Critical Findings

### NC01: Unspecified Pragma Compiler Version

**Description:**

The pragma compiler version for all .sol files is not specified.

**Recommended Mitigation Steps**

Change `pragma solidity ^0.8.13;`  -->  `pragma solidity 0.8.13;`

### NC02: Error Message not Provided for `require()`

**Line References:**

[GovTokenEscrow.sol#L67](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67)

**Description:**

An error message is not provided for the require() statement

**Recommended Mitigation Steps:**

Provide an error string (preferably a custom error string) for the `require()` statement.