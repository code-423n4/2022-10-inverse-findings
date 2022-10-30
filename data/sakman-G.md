## 1. Use multiple `require`s instead of a single one with `&&`

_src/DBR.sol:_ [L249](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L249)

_src/Market.sol:_ [L75](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L75)
[L162](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L162)
[L173](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L173)
[L195](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L195)
[L512](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L512)

## 2. Custom error are cheaper than string messages

_src/escrows/GovTokenEscrow.sol:_ [L31](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L31)
[L44](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/GovTokenEscrow.sol#L44)

_src/Oracle.sol:_ [L36](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L36)
[L83](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L83)
[L104](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L104)
[L117](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L117)
[L143](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L143)

_src/Fed.sol:_ [L49](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L49)
[L58](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L58)
[L67](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L67)
[L76](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L76)
[L88](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L88)
[L104](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L104)
[L107](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L107)

_src/escrows/SimpleERC20Escrow.sol:_ [L26](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L26)
[L37](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/SimpleERC20Escrow.sol#L37)

_src/escrows/INVEscrow.sol:_ [L45](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L45)
[L60](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L60)

_src/Market.sol:_ [L74](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L74)
[L75](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L75)
[L93](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L93)
[L162](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L162)
[L184](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L184)
[L204](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L204)
[L214](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L214)
[L236](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L236)
[L390](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L390)
[L423](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L423)
[L448](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L448)
[L462](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L462)
[L487](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L487)
[L512](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L512)
[L533](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L533)
[L561](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L561)
[L567](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L567)
[L592](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L592)
[L595](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L595)

_src/DBR.sol:_ [L45](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L45)
[L71](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L71)
[L171](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L171)
[L195](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L195)
[L224](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L224)
[L301](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L301)
[L303](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L303)
[L314](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L314)
[L328](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L328)
[L329](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L329)
[L350](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L350)

_src/BorrowController.sol:_ [L18](https://github.com/code-423n4/2022-10-inverse/tree/main/src/BorrowController.sol#L18)

## 3. Strings messages in `require` statements should not be longer than 32 bytes of length

_src/Market.sol:_ [L76](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L76)
[L214](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L214)

_src/DBR.sol:_ [L63](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L63)
[L326](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L326)

## 4. use x = x + y instead of x+= y

_src/Market.sol:_ [L397](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L397)
[L568](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L568)
[L395](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L395)
[L534](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L534)
[L565](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L565)
[L599](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L599)

_src/Fed.sol:_ [L111](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L111)
[L91](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L91)
[L110](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L110)

_src/DBR.sol:_ [L360](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L360)
[L174](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L174)
[L196](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L196)
[L362](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L362)
[L374](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L374)
[L304](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L304)
[L316](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L316)
[L332](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L332)
[L288](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L288)

## 5. `public` storage `constant`(and `immutable`) variable should be `private`
### saves tons of gas on deployment

_src/DBR.sol:_ [L13](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L13)

## 6. Use 1 and 2 for true and false

_src/Market.sol:_ [L52](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L52)
[L53](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L53)

## 7. You can make state variables take less slots than they currently are

_src/Market.sol:_ [L38](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L38)

## 8. Use `x < y + 1` in stead of `x <= y`

_src/Fed.sol:_ [L93](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L93)
[L107](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L107)
[L123](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L123)

_src/DBR.sol:_ [L195](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L195)
[L224](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L224)
[L329](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L329)
[L373](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L373)

_src/Market.sol:_ [L162](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L162)
[L361](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L361)
[L396](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L396)
[L423](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L423)
[L462](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L462)
[L487](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L487)
[L562](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L562)
[L567](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L567)
[L595](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L595)

## 9. When comparing variables of type `uint`, use `require(x != 0)` instead of `require(x > 0)`

_src/Oracle.sol:_ [L83](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L83)
[L96](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L96)
[L97](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L97)
[L117](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L117)
[L135](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Oracle.sol#L135)

_src/escrows/INVEscrow.sol:_ [L81](https://github.com/code-423n4/2022-10-inverse/tree/main/src/escrows/INVEscrow.sol#L81)

_src/DBR.sol:_ [L63](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L63)
[L328](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L328)

_src/Market.sol:_ [L75](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L75)
[L162](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L162)
[L173](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L173)
[L195](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L195)
[L592](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Market.sol#L592)

_src/Fed.sol:_ [L133](https://github.com/code-423n4/2022-10-inverse/tree/main/src/Fed.sol#L133)

## 10. Use `require(x)` instead of `require(x == true)`

_src/DBR.sol:_ [L350](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L350)

## 11. Use `constant` and `immutable` for constants

_src/DBR.sol:_ [L11](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L11)
[L12](https://github.com/code-423n4/2022-10-inverse/tree/main/src/DBR.sol#L12)

