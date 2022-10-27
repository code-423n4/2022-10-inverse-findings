1. Simplify the code :-

code snippet : -
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L212

function pauseBorrows(bool _value) public {
        if(_value) {
            require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
        } else {
            require(msg.sender == gov, "Only governance can unpause");
        }
        borrowPaused = _value;
    }
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Change to this :-

function pauseBorrows(bool _value) public {

if(msg.sender == pauseGuardian || msg.sender == gov)
{
    borrowPaused = _value;

}

else{
revert("error");
}
}
 -----------------------------------------------------------------------------------------------------------------------

Or you can change like this :-

This is simple 

function pauseBorrows(bool _value) public {
require(msg.sender == pauseGuardian || msg.sender == gov,"error");

    borrowPaused = _value;

}
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------