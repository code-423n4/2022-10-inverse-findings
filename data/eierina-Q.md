Contract/File Market/Market.sol

In most places where [predictEscrow()](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L292) is called, it is used to retrieve the escrow balance, all read functions where the balance is used to derive other values. If the escrow was not created yet, these functions will revert with an unhandled error. It would make sense to check the returned address for code size and return zero if no contract deployed at the address, or in the single case where the escrow is [predictEscrow()](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L292) is called to pay from the escrow, it would make sense to revert with a more specific error.

```solidity
function escrowBalanceOf(address user) internal view returns (uint256 balance) {
	IEscrow escrow = predictEscrow(user);
	return address(escrow).code.length > 0 
		? escrow.balance() 
		: 0;
}

function escrowPay(addres from, address recipient, uint amount) external {
	IEscrow escrow = predictEscrow(from);
	require(escrow != IEscrow(address(0)), "Sender has no escrow");
	
}

```
