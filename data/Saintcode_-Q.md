## MISSING INITIALIZER MODIFIER ON `initialize()` function

OpenZeppelin recommends that the `initializer`modifier be applied to `initialize()` in order to avoid potential griefs, social engineering, or exploits. Ensure that the modifier is applied to the implementation contract.

Instance:
-GovTokenEscrow.sol line 30
-INVEscrow.sol line 44
-SimpleERC20.sol line 25

# TYPOS

-Fed.sol line 45, the word “contract” is written wrong, “contact”

## STATE VARIABLE VISIBILITY IS NOT SET
Visibility is not set for the `callOnDepositCallback` state variable in Market.sol.

-Market.sol line 52












