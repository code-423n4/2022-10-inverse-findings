An alternative design of the whole Inverse ecosystem is recommended:  maybe we only need to deploy ONE market contract and ONE Escrow contract. Besides saving deployment gas, it reduces the attack surface since less contracts will be deployed. 

1. To support ONE market contract, rename market.sol to MarketGate.sol (or sth like that)
introduce a struct called *Market* that includes all the factors for the market, including the collaboral tokens to be used. 
```
 struct Market {
        IERC20 collateral,
        uint collateralFactorBps,
        uint replenishmentIncentiveBps,
        uint liquidationIncentiveBps,
        bool callOnDepositCallback,
        bool callOnDepositCallback;
        bool borrowPaused;
        uint  totalDebt;
        Mapping(address => Escrow) EscrowAccounts,        
    } 

   Using MarketGate for Market; 

MarketGate.sol: 
   .. // an example of a function in MarketGate.sol, the first arugment is a Market.
   function getEscrowAccount(Market m, address user) public returns (Escrow){
         return m.EscrowAccounts[user];
   }
   ...

```

Then move most code of Market.sol to a library Marketlib.sol such that the first argument of these functions is a market struct.
The MarketFactory will only provide the entry points that other contracts or admins can interact with. Escrow can also be introduced as a struct and then a EscrowManger.sol and move all existing code for Escrow.sol to Escrowlib.sol in which most functions should use the Escrow struct as the first argument.  The EscrowManager.sol will server as the entry points for interacting with Escrow logic. In the Escrow struct, there is no need to keep track of market and user information since Escrow accounts will always be used in a Market  Struct (see above) with both market and user contexts.

```
struct Escrow {
       uint balance, 
        .......                  
    }
```
```

