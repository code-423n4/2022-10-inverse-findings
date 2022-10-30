# QA Report for Inverse Finance contest

## Overview
During the audit, 1 low and 13 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
L-1 | [Missing check for zero address](#l-1-missing-check-for-zero-address) | Low | 19
NC-1 | [Order of Functions](#nc-1-order-of-functions) | Non-Critical | 8
NC-2 | [Order of Layout](#nc-2-order-of-layout) | Non-Critical | 4
NC-3 | [Maximum line length exceeded](#nc-3-maximum-line-length-exceeded) | Non-Critical | 6
NC-4 | [Inconsistent comment location](#nc-4-inconsistent-comment-location) | Non-Critical | 1
NC-5 | [Open TODO](#nc-5-open-todo) | Non-Critical | 1
NC-6 | [No error message in ```require```](#nc-6-no-error-message-in-require) | Non-Critical | 3
NC-7 | [Comment lines are too long](#nc-7-comment-lines-are-too-long) | Non-Critical | 3
NC-8 | [Missing NatSpec](#nc-8-missing-natspec) | Non-Critical | 4
NC-9 | [Spaces between the control structures](#nc-9-spaces-between-the-control-structures) | Non-Critical | 31
NC-10 | [Typos](#nc-10-typos) | Non-Critical | 8
NC-11 | [Scientific notation may be used](#nc-11-scientific-notation-may-be-used) | Non-Critical | 22
NC-12 | [Constants may be used](#nc-12-constants-may-be-used) | Non-Critical | 1
NC-13 | [Public functions can be external](#nc-13-public-functions-can-be-external) | Non-Critical | 62

## Low Risk Findings (1)
### L-1. Missing check for zero address
##### Description
If address(0x0) is set it may cause the contract to revert or work wrong.

##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L40
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L50
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L68
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L33
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L34
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L47
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L48
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L28
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L78
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L79
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L54
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26

##### Recommendation
Add checks.

## Non-Critical Risk Findings (13)
### NC-1. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private

##### Instances
Public functions before external:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44-L71

Internal functions before public:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226-L251
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L323
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L344-L363
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L460
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L266

##### Recommendation
Reorder functions where possible.

#
### NC-2. Order of Layout
##### Description
According to [Order of Layout](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), inside each contract, library or interface, use the following order:
1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions

##### Instances
Events after functions:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L146-L147
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L140-L141
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L614-L620
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L381-L386


##### Recommendation
Place events before functions.

#
### NC-3. Maximum line length exceeded
##### Description
Some lines of code are too long.

##### Instances
- (143 symbols) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53
- (122) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
- (127) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
- (130) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360
- (131) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377
- (137) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L618

##### Recommendation
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#maximum-line-length), maximum suggested line length is 120 characters.  
Make the lines shorter.

#
### NC-4. Inconsistent comment location
##### Description
Some comments are above the line of code and some next to it.

##### Instances
Here the comments are above the line of code, although in all other contracts it is on the side of the code.
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

##### Recommendation
Use consistent comment location.

#
### NC-5. Open TODO
##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35

##### Recommendation
Resolve issues.

#
### NC-6. No error message in ```require```
##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L91

##### Recommendation
Add error messages.

#
#### NC-7. Comment lines are too long
##### Description
Comments are not fully visible on the screen.

##### Instances
- (242 symbols) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L12
- (168) https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L99
- (177) https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L16

##### Recommendation
For readability, split comments across multiple lines.

#
### NC-8. Missing NatSpec
##### Description
NatSpec is missing for 4 functions in 2 contracts.

##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L262
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L266
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101

##### Recommendation
Add NatSpec for all functions.

#
### NC-9. Spaces between the control structures
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#control-structures), there should be a single space between the control structures ```if```, ```while```, and ```for``` and the parenthetic block representing the conditional.

##### Instances
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L79
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L80
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L97
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L113
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L114
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L126
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L136
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L123
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L133
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L81
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L47
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L110
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L286
- and 15 instances in [Market.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol)

##### Recommendation
Change:
```
if(...) 
```
to:
```
if (...) 
```

#
##### NC-10. Typos
##### Instances
- [```At 5000, 50% of of a borrower's underwater debt can be liquidated.```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L157) => one ```of```
- [```@param _liquidationIncentiveBps The new liqudation incentive set in basis points. 1 = 0.01% ```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L181) => ```liquidation```
- [```@param amount The amount od DOLA to recall to the the lender.```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L201) => ```of```
- [```@param amount The amount od DOLA to recall to the the lender.```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L201) => one ```the```
- [```@notice Sets pending operator of the contract. Operator role must be claimed by the new oprator.```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50) => ```operator```
- [```@notice Removes a minter from the set of addresses allowe to mint DBR tokens.```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L87) => ```allowed```
- [```Will return 0 if th user has zero DBR or more.```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128) => ```the```
- [```@param deniedContract The addres of the denied contract```](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36) => ```address```

#
### NC-11. Scientific notation may be used
##### Description
For readability and to avoid misprints, it is better to use scientific notation.

##### Instances 
10000:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L95
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L98
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L134
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L137
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L74
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L150
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L336
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L346
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L564
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L583
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330

##### Recommendation
Replace ```10000``` with ```10e4```.

#
### NC-12. Constants may be used
##### Description
Constants may be used instead of  literal values.

##### Instances
For 10000:
- See Instances in NC-11.

##### Recommendation
Define constant variables for repeated values.

#
##### NC-13. Public functions can be external
##### Description
If functions are not called by the contract where they are defined, they can be declared external.

##### Instances
- All 7 functions in [Fed.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol) (except [this](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L120))
- All 4 functions in  [GovTokenEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol)
- All 5 functions in  [INVEscrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol)
- All 3 functions in  [SimpleERC20Escrow.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol)
- All 4 functions in  [BorrowController.sol](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol)

21 functions in Market.sol:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L370
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L520
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591

18 functions in DBR.sol:
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L215
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L340
- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L349

##### Recommendation
Make public functions external, where possible.