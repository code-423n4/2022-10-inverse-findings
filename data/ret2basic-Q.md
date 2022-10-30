# Inverse Finance Contest QA Report

## Summary

The following QA issues were found during the code audit:

1. Unsafe ERC20 Operation(s) (7 instances)
2. Unspecific Compiler Version Pragma (5 instances)

Total 12 instances of 2 issues.

## 1. Unsafe ERC20 Operation(s) (7 instances)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard. It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

```solidity
src/Fed.sol::135 => dola.transfer(gov, profit);

src/Market.sol::205 => dola.transfer(msg.sender, amount);

src/Market.sol::280 => collateral.transferFrom(msg.sender, address(escrow), amount);

src/Market.sol::399 => dola.transfer(to, amount);

src/Market.sol::537 => dola.transferFrom(msg.sender, address(this), amount);

src/Market.sol::570 => dola.transfer(msg.sender, replenisherReward);

src/Market.sol::602 => dola.transferFrom(msg.sender, address(this), repaidDebt);
```

## 2. Unspecific Compiler Version Pragma (5 instances)

Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version.

```solidity
src/BorrowController.sol::2 => pragma solidity ^0.8.13;

src/DBR.sol::2 => pragma solidity ^0.8.13;

src/Fed.sol::2 => pragma solidity ^0.8.13;

src/Market.sol::2 => pragma solidity ^0.8.13;

src/Oracle.sol::2 => pragma solidity ^0.8.13;
```
