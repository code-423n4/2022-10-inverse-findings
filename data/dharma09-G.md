## 1. REQUIRE STATEMENTS INCLUDING CONDITIONS WITH THE `&&` OPERATOR CAN BE BROKEN DOWN IN MULTIPLE REQUIRE STATEMENTS TO SAVE GAS.

## PROOF OF CONCEPT
If you’re using the Optimizer at 200, instead of using the `&&` operator in a single require statement to check multiple conditions, Consider using multiple require statements with 1 condition per require statement:


Instances include:
### [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)
```
Market.sol#L75        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");
Market.sol#L162      require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");
Market.sol#L173      require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
Market.sol#L184      require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");
Market.sol#L195      require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");               
```
### [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249)
```
DBR.sol#L249    require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```
> Please, note that this might not hold true at a higher number of runs for the Optimizer (10k). However, it indeed is true at 200.

## 2. DUPLICATED CONDITIONS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION TO SAVE DEPLOYMENT COSTS
It's better to refactor these into a modifier or function if they start to grow in number.

### [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)
```
Market.sol#L448      require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
Market.sol#L512      require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
## 3.USING `BOOLS` FOR STORAGE INCURS OVERHEAD
[OpenZeppelin/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)

Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess [(100 gas)](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from false to true, after having been true in the past

### DBR.sol
```
24.        mapping (address => bool) public minters;
25.        mapping (address => bool) public markets;    
```
## 4.USE `BYTES32` INSTEAD OF STRING TO SAVE GAS WHENEVER POSSIBLE

From [solidity docs](https://docs.soliditylang.org/en/develop/types.html#dynamically-sized-byte-array).
>>If you can limit the length to a certain number of bytes, always use one of bytes1 to bytes32 because they are much cheaper.

If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity. Basically, any fixed size variable in solidity is cheaper than variable size. That will save gas on the contract.
### [DBR.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L32)
```
32.       string memory _name,
33.       string memory _symbol,
```