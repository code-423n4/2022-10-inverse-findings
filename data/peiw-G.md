- Using `PRIVATE` rather than `PUBLIC` for `CONSTANTS`, saves gas.
	- If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.
	- Instance:
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13

- Usage of UINTS/INTS smaller than 32 bytes incurs overhead.
	- When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed.
	- Instance:
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L13


- `!=` is more efficient than `>` in `require()` (6 gas less).
	- `!= 0` costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas). For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas. Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proofs: https://twitter.com/gzeon/status/1485428085885640706 Suggest changing `> 0` with `!= 0`
	- Instance:
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L83
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L117
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L63
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L328
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L592

- Splitting `REQUIRE()` statements that use `&&` saves gas.
	-Instance:
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L249
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L75
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L162
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L195
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L448
		- https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L512