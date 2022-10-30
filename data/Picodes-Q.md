### [NC-01] Typo

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L159
%/ -> %

### [NC-02] Typo

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L201

`The amount od DOLA to recall to the the lender.` -> `The amount of DOLA to recall to the lender.`

### [NC-03] Typo

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L146

`Collateral factor mus be set below 100%`->`Collateral factor must be set below 100%`

### [NC-04] `Fed.sol`, `takeProfit` lacks an event

No event is emitted when profits are withdrawn from a `market` [here](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L131)

### [NC-05] Typo

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L589

Th -> The
user user -> user

### [L-1] Multisig and smart wallets cannot borrow

Due to the check of `borrowAllowed`, any smart wallet (for example multisig) will not be able to `borrow` on markets with a `BorrowController`. The risk is that this layer of security ends up decreasing the overall security of users (as they won't use multisigs) or impacts the UX too much.