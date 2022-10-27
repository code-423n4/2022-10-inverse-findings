## Mark storage variables as `immutable` if they never change after contract initialization.
### Lines
- DBR.sol:11
- DBR.sol:12

## Use assembly to write storage values
### Lines
- Fed.sol:77
- Fed.sol:40
- Fed.sol:41
- Fed.sol:39
- Fed.sol:59
- Fed.sol:50
- Fed.sol:68
- DBR.sol:36
- DBR.sol:39
- DBR.sol:73
- DBR.sol:72
- DBR.sol:37
- DBR.sol:64
- DBR.sol:38
- DBR.sol:54
- Oracle.sol:32
- Oracle.sol:44
- Oracle.sol:69
- Oracle.sol:68
- BorrowController.sol:14
- BorrowController.sol:26
- Market.sol:218
- Market.sol:77
- Market.sol:84
- Market.sol:79
- Market.sol:136
- Market.sol:163
- Market.sol:130
- Market.sol:151
- Market.sol:78
- Market.sol:174
- Market.sol:185
- Market.sol:196
- Market.sol:86
- Market.sol:85
- Market.sol:142

G-3 Use multiple require() statments insted of require(expression && expression && ...)
### Lines
- DBR.sol:249
- Market.sol:512
- Market.sol:184
- Market.sol:75
- Market.sol:173
- Market.sol:162
- Market.sol:195
- Market.sol:448

## Use `calldata` instead of `memory` for function arguments that do not get mutated.
### Lines
- DBR.sol:32
- DBR.sol:33

## `unchecked{++i}` instead of `i++` (or use assembly when applicable)
### Lines
- DBR.sol:259
- DBR.sol:239
- Market.sol:521
- Market.sol:438
- Market.sol:502

## Use custom errors instead of string error messages
### Lines
- Fed.sol:105
- Fed.sol:88
- Fed.sol:87
- Fed.sol:89
- Fed.sol:107
- Fed.sol:58
- Fed.sol:67
- Fed.sol:49
- Fed.sol:76
- Fed.sol:104
- DBR.sol:314
- DBR.sol:326
- DBR.sol:329
- DBR.sol:224
- DBR.sol:45
- DBR.sol:328
- DBR.sol:171
- DBR.sol:373
- DBR.sol:249
- DBR.sol:303
- DBR.sol:71
- DBR.sol:63
- DBR.sol:301
- DBR.sol:350
- DBR.sol:195
- Oracle.sol:36
- Oracle.sol:83
- Oracle.sol:117
- Oracle.sol:67
- BorrowController.sol:18
- Market.sol:592
- Market.sol:392
- Market.sol:74
- Market.sol:214
- Market.sol:75
- Market.sol:423
- Market.sol:150
- Market.sol:184
- Market.sol:462
- Market.sol:173
- Market.sol:76
- Market.sol:195
- Market.sol:396
- Market.sol:236
- Market.sol:390
- Market.sol:448
- Market.sol:567
- Market.sol:595
- Market.sol:561
- Market.sol:562
- Market.sol:512
- Market.sol:204
- Market.sol:533
- Market.sol:162
- Market.sol:93
- Market.sol:487
- Market.sol:216
- Market.sol:594




