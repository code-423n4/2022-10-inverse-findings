Issue 1:

Typos, There are 4 instances of this issue:

1- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172

Error: setReplenismentIncentiveBps
correct: setReplenishmentIncentiveBps. (Missing an h letter)


2-https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L201

Error: @param amount The amount od DOLA to recall to the the lender.
correct: @param amount The amount of DOLA to recall to the lender. (od, the the)

3-https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L128

Error: @notice Get the DBR deficit of an address. Will return 0 if th user has zero DBR or more.
correct: @notice Get the DBR deficit of an address. Will return 0 if the user has zero DBR or more. (Missing e letter)

4-https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L129

Error: @dev The deficit of a user is calculated as the difference between the user's accrued DBR deb + due DBR debt and their balance.
correct:  @dev The deficit of a user is calculated as the difference between the user's accrued DBR debt + due DBR debt and their balance. (Missing t).

Issue 2:

Consider Using more recent version of solidity (0.8.13 is not recommended for deployment) like 0.8.16.




