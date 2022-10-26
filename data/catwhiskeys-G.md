# 1) Require string is too long
Require strings cost more gas to deploy if the string is larger than 32 bytes. 
Instance:
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76

Recommended mitigation steps:
Write the error strings so that they do not exceed 32 bytes. 