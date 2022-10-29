## [NAZ-G1] Zero Transfers
**Context**: [`Market.sol#L205`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L205), [`Market.sol#L280`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L280), [`Market.sol#L399`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L399), [`Market.sol#L537`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L537), [`Market.sol#L570`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L570), [`SimpleERC20Escrow.sol#L38`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L38), [`INVEscrow.sol#L63`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L63), [`GovTokenEscrow.sol#L45`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L45)

**Description**:
Checking non-zero transfer values can avoid an external call to save gas. Checking if amount > 0 before making the external call to erc20 transfer can save gas by avoiding the external call in such situations.

**Recommendation**: 
Consider checking for zero amount before performing a transfer. Will save a tx where the target ends up getting nothing.


## [NAZ-G2] Use Assembly To Set Storage Variables 
**Context**: [`Market.sol#L118`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118), [`Market.sol#L124`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124), [`Market.sol#L136`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136), [`Market.sol#L142`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142), [`Market.sol#L151`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L151), [`Market.sol#L161`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L161), [`Market.sol#L172`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L172), [`Market.sol#L183`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L183), [`Market.sol#L194`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L194), [`Market.sol#L212`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212), [`DBR.sol#L53`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53), [`DBR.sol#L62`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L62), [`BorrowController.sol#L26`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26), [`Oracle.sol#L44`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44), [`Fed.sol#L48`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48), [`Fed.sol#L57`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L57), [`Fed.sol#L66`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66), [`Fed.sol#L75`](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L75)

**Description**:
Using assembly to set storage variable is cheaper in gas than setting it normally. This also doesn't degrade readability for the code is still self-explanatory EX:
```js
contract SetExpensive {
    uint256 public set;

    // ~22_520 gas
    function setIt(uint256 _set) public {
        set = _set;
    }
}

contract SetOptimized {
    uint256 public set;
    // ~22_512 gas
    function setIt(uint256 _set) public {
        assembly {
            sstore(set.slot, _set)
        }
    }
}
// ~8 gas cheaper
```

**Recommendation**: 
Consider using assembly to set storage variables.