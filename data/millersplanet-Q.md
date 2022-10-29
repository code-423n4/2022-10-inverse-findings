## Summary

`Initialize` function is sensitive to front-running attacks. (INVEscrow.sol#44)

## Vulnerability Detail

This function is intended to be called immediately after deployment of the contract. Nonetheless, since the function is unprotected and available to call by any user, an attacker could see the transaction on the public mempool and front-run the transaction.

## Impact

An attacker that successfully intercepts the transaction could declare his/her address as the `market`, as in line 44 :
`market = msg.sender;`, as well as declaring an arbitrary `token` address and `beneficiary`. This could lead to the attacker be available to call the `pay()` function and the `delegate()` function and drain the contract of any of the funds that may be stored in it.

## Code Snippet

```solidity
function initialize(IERC20 _token, address _beneficiary) public {
    require(market == address(0), "ALREADY INITIALIZED");
    market = msg.sender;
    token = _token;
    beneficiary = _beneficiary;
    _token.delegate(_token.delegates(_beneficiary));
    _token.approve(address(xINV), type(uint).max);
    xINV.syncDelegate(address(this));
}
```

## Tool used

Manual Review

## Recommendation

Add a specific caller address for the `initialize()` function.
```solidity
address initializer;

constructor(IXINV _xINV) {
   xINV = _xINV; 
   initializer = msg.sender;
}

function initialize(IERC20 _token, address _beneficiary) public {
   require(msg.sender == initializer, "INITIALIZER ONLY");
}
```

 Alternatively, you can send the transaction call to a private mempool, e.g: via Flashbots.