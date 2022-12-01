QA1: https://github.com/para-space/paraspace-core/blob/d3a14348c91d0869f97bc438659fa65b6fd79a01/contracts/protocol/pool/PoolApeStaking.sol#L120
A zero length check for ``_nftPairs`` is necessary. 

QA2: https://github.com/para-space/paraspace-core/blob/d3a14348c91d0869f97bc438659fa65b6fd79a01/contracts/protocol/pool/PoolApeStaking.sol#L188
A zero length check for ``_nftPairs`` is necessary. 

QA3: https://code4rena.com/contests/2022-11-paraspace-contest/submit
change the line to the following to avoid hash collision. Given a known input, it is not difficult to create another input to have the same hash, but it is extremely difficult to create another input to have the same output hash of keccak256("UPDATER_ROLE") - 1. 
```
 bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE") - 1;
```

QA4: 
