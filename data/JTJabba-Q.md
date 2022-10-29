## Non-Critical Issues

## [N-01] `DOLABORROWINGRIGHTS::PERMIT`, `MARKET::BORROWONBEHALF`, AND `MARKET::WITHDRAWONBEHALF` AREN'T USABLE FOR SIGNING MESSAGES TO MULTIPLE PARTIES
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L226-L249

A signer has one nonce for all messages to a smart contract, and all messages must be submitted in order. This means`DolaBorrowingRights::permit`, `Market::borrowOnBehalf`, and `Market::withdrawOnBehalf` aren't usable for signing messages to multiple parties that can't be expected to collaborate to submit their messages in order.

### Recommended Mitigation Steps
It is possible for the owner to have a unique nonce for each spender. A unique nonce is already being used for messages to different contracts.

Example implementing in DolaBorrowingRights:
```solidity
// would replace /src/DBR.sol#L23
    mapping(address => mapping(address => uint256)) public nonces;
    
// would replace /src/DBR.sol#L239
                                nonces[owner][spender]++,
                                
// would replace /src/DBR.sol#L258-L260
    function invalidateNonce(address spender) public {
        nonces[msg.sender][spender]++;
    }
```