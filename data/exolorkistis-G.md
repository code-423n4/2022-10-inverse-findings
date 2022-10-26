[1] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED ``PAYABLE``

If a function modifier such as ``onlyOwner`` is used, the function will revert if a normal user tries to pay the function. Marking the function as ``payable`` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are ``CALLVALUE``(2),``DUP1``(3),``ISZERO``(3),``PUSH``2(3),``JUMPI``(10),``PUSH1``(3),``DUP1``(3),``REVERT``(0),``JUMPDEST``(1),``POP``(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

*There are 21 instances of this issue:*

```
File: 2022-10-inverse/src/BorrowController.sol

26: function setOperator(address _operator) public onlyOperator { operator = _operator; }

32: function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

38: function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38)

```
File: 2022-10-inverse/src/DBR.sol

53: function setPendingOperator(address newOperator_) public onlyOperator {

62: function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

81: function addMinter(address minter_) public onlyOperator {

90: function removeMinter(address minter_) public onlyOperator {

99: function addMarket(address market_) public onlyOperator {
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99)

```
File: 2022-10-inverse/src/Market.sol

118: function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

124: function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

130: function setGov(address _gov) public onlyGov { gov = _gov; }

136: function setLender(address _lender) public onlyGov { lender = _lender; }

142: function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }

149: function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161:  function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172: function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183: function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194: function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {
```

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194)

```
File: 2022-10-inverse/src/Oracle.sol

44: function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

53: function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

61: function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61)

[2] SPLITTING ``REQUIRE()`` STATEMENTS THAT USE ``&&`` SAVES GAS
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper.

*There are 8 instances of this issue:*

```
File:  2022-10-inverse/src/DBR.sol

249:  require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```
 [https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249)

```
File:  2022-10-inverse/src/Market.sol

75 : require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

173:  require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

448: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");

512: require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512)

[3]  USAGE OF ``UINTS/INTS`` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

*There are 13 instances of this issue*

```
File: 2022-10-inverse/src/DBR.sol

13: uint8 public constant decimals = 18;

220:  uint8 v,
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L220](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L220)

```
File: 2022-10-inverse/src/Market.sol

422: function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

486:  function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {
```

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486)

```
File:  2022-10-inverse/src/Oracle.sol

5: function decimals() external view returns (uint8);

20: uint8 tokenDecimals;

53: function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

85: uint8 feedDecimals = feeds[token].feed.decimals();

86: uint8 tokenDecimals = feeds[token].tokenDecimals;

87:  uint8 decimals = 36 - feedDecimals - tokenDecimals;

119:  uint8 feedDecimals = feeds[token].feed.decimals();

120:  uint8 tokenDecimals = feeds[token].tokenDecimals;

121:  uint8 decimals = 36 - feedDecimals - tokenDecimals;
```

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L5](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L5)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L20](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L20)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L85-L87](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L85-L87)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L119-L121](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L119-L121)

[4] DUPLICATED ``REQUIRE()``/``REVERT()`` CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

Saves deployment cost

*There are 15 instances of this issue:*

```
File: 2022-10-inverse/src/DBR.sol

301: require(markets[msg.sender],

314:  require(markets[msg.sender], 

326:  require(markets[msg.sender],

195: require(balanceOf(from) >= amount, "Insufficient balance");

373: require(balanceOf(from) >= amount, "Insufficient balance");
```

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L301](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L301)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L314](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L314)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L326)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L195)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L373](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L373)

```
File:  2022-10-inverse/src/Fed.sol

49: require(msg.sender == gov, "ONLY GOV");

58: require(msg.sender == gov, "ONLY GOV");

67: require(msg.sender == gov, "ONLY GOV");

76: require(msg.sender == chair, "ONLY CHAIR");

87: require(msg.sender == chair, "ONLY CHAIR");

104:  require(msg.sender == chair, "ONLY CHAIR");

88: require(dbr.markets(address(market)), "UNSUPPORTED MARKET");

105:  require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L58](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L58)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L88](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L88)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L105](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L105)

```
File: 2022-10-inverse/src/Oracle.sol

83: require(price > 0, "Invalid feed price");

117: require(price > 0, "Invalid feed price");
```

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L117](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L117)

[5] ``ABI.ENCODE()`` IS LESS EFFICIENT THAN ``ABI.ENCODEPACKED()``

*There are 5 instances of this issue:*

```
File: 2022-10-inverse/src/DBR.sol

232: abi.encode(

269:  abi.encode(
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L232](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L232)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L269](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L269)

```
File: 2022-10-inverse/src/Market.sol

104: abi.encode(

431:  abi.encode(

495:  abi.encode(

```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L104](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L104)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L431](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L431)

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L495](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L495)

[6] USE ASSEMBLY TO CHECK FOR ADDRESS(0) to save gas.

*There are 2 instances of this issue:*

```
File: 2022-10-inverse/src/DBR.sol

249:  require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249)

```
File: 2022-10-inverse/src/Market.sol

448:   require(recoveredAddress != address(0) && recoveredAddress == from, "INVALID_SIGNER");
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448)


[7] USING  ``PRIVATE`` RATHER THAN ``PUBLIC`` FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

*There is 1 instance of this issue:*

```
File:  2022-10-inverse/src/DBR.sol

13:  uint8 public constant decimals = 18;

```
 [https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13)


[8] STATE VARIABLES ONLY SET IN THE CONSTRUCTOR SHOULD BE DECLARED IMMUTABLE
Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) and each access thereafter (Gwarmacces - 100 gas) with a ``PUSH32`` (3 gas).

*There are 4 instances of this issue:*

```

File:  2022-10-inverse/src/DBR.sol

31  :   uint _replenishmentPriceBps,
32  :      string memory _name,
33  :    string memory _symbol,
34  :     address _operator
```
[https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L31-L34](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L31-L34)


























