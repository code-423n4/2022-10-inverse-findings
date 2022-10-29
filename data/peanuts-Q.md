## Table of Contents

- [L-01] ecrecover() not checked for signer address of zero
- [N-01] Non-library/interface files should use fixed compiler versions, not floating ones
- [N-02] Events should be written on the top of the contract instead of at the very bottom
- [N-03] Typos


### [L-01] ecrecover() not checked for signer address of zero

The ecrecover() function returns an address of zero when the signature does not match. This can cause problems if address zero is ever the owner of assets, and someone uses the permit function on address zero. If that happens, any invalid signature will pass the checks, and the assets will be stealable. In this case, the asset of concern is the vault’s ERC20 token, and fortunately OpenZeppelin’s implementation does a good job of making sure that address zero is never able to have a positive balance. If this contract ever changes to another ERC20 implementation that is laxer in its checks in favor of saving gas, this code may become a problem.

Use OpenZeppelin's ECDSA contract rather than calling ecrecover() directly.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L226

### [N-01] Non-library/interface files should use fixed compiler versions, not floating ones

Lock floating pragma in all non-library files

### [N-02] Events should be written at the top of the contract instead of at the very bottom

For better readability, eg [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L381-L386)

### [N-03] Typos

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L127-L132

```
    /**
    @notice Get the DBR deficit of an address. Will return 0 if (th)the user has zero DBR or more.
    @dev The deficit of a user is calculated as the difference between the user's accrued DBR (deb)debt+ due DBR debt and their balance.
    @param user Address of the user.
    @return uint representing the deficit of the user.
    */
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L146

```
    @dev Collateral factor (mus) must be set below 100%
```

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L201

```
    @param amount The amount (od) of DOLA to recall to the the lender.
```