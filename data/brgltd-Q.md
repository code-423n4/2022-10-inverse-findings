# [01] Lack of checks-effects-interactions in `Market.getEscrow()`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245-L251

The function `getEscrow()` called by `deposit()` will execute the external call `escrow.initialize()` before updating the state variable `escrows[user]`. Consider updating the state before the external call to use the checks-effects-interactions pattern.

```
diff --git a/Market.sol.orig b/Market.sol
--- a/Market.sol.orig
+++ b/Market.sol
     function getEscrow(address user) internal returns (IEscrow) {
         if(escrows[user] != IEscrow(address(0))) return escrows[user];
+        escrows[user] = escrow;
         IEscrow escrow = createEscrow(user);
         escrow.initialize(collateral, user);
-        escrows[user] = escrow;
         return escrow;
     }
```

# [02] Usage of tx.origin

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L47

tx.origin can be manipulated by malicious contracts calling into `BorrowController`. Consider removing tx.origin for authorization and using a pattern where `msg.sender` is set during initalization and verified for later calls.

# [03] block.timestamp strict equal check in `DBR.accrueDueTokens()`

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286

Consider replacing `lastUpdated[user] == block.timestamp`, with `lastUpdated[user] <= block.timestamp`, to ensure that the current timestamp must be bigger the last update.

# [04] Missing zero address checks for setters and constructor

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L80

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142

If a variable gets configured with address zero, failure to immediately reset the value can result in unexpected behavior for the project.

# [05] Critical changes should use two-step procedure

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

Consider adding a two-steps pattern on critical changes to avoid mistakenly transferring ownership of roles or critial functionalities to the wrong address.

# [06] Missing event for critical parameter changes

Adding events will facilitate offchain monitoring

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142

# [07] Floating pragma

All the contracts in scope are floating the pragma version.

Consider locking the pragma to ensure that contracts do not accidentally get deployed using an outdated compiler version.

Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package.

# [08] Repeated validation statements can be converted into a function modifier to improve code reusability

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149

# [09] Interchangeable usage of uint and uint256

Following instance is using uint256

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L55

Following instance is using uint

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L69

Consider using only one identifier throughout the codebase, e.g. only uint or only uint256.

# [10] Order of functions and events

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

Another [recommendation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout) from the style guide is related to declaring events before functions.

Consider adopting the following strategy throughout the codebase.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L258

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L258

# [11] Replace magic numbers with constants to improve code readability

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74-L76

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563-L564

# [12] Public functions not called by the contract should be declared external

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591
