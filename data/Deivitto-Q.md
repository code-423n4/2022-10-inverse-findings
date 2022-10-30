
# Low

## ERC20 transfer / transferFrom with not checked return value
### Impact
Not every ERC20 token follows OpenZeppelin's recommendation. It's possible (inside ERC20 standard) that a `transferFrom` doesn't revert upon failure but returns false.

### Proof of Concept
- ERC20 `transfer`
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L205
        dola.transfer(msg.sender, amount);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L399
        dola.transfer(to, amount);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L570
        dola.transfer(msg.sender, replenisherReward);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L135
            dola.transfer(gov, profit);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L45
        token.transfer(recipient, amount);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/SimpleERC20Escrow.sol#L38
        token.transfer(recipient, amount);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L63
        token.transfer(recipient, amount);


- ERC20 `transferFrom`
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L280
        collateral.transferFrom(msg.sender, address(escrow), amount);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L537
        dola.transferFrom(msg.sender, address(this), amount);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L602
        dola.transferFrom(msg.sender, address(this), repaidDebt);


### Recommended Mitigation Steps
Consider using OpenZeppelin's library with safe versions of transfer functions.
Check return value / revert if needed.


## IERC20 `approve` with no return value checked and wrong interface return value
### Impact 
IERC20 `approve` as said in OZ documentation
`Returns a boolean value indicating whether the operation succeeded.`

The interface is returning a uint value what can be casted but is not the expected type. Also the return value is not being checked

### Docs
https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-approve-address-uint256-

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/INVEscrow.sol#L50
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/INVEscrow.sol#L9

### Recommended Mitigation
- Add a check on the return value
- Return expected type

## Prevent div by 0
### Impact
On several locations in the code precautions are not being taken to not divide by `0`, this would revert the code.

### Proof of Concept
Navigate to the following contracts,
- collateralFactorBps is user input many times, others can be wrongly assigned to 0 but no checks are done.

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L360
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;//

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L377
        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

- If oracle fails it will revert
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L597
        uint liquidatorReward = repaidDebt * 1 ether / price;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L606
            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L98
                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L137
                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;


### Recommended Mitigation Steps
Recommend making sure division by `0` won’t occur by checking the variables beforehand and handling this edge case.

## Missing event for core paths/functions/parameters
### Summary
Events are important for critical parameter change and some paths of the code where privileges, ether/tokens are involved.
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L196
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L163
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L151
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L174
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L185
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L64
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/BorrowController.sol#L27
### Mitigation
Emit events for this attributes


## Use of hardcoded amount of days
### Summary 
Formula uses a hardcoded value of `365` (days) which would be wrong applied in a leap year (`366` days)
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L122
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L135
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L148
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L287
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;


### Mitigation
Consider using an oracle for this
Consider using a method that change the value between `365` and `366` for the operations in leap years and regular years



## Missing checks for address(0x0) when assigning values to `address` state or `immutable` variables 
### Summary
Zero address should be checked for state variables, immutable variables. A zero address can lead into problems.
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/BorrowController.sol#L13-L15
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L39
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/Fed.sol#L36-L40
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/INVEscrow.sol#L33-L36
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L29-L33
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L62-L68
### Mitigation
Check zero address before assigning or using it





## Missing checks for address(0x0) on different functions
### Summary
Zero address should be checked for some function parameters. For example in functions like role assignments, mints, withdrawals... 

A zero address can lead into serious problems as locking eth or correct functioning.

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L53
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/BorrowController.sol#L26
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L66
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L48
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L44
### Mitigation
Check zero address before assigning or using it


## block.timestamp used as time proxy 
### Summary
Risk of using `block.timestamp` for time should be considered. 
### Details
`block.timestamp` is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
- Used with a strict comparison, hardly not recommended as block.timestamp can be manipulated
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L286
        if(lastUpdated[user] == block.timestamp) return;

- Other uses of timestamp as time proxies
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L284-L292

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L423
        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L487
        require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L122
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L135
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L148
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L224
        require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L287
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L290
        lastUpdated[user] = block.timestamp;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L89
            uint day = block.timestamp / 1 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L124
            uint day = block.timestamp / 1 days;


### Mitigation
- Consider the risk of using `block.timestamp` as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision


## Front run initializer
### Summary
The initialize function that initializes important contract state can be called by anyone.
### Details
The attacker can initialize the contract before the legitimate deployer, hoping that the victim continues to use the same contract.

In the best case for the victim, they notice it and have to redeploy their contract costing gas.

### Github Permalinks


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L17
    function initialize(IERC20 _token, address beneficiary) external;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L30
    function initialize(IERC20 _token, address _beneficiary) public {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/SimpleERC20Escrow.sol#L25
    function initialize(IERC20 _token, address) public {

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L44
    function initialize(IERC20 _token, address _beneficiary) public {


### Mitigation
Use the constructor to initialize non-proxied contracts.

For initializing proxy contracts deploy contracts using a factory contract that immediately calls initialize after deployment or make sure to call it immediately after deployment and verify the transaction succeeded.



## Different versions of pragma
### Summary
Some of the contracts include an unlocked pragma, e.g., pragma solidity >=0.8.13. 

Locking the pragma helps ensure that contracts are not accidentally deployed using an old compiler version with unfixed bugs.

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/SimpleERC20Escrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol

### Mitigation
Lock pragmas to a specific Solidity version. 
Consider converting >= 0.8.13 into 0.8.13
Consider converting ^ 0.8.13 into 0.8.13

## Authentication system in `borrowAllowed` can be overpassed easily
### Impact 
msgSender == tx.origin with user input in public function is easy to overpass.
However, it only returns if an user is allowed or not, so doesn't seem harmful, but in the @dev comment says, "currently".

This check `msgSender == tx.origin` is more expected as `msg.sender == tx.origin` if the objective is to check that is not being called by a contract than `userInputAddress == tx.origin`. 

If in the future of that "currently" it does something important to the system and the check is for not being a contract, this can be easily exploited as
- User with address Alice calls contract Bob
- contract Bob calls borrowAllowed(Alice_address, address_not_used, uint_not_used)
- tx.origin is Alice
- msgSender is Alice_address that is Alice
- comparison would be passed
- some_future_exploit()?
- profit of attacker

```
    /**
    @notice Checks if a borrow is allowed
    @dev Currently the borrowController only checks if contracts are part of an allow list //@audit says currently, this means it can be used in different scenario in the future or with different logic
    @param msgSender The message sender trying to borrow
    @return A boolean that is true if borrowing is allowed and false if not.
    */
function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
        if(msgSender == tx.origin) return true;
        return contractAllowlist[msgSender];
}
```

Piece of code where is actually used in `Market.sol#borrowAllowed()`
```
if(borrowController != IBorrowController(address(0))) {
        require(borrowController.borrowAllowed(msg.sender, borrower, amount), "Denied by borrow controller");
}
```

So actually it's being used msg.sender here, I don't know what's expected to happen in the future, but it's safer (and needs a little different implementation) using `msg.sender == tx.origin`.

### Github Permalink
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/BorrowController.sol#L40-L49
### Mitigation
Consider what's is going to do that function and add the checks / change the implementation as needed.



# Informational
## Missing initializer modifier on constructor
### Impact
OpenZeppelin [recommends](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5) that the initializer modifier be applied to constructors in order to avoid potential griefs, [social engineering](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/4), or exploits. 

### Github Permalink
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/INVEscrow.sol#L34-L36

### Mitigation
Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.

## Comparison with a a boolean
### Summary
There are a number of instances where a boolean variable/function is checked.
### Details
- This check can be further simplified from `variable == true` to `variable`.

### Github Permalink
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L350
        require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");

### Mitigation
Simplify boolean comparisons in order to improve readability and save gas



## Naming convention of constants
### Summary
Constant naming convention is all upper case. 
### Details
Some constants are not using proper style.
Constant should be in `UPPER_CASE_WITH_UNDERSCORES` as per Solidity Style Guide. 
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L13

### Mitigation
Rename the constant to uppercase style: `CONSTANTS_WITH_UNDERSCORES`. 




## Naming convention of state variable non constant
### Summary
Only constants are suggested to use style `CONSTANTS_WITH_UNDERSCORES`, other variables are suggested to use `camelCase`
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L21-L22
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L55-L56
### Mitigation
Rename to `camelCase`



## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots
### Details
- Events are declared the last of all
### Github Permalink
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L380-L387
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/Fed.sol#L138-L141
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L614-L620
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L146-L147


### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.15/style-guide.html




## Missing Natspec 
### Summary 
Missing Natspec and regular comments affect readability and maintainability of a codebase. 
### Details 
Contracts has partial or full lack of comments
### Github Permalinks 
#### Natspec @param
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/INVEscrow.sol#L34-L36
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L30-L42
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/BorrowController.sol#L13-L15
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/Fed.sol#L36-L42
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L61-L90
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L29-L33
#### Natspec @return value
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L287-L380
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L221-L251
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L262-L277

 ### mitigation
 - Add @param descriptors
 - Add @return descriptors
 - Add Natspec comments. 
 - Add inline comments. 
 - Add comments for what the contract does



## Open TODOs   
### Summary
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
### Details
The code includes a `TODO` that affects readability and focus on the readers/auditors of the contracts
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L35
        xINV = _xINV; // TODO: Test whether an immutable variable will persist across proxies

### Mitigation
Remove already done `TODO`


## Commented out code
### Summary
There is some code that is commented out. It could point to items that are not done or need redesigning, be a mistake, or just be testing overhead.

### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/SimpleERC20Escrow.sol#L50-L52

### Mitigation
Review and remove or resolve/document the commented out lines if needed.


## Maximum line length exceeded
### Summary
Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details 
Lines that exceed the 79 (or 99) character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length
### Github Permalinks 


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L33

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L75

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L76

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L98

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L105

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L122

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L124

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L133

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L139

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L142

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L145

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L156

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L157

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L162

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L168

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L170

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L173

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L179

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L184

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L190

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L195

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L209

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L210

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L214

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L233

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L289

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L300

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L309

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L315

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L319

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L326

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L341

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L350

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L360

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L367

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L377

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L392

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L413

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L414

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L422

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L433

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L477

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L478

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L486

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L497

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L518

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L526

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L555

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L587

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L618

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L619

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/BorrowController.sol#L32

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/BorrowController.sol#L38

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L6

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L7

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L50

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L58

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L78

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L87

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L96

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L106

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L116

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L129

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L181

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L234

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L263

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L270

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L320

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L321

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L345

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L11

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L12

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L13

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L14

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L44

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L48

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L53

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L57

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L59

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L61

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L96

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L108

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L135

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L23

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L82

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L98

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L99

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L16

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L50

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L56

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/SimpleERC20Escrow.sol#L49

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L25

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L62

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L68

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L72

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L82


### Mitigation
Reduce line length to less than 99 at least to improve maintainability and readability of the code 


## Missing error messages in require statements
### Summary
Require/revert statements should include error messages in order to help at monitoring the system.
### Github Permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Fed.sol#L93
        require(globalSupply <= supplyCeiling);


https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/GovTokenEscrow.sol#L67
        require(msg.sender == beneficiary);

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/escrows/INVEscrow.sol#L91
        require(msg.sender == beneficiary);

### Mitigation
Add error messages 

##  Use of magic numbers is confusing and risky
### Summary
Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
### Details
Values are hardcoded and would be more readable and maintainable if declared as a constant


### Github Permalinks
10000, 5000, 1 ether, 365 days
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L51
    uint public liquidationFactorBps = 5000; // 50% by default

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L74
        require(_collateralFactorBps < 10000, "Invalid collateral factor");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L75
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L76
        require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L150
        require(_collateralFactorBps < 10000, "Invalid collateral factor");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L162
        require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L173
        require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L184
        require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L195
        require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L336
        return collateralValue * collateralFactorBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L346
        return collateralValue * collateralFactorBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L360
        uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L377
        uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L563
        uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L564
        uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L583
        return debt * liquidationFactorBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L595
        require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L598
        liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L606
            uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L330
        uint replenishmentCost = amount * replenishmentPriceBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L95
            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L98
                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L134
            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Oracle.sol#L137
                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L122
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L135
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L148
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;

https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/DBR.sol#L287
        uint accrued = (block.timestamp - lastUpdated[user]) * debt / 365 days;


### Mitigation
Replace magic hardcoded numbers with declared constants.

## Inconsistent way of declaring mappings
### Impact
Incosistent way of declaring mappings affects searchability of the code and therefore maintainability
### Details
Sometimes using `mapping(`
sometimes using `mapping (`
### Github permalinks
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L57-L59
- - - - -
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L23-L28
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/DBR.sol#L19-L20
### Mitigation
Use one way of declaring them but not both

