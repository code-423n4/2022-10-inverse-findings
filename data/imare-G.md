### [G-01] Functions guaranteed to revert when called by normal users can be marked ***payable***

If a function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are
CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

There are 35 instances of this issue:

File : BorrowController.sol

1. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26

``function setOperator(address _operator) public onlyOperator { operator = _operator; }``

2. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L32
    
    function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }``

3. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L38

``    function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }``

File : DBR.sol

4. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L53
    
``function setPendingOperator(address newOperator_) public onlyOperator {``

5. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L62

``   function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {``

6. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L70-L71

```
    function claimOperator() public {
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
```

7.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L81

``    function addMinter(address minter_) public onlyOperator {``

8. 
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L90

``   function removeMinter(address minter_) public onlyOperator {``

9.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L99

``    function addMarket(address market_) public onlyOperator {``

10.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L300-L301

```
    function onBorrow(address user, uint additionalDebt) public {
        require(markets[msg.sender], "Only markets can call onBorrow");
```

11.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L313-L314

```
    function onRepay(address user, uint repaidDebt) public {
        require(markets[msg.sender], "Only markets can call onRepay");
```

12.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L325-L326

```
    function onForceReplenish(address user, uint amount) public {
        require(markets[msg.sender], "Only markets can call onForceReplenish");
```

13.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L349-L350

```
    function mint(address to, uint amount) public {
        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

14.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L118

``    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }``

15.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L124

``  function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }``

16.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130

``  function setGov(address _gov) public onlyGov { gov = _gov; }``

17.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L136

``
function setLender(address _lender) public onlyGov { lender = _lender; }
``

18.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L142

``
function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
``    

19.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L149

``
    function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {
``

20.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L161

``function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {``

21.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L172

``
function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {``

22.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L183

``function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {``

23.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L194

``function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {``

24.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L203-L204

```
   function recall(uint amount) public {
        require(msg.sender == lender, "Only lender can recall");
```

25.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L212-L217

```
    function pauseBorrows(bool _value) public {
        if(_value) {
            require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
        } else {
            require(msg.sender == gov, "Only governance can unpause");
        }
```

File : Oracle.sol

26.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L44

``    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }``

27.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L53

``function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }
``

28.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L61

``
function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
``

29.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Oracle.sol#L66-L67
```
    function claimOperator() public {
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
```

File : Fed.sol

30.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48-L49

```
    function changeGov(address _gov) public {
        require(msg.sender == gov, "ONLY GOV");
```

31.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L57-L58

```
    function changeSupplyCeiling(uint _supplyCeiling) public {
        require(msg.sender == gov, "ONLY GOV");
```

32.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L66-L67

```
  function changeChair(address _chair) public {
        require(msg.sender == gov, "ONLY GOV");
```

33.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L75-L76

```
    function resign() public {
        require(msg.sender == chair, "ONLY CHAIR");
```

34.

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L86-L87
```
    function expansion(IMarket market, uint amount) public {
        require(msg.sender == chair, "ONLY CHAIR");
```

35.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L103-L104
```
    function contraction(IMarket market, uint amount) public {
        require(msg.sender == chair, "ONLY CHAIR");
```

