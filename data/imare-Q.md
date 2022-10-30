### [L01]  ``ecrecover()`` allows malleable signatures

* https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L226
* https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L425
* https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L489

Best practice is to use OpenZeppelin’s ECDSA contract rather than calling ecrecover() directly

### [L02] variable is not persisted across proxies

* https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/escrows/INVEscrow.sol#L35

Variable passed in the constructor will not persist in the returned proxy by calling ``createEscrow``

### [L03] check if user is in the system inside ``forceReplenish`` ``liquidate`` and ``borrow`` functions

This functions will internally use `` IEscrow escrow = predictEscrow(user);`` and for non existing users there will be an unknown EVM revert error.

I suggest to check inside this functions if the user is in the system like this:

```
require(escrows[user] != IEscrow(address(0)), "Unknown user");
```