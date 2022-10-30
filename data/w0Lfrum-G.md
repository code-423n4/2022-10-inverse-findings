# Gas Optimization Report

This report describes opportunity for gas optimization in the order of appearance in the code.

## G01: Using `private` Instead of `public` for Constants Saves Gas

**Line Reference :**

[DBR.sol#L13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)

**Details and Solution**

When the constants are declared as `private`, the compiler doesn't have to create separate non-payable getter functions. This results in saving gas.

## G02: Using Custom Errors for Error Strings in `revert()` / `require()` Statements Saves gas

**Details and Solution**

All `revert()` and `require()` statements use ordinary strings as error messages. Using custom error strings in these `revert()` / `require()` statements saves gas.