# 1. Tautology on `require(a != b && a == c);` can be replaced by `require(a == c);`

Code changes

```diff
diff --git a/src/Market.sol b/src/Market.sol
index 9585b85..a6c76f7 100644
--- a/src/Market.sol
+++ b/src/Market.sol
@@ -445,7 +445,7 @@ contract Market {
                 r,
                 s
             );
-            require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
+            require(recoveredAddress == from, "INVALID_SIGNER");
             borrowInternal(from, msg.sender, amount);
         }
     }

```

Gas optimization

```
$ forge snapshot --diff .gas-snapshot

testBorrowOnBehalf() (gas: -52 (-0.011%))
testBorrowOnBehalf_Fails_When_InvalidateNonceCalledPrior() (gas: -52 (-0.019%))
Overall gas change: -104 (-0.030%)
```