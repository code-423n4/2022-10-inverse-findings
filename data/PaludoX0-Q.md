
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.solL#48 57 66
use a modifier instead of repeating require statement
    modifier onlyGov {
        require(msg.sender == gov, "ONLY GOV");
        _;
    }

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.solL#48
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/BorrowController.solL#26
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.solL#130
Use a two step approval for Governance or Operator address change as already set for operator at src/Oracle.solL#44 or at least chech that _gov!=address(0)
Moreover an event should be emitted for such important change

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.solL#91 and L#110
Check-effects-interactions pattern is not respected where code: (i) first undertakes any checks to ensure it should continue to make progress within the function; then is followed by (ii) any code that manipulates contract state; and finally followed by (iii) any code that interacts with any other contracts or external parties
Should be rewritten as follows:
        
        supplies[market] += amount;
        globalSupply += amount;
        dola.mint(address(market), amount);
Same thing for contraction function

https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.solL#567
It is correct that transaction shall revert if collateralValue >= debts[user], but if dbr.replenishmentPriceBps() is high function will revert  
it's sufficient for a borrower to keep debts[user] close to collateralValue and forceReplenish will always revert
Suggestion is: in case collateralValue >= debts[user] instead of reverting, set debts[user] = collateralValue and force liquidation
