# Report
## Low Risk ##

### [L-01]: Missing checks for address(0x0)
**Context:** 

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L33
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L34
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L47
4. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L48
5. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L28
6. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L39
7. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L40
8. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L50
9. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L68
10. https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
11. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L39
12. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L54
13. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77
14. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L78
15. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L79
16. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80
17. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
18. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
19. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142

**Recommendation:**

Add non-zero address checks when set address state variables.

## Non-Critical Issues ##

### [N-01]: Wrong order of functions
**Context:** 

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L266 (internal functions must be after all external and public functions)
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101 (internal functions must be after all external and public functions)
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L226 (internal functions must be after all external and public functions)
4. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L245 (internal functions must be after all external and public functions)
5. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L323 (internal functions must be after all external and public functions)
6. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L344 (internal functions must be after all external and public functions)
7. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L353 (internal functions must be after all external and public functions)
8. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L389 (internal functions must be after all external and public functions)
9. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L460 (internal functions must be after all external and public functions)

**Description:**

According [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) functions should be grouped according to their visibility and ordered:

+ constructor

+ receive function (if exists)

+ fallback function (if exists)

+ external

+ public

+ internal

+ private

**Recommendation:**

Put the functions in the correct order according to the documentation.


### [N-02]: Public functions can be external
**Context:** 

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L30
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L43
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L52
4. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L66
5. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L44
6. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L59
7. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L70
8. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L79
9. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L90
10. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L25
11. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L36
12. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L45
13. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48
14. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57
15. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66
16. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75
17. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L86
18. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L103
19. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L131
20. https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26
21. https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32
22. https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38
23. https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L46
24. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53
25. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62
26. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70
27. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L81
28. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L90
29. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L99
30. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L109
31. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L146
32. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158
33. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L170
34. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L188
35. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L215
36. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L258
37. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L300
38. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L313
39. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L325
40. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L340
41. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L349
42. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
43. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
44. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
45. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136
46. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142
47. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L149
48. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161
49. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172
50. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183
51. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194
52. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L203
53. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212
54. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267
55. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L370
56. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L422
57. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L486
58. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L520
59. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L546
60. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L559
61. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L578
62. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L591

**Description:**

Public functions can be declared external if they are not called by the contract.

**Recommendation:**

Declare these functions as external instead of public.

### [N-03]: Require() statements should have descriptive reason string 

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L67
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L91
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L93

### [N-04]: NatSpec is missing

1. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L262
2. https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L266
3. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97
4. https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L101

### [N-05]: Typos
**Context:** 

1. ``` @param deniedContract The addres of the denied contract ``` [L36](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L36) (change ***addres*** to ***address***) 
2. ``` Operator role must be claimed by the new oprator. ``` [L50](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L50) (change ***oprator*** to ***operator***)
3. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
4. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
5. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
6. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
7. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
8. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
9. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
10. ``` @param newReplenishmentPriceBps_ The new replen ``` [L60](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L60) (change ***replen*** to ***replenishmentPriceBps***) 
11. ``` Will return 0 if th user has zero DBR or more. ``` [L128](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128) (change ***th*** to ***the***) 
12. ``` At 5000, 50% of of a borrower's underwater debt can be liquidated. ``` [L157](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L157) (change ***of of*** to ***of***) 
13. ``` @param _liquidationIncentiveBps The new liqudation incentive set in basis points. 1 = 0.01%  ``` [L181](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L181) (change ***liqudation*** to ***liquidation***) 
14. ``` @param amount The amount od DOLA to recall to the the lender. ``` [L201](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L201) (change ***od*** to ***of***) 
15. ``` @param amount The amount od DOLA to recall to the the lender. ``` [L201](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L201) (change ***the the*** to ***the***)