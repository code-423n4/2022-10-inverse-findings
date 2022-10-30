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

etc