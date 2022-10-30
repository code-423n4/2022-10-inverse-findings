## USE CUSTOM ERROR INSTEAD OF STRING
Use custom error instead of string saves gas
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L45
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L71
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L171

## USING IMMUTABLE FOR STATE VARIABLE ONLY SET IN CONSTRUCTOR WILL SAVE GAS 
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L12
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L11

## USING PRIVATE FOR CONSTANT VARIBALE SAVES GAS
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13

## USAGE OF UINTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L17
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L18

## SPLITTING REQUIRE SAVES GAS
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249


