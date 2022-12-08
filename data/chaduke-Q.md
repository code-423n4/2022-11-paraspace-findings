QA1: https://github.com/para-space/paraspace-core/blob/d3a14348c91d0869f97bc438659fa65b6fd79a01/contracts/protocol/pool/PoolApeStaking.sol#L120
A zero length check for ``_nftPairs`` is necessary. 

QA2: https://github.com/para-space/paraspace-core/blob/d3a14348c91d0869f97bc438659fa65b6fd79a01/contracts/protocol/pool/PoolApeStaking.sol#L188
A zero length check for ``_nftPairs`` is necessary. 

QA3: https://code4rena.com/contests/2022-11-paraspace-contest/submit
change the line to the following to avoid hash collision. Given a known input, it is not difficult to create another input to have the same hash, but it is extremely difficult to create another input to have the same output hash of keccak256("UPDATER_ROLE") - 1. 
```
 bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE") - 1;
```
QA4:https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L81
The comparison to ADDRESSES_PROVIDER might be wrong since ``ADDRESSES_PROVIDER`` is not the same state variable that is initialized in the constructor. When ``initialize`` is called, ``ADDRESSES_PROVIDER`` will refer to the storage slot in the calling context (the caller) that corresponds to the same slot number as ``ADDRESSES_PROVIDER``, which could create a storage collision issue for delegate call (https://blog.finxter.com/delegatecall-or-storage-collision-attack-on-smart-contracts/). 
Mitigation: define ADDRESSES_PROVIDER as a constant

QA5: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L57-L59
Zero address check is needed for these arguments of the constructor. 
