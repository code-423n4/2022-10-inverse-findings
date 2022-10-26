Handle:
Draco000

Title:
Use != 0 instead of > 0 for Unsigned Integer Comparison

Vulnerability Details:
When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Use != 0 instead of > 0 for Unsigned Integer Comparison

Code:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172-L175
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161-L164


