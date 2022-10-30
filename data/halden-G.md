## [G-01] Optimizations with assembly
### [G-01.1] Use assembly for math (add, sub, mul, div)
File Market.sol: [336](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L336), [346](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L346), [360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L360), [377](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L377), [563](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L563), [583](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L583), [595](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L595), [597](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L597), [598](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L598), [606](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L606)

File DBR.sol: [122](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L122), [124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L124), [135](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L135), [137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L137), [148](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L148), [149](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L149), [287](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L287), [330](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L330)

File Oracle.sol: [87-89](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32), [44](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L87-L89),[95](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L95), [98](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L98), [121-124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L121-L124), [134](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L134), [137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L137)

File INVEscrow.sol [72](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L72)

### [G-01.2] Use assembly to check for address(0)
File Market.sol: [236](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L236), [246](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246), [391](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L391), [448](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448), [512](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512)

### [G-01.3] Use assembly to store storage values
File Market.sol: [77-89](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L77-L89), [118](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118), [118](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118), [124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L124), [130](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130), [136](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L136), [142](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L142), [151](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L151), [163](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L163), [174](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L174), [185](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L185), [196](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L196), [218](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L218), [249](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L249), [395](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395), [397](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397), [534-535](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534-L535), [565](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565), [568](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568), [599-600](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L599-L600)

File DBR.sol: [54](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L54), [64](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L64), [72-73](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L72-L73), [82](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L82), [91](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L91), [100](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L100), [159](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L159), [250](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L250), [288-290](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288-L290), [304](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304), [316](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316), [332](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332), [360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360), [363](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L363), [374](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374), [376](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L376)

File BorrowController.sol: [14](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L14), [26](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26), [32](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L32), [38](https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L38)

File Oracle.sol: [32](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L32),[44](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44),[53](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L53), [61](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L61),[68-69](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L68-L69),  [127](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L127)

File: Fed.sol: [37-41](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L77),[50](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L37-L41),[59](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L59),[68](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L68),[77](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L77), [91-92](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91-L92), [110-111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110-L111)

File SimpleERC20Escrow.sol: [27-28](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol#L27-L28)

File INVEscrow.sol [46-48](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L46-L48)

File GovTokenEscrow.sol: [32-34](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol#L32-L34)

## [G-02] Cache storage values in memory to minimize SLOADs
`escrows[user]` 
File Market.sol: [246](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L246)

File DBR.sol: [71-74](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L71-L74)
File Oracle.sol: [67-70](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L67-L70)

```       
require(msg.sender == pendingOperator, "ONLY PENDING OPERATOR");
operator = pendingOperator;
pendingOperator = address(0);
emit ChangeOperator(operator);
```
to
```
address newOperator = pendingOperator;
require(msg.sender == newOperator, "ONLY PENDING OPERATOR");
operator = newOperator; // minimize SLOAD : 1
pendingOperator = address(0);
emit ChangeOperator(newOperator); minimize SLOAD : 2
```

```
dueTokensAccrued[user] 
balances[user
```
File DBR.sol: [123-124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L123-L124),[136-137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L136-L137)


`fixedPrices[token]`
File Oracle.sol: [79](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L79), [113](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L113)

## [G-03] Use Add unchecked {} where the operands can not underflow/overflow because of a previous
File Market.sol: [362](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L362), [379](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L379)

File DBR.sol: [111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L111), [124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L124), [137](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L137)

File: Fed.sol: [124](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L124)

`amount - invBalance`
File INVEscrow.sol [62](https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L62)

## [G-04] X += Y or X -= Y costs more gas than  X = X + Y or X = X - Y  for state variables
File Market.sol: [395](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L236), [246](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L395), [397](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L397), [534-535](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L534-L535), [565](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L565), [568](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L568), [599](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L599), [600](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L600)


File DBR.sol: [172](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L172), [174](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L174), [196](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L196), [198](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L198), [288-289](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L288-L289), [304](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L304), [316](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L316), [332](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L332), [360](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L360), [363](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L363), [374](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L374), [376](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L376)

File: Fed.sol: [91-92](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L91-L92), [110-111](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L110-L111)

## [G-05] combine mapping variables to one mapping

```   
 mapping (address => IEscrow) public escrows; // user => escrow
    mapping (address => uint) public debts; // user => debt
    mapping(address => uint256) public nonces; // user => nonce
 ```
   
to
```
struct Account{
    IEscrow escrow,
    uint256 nonce,
    uint debt
}

mapping(address => Account) public accounts; // user => account
```

File Market.sol: [57-59](https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L57-L59)

Same in  File DBR.sol: [26-29](https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L26-L28)
```
    mapping (address => uint) public debts; // user => debt across all tracked markets
    mapping (address => uint) public dueTokensAccrued; // user => amount of due tokens accrued
    mapping (address => uint) public lastUpdated; // user => last update timestamp
```