-> USE X = X + Y ITS CHEAPER THAN X += Y (same for X = X - Y IS CHEAPER THAN X -= Y); SO TRY TO AVOID +=/-= AND USE X=X+Y OR X=X-Y INSTEAD

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=balances%5Bmsg.sender%5D%20%2D%3D%20amount%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=unchecked%20%7B-,balances%5Bto%5D%20%2B%3D%20amount%3B,%7D,-emit%20Transfer(msg
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=dueTokensAccrued%5Buser%5D%20%2B%3D%20accrued%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=totalDueTokensAccrued%20%2B%3D%20accrued%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=debts%5Buser%5D%20%2B%3D%20additionalDebt%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=debts%5Buser%5D%20%2D%3D%20repaidDebt%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=debts%5Buser%5D%20%2B%3D%20replenishmentCost%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=_totalSupply%20%2B%3D%20amount%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol#:~:text=supplies%5Bmarket%5D,globalSupply%20%2B%3D%20amount%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=debts%5Bborrower%5D%20%2B%3D%20amount%3B

->SPLIT REQUIRE () STATEMENTS, IF THEY USE &&, TO SAVE GAS.

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=require(recoveredAddress%20!%3D,%7D
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=require(_liquidationIncentiveBps%20%3E%200%20%26%26%20_liquidationIncentiveBps%20%3C%2010000%2C%20%22Invalid%20liquidation%20incentive%22)%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=require(_liquidationFactorBps%20%3E,%7D
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=require(_replenishmentIncentiveBps%20%3E,%7D
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=require(_liquidationIncentiveBps%20%3E%200%20%26%26%20_liquidationIncentiveBps%20%2B,%7D
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=require(_liquidationFeeBps%20%3E,%7D
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=)%3B-,require(recoveredAddress%20!%3D%20address(0)%20%26%26%20recoveredAddress%20%3D%3D%20from%2C%20%22INVALID_SIGNER%22)%3B,-borrowInternal(from%2C

-> IN GENERALLY USING ++i COST LESS GAS THAN i++ OR += 1 (the same applies to i-- or i -=1)

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=value%2C-,nonces%5Bowner%5D%2B%2B%2C,-deadline
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=nonces%5Bmsg.sender%5D%2B%2B%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol#:~:text=nonces%5Bmsg.sender%5D%2B%2B%3B

->WHEN YOU USE BOOLS FOR STORAGE IT WILL COST MORE GAS AND INCURS OVERHEAD

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=mapping%20(address%20%3D%3E%20bool)%20public%20minters%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=mapping%20(address%20%3D%3E%20bool)%20public%20markets%3B
https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#:~:text=mapping%20(address%20%3D%3E%20uint)%20public%20debts%3B



