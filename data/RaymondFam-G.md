## `abi.encode()` Costs More Gas Than `abi.encodePacked()`
Changing `abi.encode()` to `abi.encodePacked()` can save gas considering the former pads extra null bytes at the end of the call data, which is unnecessary. Please visit the following the link delineating how `abi.encodePacked()` is more gas efficient in general:

https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison

Here is one of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L103-L111

## Split Require Statements Using &&
Instead of using the `&&` operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per require statement will save 3 GAS per `&&`. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512

## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =). As an example, consider replacing >= with the strict counterpart > in the following line of code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L533

```
        require(debt > amount - 1, "Insufficient debt");
```
Similarly, as an example, consider replacing <= with the strict counterpart < in the following line of code:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L361

```
        if(collateralBalance < minimumCollateral + 1) return 0;
```
## Ternary Over `if ... else`
Using ternary operator instead of the if else statement saves gas. For instance the following code block may be rewritten as:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L213-L217

```
        _value 
            ? {
                require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can  pause");
               }
            : {
                require(msg.sender == gov, "Only governance can unpause");
            }
```
## External costs less gas than public visibility
Functions not internally called may have its visibility changed to external to save gas. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212

## Payable Access Control Functions Costs Less Gas
Consider marking functions with access control as `payable`. This will save 20 gas on each call by their respective permissible callers for not needing to have the compiler check for `msg.value`. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194

## Private/Internal Function Embedded Modifier Reduces Contract Size
Consider having the logic of a modifier embedded through an internal or private (if no contracts inheriting) function to reduce contract size if need be. For instance, the following instance of modifier may be rewritten as follows:

```
    function _onlyGov() private view {
        require(msg.sender == gov, "Only gov can call this function");
    }

    modifier onlyGov() {
        _onlyGov();
        _;
    }
```
## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
  solidity: {
    version: "0.8.11",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI

Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## `uint256` Can be Cheaper Than `uint8` and Other Unsigned Integer Type of Smaller Bit Size
When dealing with function arguments or memory values, there is no inherent benefit because the compiler does not pack these values. Your contract’s gas usage may be higher because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. The EVM needs to properly enforce the limits of this smaller type.

It is only more efficient when you can pack variables of uint8 into the same storage slot with other neighboring variables smaller than 32 bytes. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L220
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486

## `||` Costs Less Gas Than Its Equivalent `&&`
Rule of thumb: `(x && y)` is `(!(!x || !y))`

Even with the 10k Optimizer enabled: `||`, OR conditions cost less than their equivalent `&&`, AND conditions.

As an example, the following code line may be rewritten as:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249

```
            require(!(recoveredAddress == address(0) || recoveredAddress != owner), "INVALID_SIGNER");
```
## += and -= Costs More Gas
`+=` generally costs 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop. As an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304

```
        debts[user] =  debts[user] + additionalDebt;
```
Similarly, the following code line should be refactored as:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316

```
        debts[user] = debts[user] - repaidDebt;
```
## Avoid Emitting State Variable When an Equivalent Alternative is Available
The following instance could have had `msg.sender` emitted instead of `operator` to save gas. In fact, `msg.sender' instead of `pendingOperator` should be assigned to `operator` to save more gas. Here are two of the instances entailed which may have the functions unanimously refactored as follows:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L74
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L66-L71

```
    function claimOperator() public {
        require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
        operator = msg.sender;
        pendingOperator = address(0);
        emit ChangeOperator(msg.sender);
    }
```
## Use of Named Returns for Local Variables Saves Gas
You can have further advantages in term of gas cost by simply using named return values as temporary local variable. There are numerous instances entailed throughout all codebases:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L312

As an example, the following code block can be refactored as:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245
```
    function getEscrow(address user) internal returns (IEscrow) {
        if(escrows[user] != IEscrow(address(0))) return escrows[user];
        escrow = createEscrow(user);
        escrow.initialize(collateral, user);
        escrows[user] = escrow;
    }
```
## State Variables Repeatedly Read Should be Cached
SLOADs are cost 100 gas each after the 1st one whereas MLOADs/MSTOREs only incur 3 gas each. As such, storage values read multiple times should be cached in the stack memory the first time (costing only 1 SLOAD) and then re-read from this cache to avoid multiple SLOADs.

For instance, `collateralFactorBps` should be cached in the following two instances:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L359-L360
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L376-L377

The following code line should respectively be inserted before lines 359 and 376:

```
        uint256 collateralFactorBps = collateralFactorBps;
```
## Use of Modifiers for Repeated Checks
Consider using modifiers for common checks within different functions. This will result in less code duplication in the given contract and add significant readability into the code base. For instance, the first if statement of `viewPrice()` and `getPrice()` in `Oracle.sol` may have a modifier in place for the identical check:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L79
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L113

```
    modifier fixedPrices () {
        if(fixedPrices[token] > 0) return fixedPrices[token];
        _;
    }
```
## Identical Code Block Should Be Grouped Into an Internal Function
The logic block of `viewPrice()` and `getPrice()` in `Oracle.sol` are almost identical except for a minor central part. They could be grouped and refactored into an internal function as follows:

```
    function getOrViewPrice(address token, uint collateralFactorBps, bool write) external returns (uint) {
        if(fixedPrices[token] > 0) return fixedPrices[token];
        if(feeds[token].feed != IChainlinkFeed(address(0))) {
            // get price from feed
            uint price = feeds[token].feed.latestAnswer();
            require(price > 0, "Invalid feed price");
            // normalize price
            uint8 feedDecimals = feeds[token].feed.decimals();
            uint8 tokenDecimals = feeds[token].tokenDecimals;
            uint8 decimals = 36 - feedDecimals - tokenDecimals;
            uint normalizedPrice = price * (10 ** decimals);
            // potentially store price as today's low
            uint day = block.timestamp / 1 days;
            uint todaysLow = dailyLows[token][day];
            if (write) {
                if(todaysLow == 0 || normalizedPrice < todaysLow) {
                    dailyLows[token][day] = normalizedPrice;
                    todaysLow = normalizedPrice;
                    emit RecordDailyLow(token, normalizedPrice);
                }
            }
            // get yesterday's low
            uint yesterdaysLow = dailyLows[token][day - 1];
            // calculate new borrowing power based on collateral factor
            uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;
            uint twoDayLow = todaysLow > yesterdaysLow && yesterdaysLow > 0 ? yesterdaysLow : todaysLow;
            if(twoDayLow > 0 && newBorrowingPower > twoDayLow) {
                uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
                return dampenedPrice < normalizedPrice ? dampenedPrice: normalizedPrice;
            }
            return normalizedPrice;

        }
        revert("Price not found");
    }
```
`viewPrice()` and `getPrice()` may respectively be refactored as:

```
    function viewPrice(address token, uint collateralFactorBps) external view returns (uint) {
        getOrViewPrice (token, collateralFactorBps, false);
    }
``` 
```
    function getPrice(address token, uint collateralFactorBps) external view returns (uint) {
        getOrViewPrice (token, collateralFactorBps, true);
    }
``` 
## Return Value Check
It is a good practice to allow an anticipated failed transaction to transpire as early as possible to avoid any unnecessary wastage of gas. This would object to the following line of code commented thereupon:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62   

## Functions of Similar Logic Could be Merged
The access controlled `allow()` and `deny()` in BorrowController.sol could be merged into and replaced by one single function as refactored below:

```
    function allowOrDeny(address contract, bool auth)  public onlyOperator { contractAllowlist[allowedContract] = auth; }
```
## Unchecked SafeMath Saves Gas
"Checked" math, which is default in ^0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. When no arithmetic overflow/underflow is going to happen, `unchecked { ... }` to use the previous wrapping behavior further saves gas.

For instance, the following code block could be refactored as:

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395-L397
 
```
    unchecked { 
        debts[borrower] += amount;
        require(credit >= debts[borrower], "Exceeded credit limit");
        totalDebt += amount;
    }
```