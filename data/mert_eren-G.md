https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L148-L154


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296-L305

First one call second function and they double check "onlyWhenAssetExisted" condition so they consume gas inefficiently.  "onlyWhenAssetExisted" can be removed in one of the  "removeasset" or "_removeasset" function.