# Missing zero address check can set Operator to zero address

## setOperator() is missing a zero address check for _operator parameter, which could may allow operator to be mistakenly set to 0 address.

1.function setOperator(address _operator) public onlyOperator { operator = _operator; }
1.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26

### Recommended Mitigation Steps

Add a require() check for zero address:
### require(_operator != address(0), "Can't Be Zero");

File: FED.sol 

2. function changeGov(address _gov) public {
        require(msg.sender == gov, "ONLY GOV");
        gov = _gov;
    }
2. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48


3. function changeChair(address _chair) public {
        require(msg.sender == gov, "ONLY GOV");
        chair = _chair;
    }
3. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L66

===============================

# Use msg.sender instead of tx.origin to avoid phishing attack

1. function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
          if(msgSender == tx.origin) return true;
          return contractAllowlist[msgSender];
 
1. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L46

=====================================

# Add minter and remove minter can be merged into one function

1. function addMinter(address minter_) public onlyOperator {
        minters[minter_] = true;
        emit AddMinter(minter_);
    }

1. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L81

2. function removeMinter(address minter_) public onlyOperator {
        minters[minter_] = false;
        emit RemoveMinter(minter_);
2. https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L90

### Consider merging both into one function like this:

function setMinter(address minter_, bool present) public onlyOperator {
minters[minter_] = present;
}

========================================

# Use constant variables instead of magic numbers

### Constant variables instead of magic numbers can help keep the code easier to read and maintain.

### File: Market.sol

1.require(_collateralFactorBps < 10000, "Invalid collateral factor");
1.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L74

2.uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;
2.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L360

3.require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");
3.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L76

4.require(_collateralFactorBps < 10000, "Invalid collateral factor");
4.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L150

5.require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");
5.https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L173

==========================================

# Lack of Zero Address Check May Cause User Lose Fund Permanently

### File:SimpleERC20Escrow.sol

1. function pay(address recipient, uint amount) public {
        require(msg.sender == market, "ONLY MARKET");
        token.transfer(recipient, amount);
    }
1. https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/escrows/SimpleERC20Escrow.sol#L36

## Consider adding the following check:

require(recipient != address(0), "Address Can't Be Zero")

=======================

### File:GovTokenEscrow.sol

2. function pay(address recipient, uint amount) public {
        require(msg.sender == market, "ONLY MARKET");
        token.transfer(recipient, amount);
    }
2. https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/escrows/GovTokenEscrow.sol#L43

========================

### File: INVEscrow.sol

3. function pay(address recipient, uint amount) public {
        require(msg.sender == market, "ONLY MARKET");
        uint invBalance = token.balanceOf(address(this));
        if(invBalance < amount) xINV.redeemUnderlying(amount - invBalance); // we do not check return value because next call will fail if this fails anyway
        token.transfer(recipient, amount);
    }
3. https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/escrows/INVEscrow.sol#L59

==========================

# Constants should be in ALL CAPS:

### Here are some examples that the code style does not follow the best practices:

### File: Test.sol

 1. bytes public constant assertionError 
    bytes public constant arithmeticError 
    bytes public constant divisionError 
    bytes public constant enumConversionError 
    bytes public constant encodeStorageError 
    bytes public constant popError 
    bytes public constant indexOOBError 
    bytes public constant memOverflowError 
    bytes public constant zeroVarError
    bytes public constant lowLevelError 

1. https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/lib/forge-std/src/Test.sol#L458

### From #L458 to #L468

===========================

### File: DBR.sol

2.uint8 public constant decimals
2.https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/src/DBR.sol#L13

### File: Script.sol

3.Vm public constant vm 
3.https://github.com/code-423n4/2022-10-inverse/blob/d86e73034e6c9e81124cd1a763c3a7288268f1ab/lib/forge-std/src/Script.sol#L13

==============================



