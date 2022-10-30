### `DBR`: Use unchecked increment in `DBR#invalidateNonce`

Since incrementing the user's nonce will never realistically overflow, you can safely use an unchecked increment in `DBR#invalidateNonce`. (Note that you're already doing so when incrementing the nonce in other functions):

[`DBR#invalidateNonce`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/DBR.sol#L257-L260)

```solidity
function invalidateNonce() public {
  nonces[msg.sender]++;
}

```

Suggestion:

```solidity
function invalidateNonce() public {
  unchecked {
    nonces[msg.sender]++;
  }
}

```

### `Fed`: Use unchecked subtraction in `Fed#contraction`

Since an earlier `require` statement ensures that `amount` is less than or equal to `supply`, you can safely perform an unchecked subtraction in `Fed#contraction`.

[`Fed#contraction`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Fed.sol#L103-L113)

```solidity
function contraction(IMarket market, uint amount) public {
  require(msg.sender == chair, "ONLY CHAIR");
  require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
  uint supply = supplies[market];
  require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
  market.recall(amount);
  dola.burn(amount);
  supplies[market] -= amount;
  globalSupply -= amount;
  emit Contraction(market, amount);
}

```

Suggestion:

```solidity
function contraction(IMarket market, uint amount) public {
  require(msg.sender == chair, "ONLY CHAIR");
  require(dbr.markets(address(market)), "UNSUPPORTED MARKET");
  uint supply = supplies[market];
  require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits
  market.recall(amount);
  dola.burn(amount);
  unchecked {
    supplies[market] -= amount;
    globalSupply -= amount;
  }
  emit Contraction(market, amount);
}

```

### `Market`: Use unchecked subtraction in `getWithdrawalLimit`

Since an earlier `require` check ensures that `collateralBalance` is less than or equal to `minimumCollateral`, you can safely perform an unchecked subtraction in `getWithdrawalLimit`:

[`Market#getWithdrawalLimit`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L370-L380)

```solidity
function getWithdrawalLimit(address user) public view returns (uint) {
  IEscrow escrow = predictEscrow(user);
  uint collateralBalance = escrow.balance();
  if (collateralBalance == 0) return 0;
  uint debt = debts[user];
  if (debt == 0) return collateralBalance;
  if (collateralFactorBps == 0) return 0;
  uint minimumCollateral = (((debt * 1 ether) /
    oracle.viewPrice(address(collateral), collateralFactorBps)) * 10000) /
    collateralFactorBps;
  if (collateralBalance <= minimumCollateral) return 0;
  return collateralBalance - minimumCollateral;
}

```

Suggestion:

```solidity
function getWithdrawalLimit(address user) public view returns (uint) {
  IEscrow escrow = predictEscrow(user);
  uint collateralBalance = escrow.balance();
  if (collateralBalance == 0) return 0;
  uint debt = debts[user];
  if (debt == 0) return collateralBalance;
  if (collateralFactorBps == 0) return 0;
  uint minimumCollateral = (((debt * 1 ether) /
    oracle.viewPrice(address(collateral), collateralFactorBps)) * 10000) /
    collateralFactorBps;
  if (collateralBalance <= minimumCollateral) return 0;
  unchecked {
    return collateralBalance - minimumCollateral;
  }
}

```

### `Market`: Use unchecked increment in `invalidateNonce`

Since incrementing the user's nonce will never realistically overflow, you can safely use an unchecked increment in `Market#invalidateNonce`. (Note that you're already doing so when incrementing the nonce in other functions):

[`Market#invalidateNonce`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L519-L522)

```solidity
function invalidateNonce() public {
  nonces[msg.sender]++;
}

```

Suggestion:

```solidity
function invalidateNonce() public {
  unchecked {
    nonces[msg.sender]++;
  }
}

```

### `Market`: Use unchecked subtraction in `repay`

Since an earlier `require` statement ensures that `debts[user]` is greater than or equal to `amount`, you can safely use an unchecked subtraction in `repay`:

[`Market#repay`](https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L531-L539)

```solidity
function repay(address user, uint amount) public {
  uint debt = debts[user];
  require(debt >= amount, "Insufficient debt");
  debts[user] -= amount;
  totalDebt -= amount;
  dbr.onRepay(user, amount);
  dola.transferFrom(msg.sender, address(this), amount);
  emit Repay(user, msg.sender, amount);
}

```

Suggestion:

```solidity
function repay(address user, uint amount) public {
  uint debt = debts[user];
  require(debt >= amount, "Insufficient debt");
  unchecked {
    debts[user] -= amount;
  }
  totalDebt -= amount;
  dbr.onRepay(user, amount);
  dola.transferFrom(msg.sender, address(this), amount);
  emit Repay(user, msg.sender, amount);
}

```