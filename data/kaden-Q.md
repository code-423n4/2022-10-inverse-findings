#### Lack of address(0) checks

##### Severity: Low

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L130
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.sol#L26
- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L48

##### Description:

In several places, setters for addresses controlling the contracts are missing important validation to ensure that `operator`/`gov` do not get unintentionally set as, e.g. `address(0)`. A human error could result in these authorization positions becoming permanently locked.

##### Remediation:

Ideally, a two-step transfer process is used for these authorization positions, but at the very least a check should be included that ensures the address being set is not `address(0)`.

#### User `debt` being decremented before token transfer could be exploited in case of reentrancy

##### Severity: Low

##### Context:

- https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L531

##### Description:

In `Market.repay`, the users `debt` gets decremented before transferring Dola to repay the debt. This can be exploited in the case that an attacker manages to reenter the function before the `transferFrom`.

It doesn't appear that there is a way to reenter the function, however, if the Inverse team decides to update `DBR.onRepay` to include a callback, this would be exploitable.

##### Recommendation:

It is recommended that the transfer of Dola is done before decrementing the users debt.