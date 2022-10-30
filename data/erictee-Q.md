### [L-01] ```approve()``` and ```safeApprove()``` should be replaced with ```safeIncreaseAllowance()``` / ```safeDecreaseAllowance()```


#### Impact
```approve()``` & ```safeApprove()``` are deprecated and subject to a known front-running attack. Consider using  ```safeIncreaseAllowance()``` & ```safeDecreaseAllowance()``` instead.


#### Findings:
```
src/escrows/INVEscrow.sol:L50                 _token.approve(address(xINV), type(uint).max);

```

### [L-02] ```decimals()``` not part of ERC20 standard.


#### Impact
```decimals()``` is not part of the official ERC20 standard and might fall for tokens that do not implement it. While in practice it is very unlikely, as usually most of the tokens implement it, this should still be considered as a potential issue.


#### Findings:
```
src/Oracle.sol:L85            uint8 feedDecimals = feeds[token].feed.decimals();

src/Oracle.sol:L119            uint8 feedDecimals = feeds[token].feed.decimals();

```

### [L-03] Use of ```ecrecover``` is susceptible to signature malleability


#### Impact
The ```ecrecover``` function is used to recover the address from the signature. The built-in EVM precompile ecrecover is susceptible to signature malleability which could lead to replay attacks (references: https://swcregistry.io/docs/SWC-117, https://swcregistry.io/docs/SWC-121 and https://medium.com/cryptronics/signature-replay-vulnerabilities-in-smart-contracts-3b6f7596df57).

Consider using OpenZeppelin’s ECDSA library (which prevents this malleability) instead of the built-in function.


#### Findings:
```
src/Market.sol:L425            address recoveredAddress = ecrecover(

src/Market.sol:L489            address recoveredAddress = ecrecover(

src/DBR.sol:L226            address recoveredAddress = ecrecover(

```

### [L-04] Avoid floating pragmas for non-library contracts.


#### Impact
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### Findings:
```
src/Market.sol:L2     pragma solidity ^0.8.13;

src/Oracle.sol:L2     pragma solidity ^0.8.13;

src/Fed.sol:L2     pragma solidity ^0.8.13;

src/DBR.sol:L2     pragma solidity ^0.8.13;

src/BorrowController.sol:L2     pragma solidity ^0.8.13;

src/escrows/INVEscrow.sol:L2     pragma solidity ^0.8.13;

src/escrows/GovTokenEscrow.sol:L2     pragma solidity ^0.8.13;

src/escrows/SimpleERC20Escrow.sol:L2     pragma solidity ^0.8.13;

```

### [L-05] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
src/Fed.sol:L93        require(globalSupply <= supplyCeiling);

src/escrows/INVEscrow.sol:L91        require(msg.sender == beneficiary);

src/escrows/GovTokenEscrow.sol:L67        require(msg.sender == beneficiary);

```

### [L-06] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
src/DBR.sol:L333        _mint(user, amount);

src/DBR.sol:L351        _mint(to, amount);

src/DBR.sol:L359    function _mint(address to, uint256 amount) internal virtual {

```

### [N-01] Open TODOs


#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.


#### Findings:
```
src/escrows/INVEscrow.sol:L35        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies

```


