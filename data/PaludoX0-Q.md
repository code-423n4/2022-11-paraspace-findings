#### Lack of immutable variables check 
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L63
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L90

        ADDRESSES_PROVIDER = provider;
Immutable addresses should be checked in constructor before assigning 

#### Ownership change function missing
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol#L12
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol#L53
I dont'see any possibility to call setContractOwner() other than in constructor. It can happen that the ownership shall be changed when contract is operative.
I suggest also to set up also a PAUSABLE feature to pause the proxy contract.

#### Missing initialize function to set Reentrancy Guard Status pointer 
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L24
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L40
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f044/9/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L46
There's not the initialize function that set `rgs._status = _NOT_ENTERED;` as done in *PoolCore#L75*

#### Pointer variable name
This is just a informational hint: maybe you meant to use `ps` (pointstorage) instead of `rgs` (reentrancyGuardStorage) in *contracts/protocol/pool/PoolStorage.sol#L22*

