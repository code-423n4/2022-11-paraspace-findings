https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L326-L338

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296-L305
 
in _removeAsset and _removeFeeder methods there is a check with _onlyAssetExisted and _onlyFeederExisted modifiers however these checks in external modifier too. Due to this methods can only with with external functions of removeAsset and removeFeeder these modifier usage just double check same thing.
(Depends on your choose u can remove modifiers from removeAsset and removeFeeder instead of _removeAsset and _removeFeeder. Because they just call internal function and can be cheacked in internal.)

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L290-L294

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L320-L324

in _addAssets and _addFeeders methods index of lists cannot be overflow or underflow because inde i start with 0 and just increase once in every repeat of loop.Because of that this i can be use in unchecked for consume less gas when increase.(Because of the prevent under/overflow solidity add check mechanism to uints after version 0.8.0. This cause more gas consume for calculations.).

I linked a picture of gas reports of original and optimized gas costs of NFTFloorPriceOracle.sol and optimized one also solutions.

https://imgur.com/a/4L7PBLa