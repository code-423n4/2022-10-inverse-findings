# IMMUTABLE ADDRESSES LACK ZERO-ADDRESS CHECK

Constructors should check the address variable in constructor is not zero address.

## Proof of Concept
Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L37-L38

Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80-L82

INVEscrow.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol#L35

## Recommended Mitigation Steps
Add a zero address check for the immutable variables aforementioned.



# SETTERS SHOULD CHECK THE INPUT VALUE

Setter should check the input value, revert if it equals zero or zero address.

## Proof of Concept
Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118-L142

Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44-L61

BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L69

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53-L55

## Recommended Mitigation Steps
Add non-zero checks to the setters aforementioned.



# Better use two step gov/operator transfer pattern.

Recommend considering implementing a two step process where the gov/operator nominates an account and the nominated account needs to call an claimGov()/claimOperator() function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.

## Proof of Concept
Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L66-L69

BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26

## Recommended Mitigation Steps
Add two step process.


# Setter should emits an event.

Setters should emit an event so that Dapps can detect important changes to storage.

## Proof of Concept
Market.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L118-L197

Fed.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#L48-L78

Oracle.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L44-L61

DBR.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L53-L65

BorrowController.sol
https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol#L26-L38

## Recommended Mitigation Steps
Add event emitting to the setter.