## Title

Public functions should be replace external or internal.

### Details

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L267
and more...

Public functions not called by the same contract should change external. And I recommend external or internal instead of public because it should make function caller clearer.