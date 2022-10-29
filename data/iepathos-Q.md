### QA-01 Recommend refactoring ONLY GOV and ONLY CHAIR require checks into modifiers for readability

In Fed.sol repeated calls to `require(msg.sender == gov, "ONLY GOV");` and `require(msg.sender == chair, "ONLY CHAIR");` are better done as modifiers like

```
    modifier onlyGov {
        require(msg.sender == gov, "ONLY GOV");
        _;
    }

    modifier onlyChair {
        require(msg.sender == chair, "ONLY CHAIR");
        _;
    }
```

And then just attaching the modifiers to the function definitions would cleanup the code and allow removal of the repeating require checks in the functions.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L49
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L58
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L67

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L76
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L87
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L104