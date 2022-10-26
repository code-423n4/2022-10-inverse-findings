## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

24 Instances:
-DBR.sol lines 174, 198, 288, 289, 304, 332, 360, 362, 172, 196, 316, 374, 376
-Fed.sol lines 91, 92, 110, 111
-Market.sol lines 395, 397, 565, 568, 534, 535, 599, 600

## ADD UNCHECKED {} FOR SUBTRACTIONS WHERE THE OPERANDS CANNOT UNDERFLOW BECAUSE OF A PREVIOUS REQUIRE() OR IF-STATEMENT

8 Instances:
-DBR.sol lines 111, 124, 137, 196
-Fed.sol lines 124
-Market.sol lines 362, 379
-INVEscrow.sol line 62


## `++I` INSTEAD OF `I++` (OR USE ASSEMBLY WHEN APPLICABLE)

Use `++i` instead of ` i++`. This is especially useful in for loops but this optimization can be used anywhere in your code. 

2 Instances:
-DBR.sol line 259
-Market.sol line 521

## USE MULTIPLE `REQUIRE()` STATMENTS INSTEAD OF `REQUIRE(EXPRESSION && EXPRESSION && ...)`

Instances:
-DBR.sol line 249
-Fed.sol 75
-Market.sol 162, 173, 184, 195, 448, 512


## USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas
