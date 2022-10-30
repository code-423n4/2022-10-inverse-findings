
1.

Grammer issues in comments in:

1.1

Contract: Market.sol

1.1.1

	line 116, and 122

	consider changing "conforming" to "conforms" for better readability
	
Modified Comments:

	@param _oracle The new oracle conforms to the IOracle interface.

	@param _borrowController The new borrow controller conforms to the IBorrowController interface.

1.1.2

	line 146

	"must" is misspelled as "mus"

Modified Comment:

	@dev Collateral factor must be set below 100%

1.1.3

	line 147

	"as" should be "is"

Modified Comment:

	@param _collateralFactorBps The new collateral factor is measured in basis points. 

1.1.4

	line 157

	there is a duplicate "of" before "a"

Modified Comment:

	At 5000, 50% of a borrower's underwater debt can be liquidated. Only callable by governance.

1.1.5

	line 181

	"liquidation" is misspelled as "liqudation"

Modified Comment:

	@param _liquidationIncentiveBps The new liquidation incentive set in basis points. 1 = 0.01% 

1.1.6

	line 201

	"of" is misspelled as "od"

	there is a duplicate "the" before "lender"

Modified Comment:

	@param amount The amount of DOLA to recall to the lender.

1.1.7

	line 385

	the "to" after "accrued" is unnecessary

Modified Comment:

	@param borrower The address of the borrower that debt will be accrued.

1.1.8

	line 414, and 478

	"message" is presented in the past tense and doesn't make the sentence make sense

Modified Comments:

	@dev Signed message can be invalidated by incrementing the nonce. Will always borrow to the msg.sender.
	
	@dev Signed message can be invalidated by incrementing the nonce. Will always withdraw to the msg.sender.

1.1.9

	line 575

	"liquidatable" is misspelled as "liquidateable"

Modified Comment:

	@notice View function for getting the amount of liquidatable debt a user holds.

1.1.10

	line 587

	"under water" should be one word

Modified Comment:

	@notice Function for liquidating a user's underwater debt. Debt is under water when the value of a user's debt is above their collateral factor.

1.1.11
	
	line 589

	there is a duplicate "user" before "debt"

Modified Comment:

	@param repaidDebt Th amount of user debt to liquidate.
	
1.2

Contract: DBR.sol

1.2.1

	line 50

	"operator" is misspelled as "oprator"

Modified Comment:

	@notice Sets pending operator of the contract. Operator role must be claimed by the new operator. Only callable by Operator.

1.2.2

	line 87

	"allowed" is misspelled as "allowed"

Modified Comment:

	@notice Removes a minter from the set of addresses allowed to mint DBR tokens. Only callable by Operator.

1.2.3

	line 128

	"the" before "user" is misspelled as "the"

Modified Comment:

	@notice Get the DBR deficit of an address. Will return 0 if the user has zero DBR or more.

1.2.4

	line 308

	the "a" before "repayment" is unnecessary

Modified Comment:

	@notice Function to be called by markets when repayment occurs.
	
1.3

Contract: BorrowController.sol

1.3.1

	line 36

	"address" is misspelled as "addres"

Modified Comment:

	@param deniedContract The address of the denied contract

1.4.

Contract: Oracle.sol

1.4.1

	line 11

	"fixed price" should be hyphenated into "fixed-price"

Modified Comment:

	@notice Oracle used by markets. Can use both fixed-price feeds and Chainlink-style feeds for prices.

1.5

Contract: Fed.sol

1.5.1

	line 99

	"have" should be "has"

Modified Comment:

	@dev Markets can have more DOLA in them than they've been supplied, due to force replenishes. This call will revert if trying to contract more than has been supplied.

1.6

Contract: SimpleERC20Escrow.sol

1.6.1

	line 49

	the "on" before "deposit" is unnecessary

Modified Comment:

	/* Uncomment if Escrow contract should handle deposit callbacks. This


1.7

Contract: GovTokenEscrow.sol

1.7.1

	line 56

	the "on" before "deposit" is unnecessary

Modified Comment:

	/* Uncomment if Escrow contract should handle deposit callbacks. This