# table of contents 
1.  latestAnswer` oracle  is deprecated 
2.  no address  zero checks 
3.  `liqaudtionfactorbps` can be set to 10_000 unlike what the comment says
4.  in the future if the lender is someone they can steal funds from market
5.  make internal functions last in the file and put public in front
6.  getEscrow should be renamed to setEscrow  because its state changing  function
7.  if an market contract has callback for minting but there are using the governace contract it will revert and cause issues so make sure that callback is either deprecated or have flags on the deposit function, to  call the escrow `onDeposit()`
8.   you should make deposit paused when the borrow is paused because that can cause loss of funds there is no pausing mechanism 
9.   as best practice dont use tx.origin the user can be opened up to phishing attacks
10.   some parameters for market can make the main functions not working like `collateralFactorbps `
11.   if `replenishmentIncentiveBps` gets set to 0  which in the comments say isnt possible
***

####     `latestAnswer` oracle  is deprecated 

```
    if (feeds[token].feed != IChainlinkFeed(address(0))) {
            // get price from feed
            uint256 price = feeds[token].feed.latestAnswer();
            require(price > 0, "Invalid feed price");
```
latestAnwer can return 0 on worst occiason and make most of the market not work but people can still deposit chick is bad and people can loose funds 
reason it qa is because of the revert of price is zero if the protocol didnt have that then it would be a medium
#### no address zero checks 
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L77-L87
#### ` liqaudtionfactorbps` can be set to 10_000 unlike what the comment says
```
    @dev Must be set between 1 and 10000.
    @param _liquidationFactorBps The new liquidation factor in basis points. 1 = 0.01%/
    */
    function setLiquidationFactorBps(uint256 _liquidationFactorBps)
        public
        onlyGov
    {
        require(
            _liquidationFactorBps > 0 && _liquidationFactorBps <= 10000,
            "Invalid liquidation factor"
        );
        liquidationFactorBps = _liquidationFactorBps;
    }

```
####  in the future if the lender is someone else then inverse  they can steal funds from market
```
   function recall(uint256 amount) public {
        require(msg.sender == lender, "Only lender can recall");
        dola.transfer(msg.sender, amount);
    }
```
#### make internal functions last in the file and put public in front of the file for readability 
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L308-L333

#### getEscrow should be renamed to setEscrow  because its state changing  function
```solidity 
    function getEscrow(address user) internal returns (IEscrow) {
        if (escrows[user] != IEscrow(address(0))) return escrows[user];
        IEscrow escrow = createEscrow(user);
        escrow.initialize(collateral, user);
        escrows[user] = escrow;
        return escrow;
    }
```
its best practice to make view functions  the getter functions
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L350
####   if an market contract has callback for minting but there are using the governace contract it will revert and cause issues so make sure that callback is either deprecated or have flags on the deposit function, to  call the escrow `onDeposit()`
```
//@done this contract cant handle onDeposit escorws just becarefull because of users use this they can loose funds
    /* Uncomment if Escrow contract should handle on deposit callbacks. This function should remain callable by anyone to handle direct inbound transfers.
    function onDeposit() public {

    }
    */
```
https://github.com/code-423n4/2022-10-inverse/blob/aaf717f13c1f9a6e2f0888fef7c9b40400eeb4ec/src/escrows/GovTokenEscrow.sol#L69

####  you should make pausing mechanism for the protocol 
Right now there is only pause for borrowing but not depositing into the protocol which if there is bug in the protocol the funds will not get stopped and it can cause loss of funds 

#### it is   best practice  to not use ` tx.origin` the user can be opened up to phishing attacks though `tx.origin`
 in `borrowcontoler.sol`
####  some parameters for market can make the main functions not working like `collateralFactorbps `
if  `collateralFactorbps ` is 0  then the whole market wont work execpt depositing and you have no way of the getting the funds out 
2 ways this can happen on deployment or goverance 
1. on deployment `collateralbps=0`  then the functions will revert 
but the governance can change it but it might take a day or 2 for the proposal to get executed 
```solidity 
 uint256 minimumCollateral = (((debt * 1 ether) /
            oracle.getPrice(address(collateral), collateralFactorBps)) *
            10000) / collateralFactorBps;

```
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L495-L497

####  in the constructor `replenishmentIncentiveBps` can be set   to ` 0`  which in the comments say isnt possible 
which is crucial for the `market.sol->forceReplenish` because if zero then there is no rewards.
```solidity 
        require(
            _replenishmentIncentiveBps < 10000,
            "Replenishment incentive must be less than 100%"
        );
```
then in the comments it says
```solidity 
    @dev Must be set between 1 and 10000.
    @param _replenishmentIncentiveBps The new replenishment incentive set in basis points. 1 = 0.01%
    */
    function setReplenismentIncentiveBps(uint256 _replenishmentIncentiveBps)
        public
     
```
https://github.com/code-423n4/2022-10-inverse/blob/cc281e5800d5860c816138980f08b84225e430fe/src/Market.sol#L242