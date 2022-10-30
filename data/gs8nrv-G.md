## Custom Errors

Instead of using `require` you can use customs error which are more gas friendly.

You need first to define the error by declaring it after your variables like:

```
error MyError();
``` 

And then replacing the 
```
require(condition, "message")
``` 
with 

```
if(!condition) revert MyError();
```


I am not listing every require but here are some:
-https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L45
-https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L63
-https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L71
-...


## Use Internal functions

Use internal function in presence of code duplicated. This will decrease the bytecode and therefore the deployment cost. It can be done for example for the `DBR.transfer` and `DBR.transferFrom`:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L170
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L188

You can make an internal function:

```

    function transfer(address to, uint256 amount) public virtual returns (bool) {
       _transfer(msg.sender,to, amount);
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual returns (bool) {
        uint256 allowed = allowance[from][msg.sender];
        if (allowed != type(uint256).max) allowance[from][msg.sender] = allowed - amount;
        _transfer(from,to, amount);
    }

    function _transfer(address, from,address to, uint256 amount) internal virtual returns (bool) {
        require(balanceOf(from) >= amount, "Insufficient balance");
        balances[from] -= amount;
        unchecked {
            balances[to] += amount;
        }
        emit Transfer(from, to, amount);
        return true;
    }
``` 

## Useless checks on `DBR.onforceReplenish()`

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L328
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L569
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L561

The only possibility to call this function is through `Market. forceReplenish()`(Market l.569) where the 2 checks on DBR l.328 and l.329 are already done at Market l.562 and l.563.
