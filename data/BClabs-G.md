Gas:
Contract can't migrate on a different chain, so block.chainid will always be equal to INITIAL_CHAIN_ID, so you could simply return INITIAL_DOMAIN_SEPARATOR.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L98

Gas:
EncodePacked is more efficient than encode
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L104

Gas:
Values that should and will never be larger than some number could be written in smaller than uint256 uint type. 
For example, BPS can never be larger than 10000, so uint16 should be sufficient and will reduce needed storage slots.
https://github.com/code-423n4/2022-10-inverse/blob/3e81f0f5908ea99b36e6ab72f13488bbfe622183/src/Market.sol#L69-L71
