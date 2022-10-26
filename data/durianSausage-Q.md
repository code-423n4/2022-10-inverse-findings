### N01: NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES
DBR.sol, 2, b'pragma solidity ^0.8.13;'
BorrowController.sol, 2, b'pragma solidity ^0.8.13;'
escrows/SimpleERC20Escrow.sol, 2, b'pragma solidity ^0.8.13;'
escrows/GovTokenEscrow.sol, 2, b'pragma solidity ^0.8.13;'
Fed.sol, 2, b'pragma solidity ^0.8.13;'
Oracle.sol, 2, b'pragma solidity ^0.8.13;'
escrows/INVEscrow.sol, 2, b'pragma solidity ^0.8.13;'
Market.sol, 2, b'pragma solidity ^0.8.13;'


### L01:_SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
#### problem
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### prof
DBR.sol, 333, b'        _mint(user, amount);'
DBR.sol, 351, b'        _mint(to, amount);'

### L02: RETURN VALUES OF approve() NOT CHECKED
#### prof
approve() NOT CHECKED
Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment
#### problem
escrows/INVEscrow.sol, 50, b'        _token.approve(address(xINV), type(uint).max);'

### L03: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
DBR.sol, 39, b'        operator = _operator;'
DBR.sol, 54, b'        pendingOperator = newOperator_;'
DBR.sol, 72, b'        operator = pendingOperator;'
DBR.sol, 73, b'        pendingOperator = address(0);'
BorrowController.sol, 14, b'        operator = _operator;'
BorrowController.sol, 26, b'    function setOperator(address _operator) public onlyOperator { operator = _operator; }'
escrows/SimpleERC20Escrow.sol, 27, b'        market = msg.sender;'
escrows/SimpleERC20Escrow.sol, 28, b'        token = _token;'
escrows/GovTokenEscrow.sol, 32, b'        market = msg.sender;'
escrows/GovTokenEscrow.sol, 33, b'        token = _token;'
escrows/GovTokenEscrow.sol, 34, b'        beneficiary = _beneficiary;'
Fed.sol, 37, b'        dbr = _dbr;'
Fed.sol, 38, b'        dola = _dola;'
Fed.sol, 39, b'        gov = _gov;'
Fed.sol, 40, b'        chair = _chair;'
Fed.sol, 50, b'        gov = _gov;'
Fed.sol, 68, b'        chair = _chair;'
Fed.sol, 77, b'        chair = address(0);'
Oracle.sol, 32, b'        operator = _operator;'
Oracle.sol, 44, b'    function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }'
Oracle.sol, 68, b'        operator = pendingOperator;'
Oracle.sol, 69, b'        pendingOperator = address(0);'
escrows/INVEscrow.sol, 35, b'        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies'
escrows/INVEscrow.sol, 46, b'        market = msg.sender;'
escrows/INVEscrow.sol, 47, b'        token = _token;'
escrows/INVEscrow.sol, 48, b'        beneficiary = _beneficiary;'
Market.sol, 77, b'        gov = _gov;'
Market.sol, 78, b'        lender = _lender;'
Market.sol, 79, b'        pauseGuardian = _pauseGuardian;'
Market.sol, 80, b'        escrowImplementation = _escrowImplementation;'
Market.sol, 81, b'        dbr = _dbr;'
Market.sol, 82, b'        collateral = _collateral;'
Market.sol, 83, b'        oracle = _oracle;'
Market.sol, 118, b'    function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }'
Market.sol, 124, b'    function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }'
Market.sol, 130, b'    function setGov(address _gov) public onlyGov { gov = _gov; }'
Market.sol, 136, b'    function setLender(address _lender) public onlyGov { lender = _lender; }'
Market.sol, 142, b'    function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }'
