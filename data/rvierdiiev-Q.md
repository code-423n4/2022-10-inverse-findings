
1.	For the safety reasons it will be good to check if `_escrowImplementation.token == _collateral`. In this way you sure, that the escrow works with same token as market.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L80

2.	`Market. DOMAIN_SEPARATOR` function can’t be called from another chain. So `computeDomainSeparator()` function will never be called.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L97-L99

3.	Ownership of Market contract can be lost.
`Market.gov` address is very important in the contract as it can call functions that change Market behaviour. However it’s not secured from changing this address to wrong account or 0. If gov will be lost, then you can’t adjust Market anymore. And one more important thing is that if you pause Market then it’s only the gov who can unpause it.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L130
I suggest to add 2 step ownership changing like it is done in DBR.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L70-L75
I would also recommend to use same flow everywhere.   

4.	`Market. setReplenismentIncentiveBps` says `@dev Must be set between 1 and 10000.`, but it’s not possible to set 10000.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L173
Also, though it’s not allowed to provide 0 in setter, you can do that in constructor.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L76

5.	`Market. setLiquidationIncentiveBps` says `@dev Must be set between 0 and 10000 - liquidation fee `, but possible values are 1-9999-liquidation fee.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184

6.	`Market. setLiquidationFeeBps` says `@dev Must be set between 0 and 10000 - liquidation factor`, but possible values are 1-9999-liquidation factor.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L184

7.	In `Market.liquidate` there is fee transfer to the protocol in case when fee is switched on. In case if escrow has balance less then calculated fee, then the fee transfer is skipped. I propose in this case to send all balance of escrow.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605-L610

8.	In `Oracle.setFeed` it’s possible to make mistake and provide incorrect token decimals. I propose instead call token.decimals() to get the 100% correct value.
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#L605-L610  