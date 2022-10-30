- use onlyGov modifier. It is used in 3 functions. Reduces deployment size.
    https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol/#L49
    
    ```solidity
    function changeGov(address _gov) public {
        require(msg.sender == gov, "ONLY GOV");
    ```
    
- All BorrowController functions can be external. function call will then use [less gas](https://ethereum.stackexchange.com/a/19391/105402)
    https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol/#L26
    
    ```solidity
    function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
    ```