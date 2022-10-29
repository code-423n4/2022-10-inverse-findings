## Non-Critical Issues

## [N-01] `DOLABORROWINGRIGHTS::PERMIT`, `MARKET::BORROWONBEHALF`, AND `MARKET::WITHDRAWONBEHALF` AREN'T USABLE FOR SIGNING MESSAGES TO MULTIPLE PARTIES
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L226-L249

A signer has one nonce for all messages to a smart contract, and all messages must be submitted in order. This breaks the functionality of `DolaBorrowingRights::permit`, `Market:borrowOnBehalf`, and `Market::withdrawOnBehalf` if an account wants to sign messages to the same smart contract for multiple parties.

### Proof of Concept
Suppose Eve signs a message for Alice and Bob, with nonces of 0 and 1 for the same contract where Eve's nonce is 0. Bob wants to use his message before Alice and calls one of the functions above with his message. It will build a hash of the message, a nonce, and the contract's domain separator and check that it is signed by the owner, but will use a nonce of 0 resulting in the incorrect hash. It gets this nonce from a mapping of signer's addresses to nonces, which it increments on success. This ensures messages with the same nonce can't be replayed, but also only accepts messages with the current nonce.

[/src/DBR.sol#L226-L249](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L226-L249):
```solidity
226            address recoveredAddress = ecrecover(
227                keccak256(
228                    abi.encodePacked(
229                        "\x19\x01",
230                        DOMAIN_SEPARATOR(),
231                        keccak256(
232                            abi.encode(
233                                keccak256(
234                                    "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
235                                ),
236                                owner,
237                                spender,
238                                value,
239                                nonces[owner]++,
240                                deadline
241                            )
242                        )
243                    )
244                ),
245                v,
246                r,
247                s
248            );
249            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

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
