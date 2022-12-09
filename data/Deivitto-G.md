
# GAS

## Increments can be unchecked in loops
### Summary
Unchecked operations as the `++i` on for loops are cheaper than checked one.

This is already done in
https://github.com/code-423n4/2022-11-paraspace/blob/8b56ab655e8edf8f6d2bdc922e11ecb2f33d66b6/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L113-L115

### Details
In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline..

We can save around 14% of gas by doing this as seen in:
https://hackmd.io/@totomanov/gas-optimization-loops

    The code would go from:
```
    for (uint256 i; i < numIterations; i++) {
    // ...
    }
```

    to
```
    for (uint256 i; i < numIterations;) {
      // ...
      unchecked { ++i; }
    }
    The risk of overflow is inexistent for a uint256 here.
```



### Github Permalinks


https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229
        for (uint256 i = 0; i < _assets.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291
        for (uint256 i = 0; i < _assets.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321
        for (uint256 i = 0; i < _feeders.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413
        for (uint256 i = 0; i < feederSize; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95
        for (uint256 i = 0; i < assets.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197
        for (uint256 i = 0; i < assets.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193
        for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210
        for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38
        for (i = 0; i < params.nftTokenIds.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56
        for (i = 0; i < params.nftTokenIds.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371
            for (uint256 index = 0; index < totalBalance; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466
        for (uint256 index = 0; index < totalBalance; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177
        for (uint256 i = 0; i < marketplaceIds.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284
        for (uint256 i = 0; i < marketplaceIds.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382
        for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470
        for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58
        for (uint16 i = 0; i < params.reservesCount; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104
        for (uint256 i = 0; i < assets.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184
            for (uint256 index = 0; index < params.tokenData.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195
        for (uint256 index = 0; index < params.tokenData.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131
        for (uint256 index = 0; index < amount; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212
            for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465
            for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910
                for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034
        for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72
        for (uint256 index = 0; index < _nfts.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103
        for (uint256 index = 0; index < _nfts.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138
        for (uint256 index = 0; index < _nftPairs.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172
        for (uint256 index = 0; index < actualTransferAmount; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199
        for (uint256 index = 0; index < _nftPairs.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215
        for (uint256 index = 0; index < _nftPairs.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279
        for (uint256 index = 0; index < _nfts.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289
        for (uint256 index = 0; index < _nftPairs.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305
        for (uint256 index = 0; index < _nftPairs.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634
        for (uint256 i = 0; i < reservesListCount; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493
        for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106
            for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145
        for (uint256 i = 0; i < ids.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107
        for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51
            for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97
        for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207
        for (uint256 index = 0; index < tokenData.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280
        for (uint256 index = 0; index < tokenIds.length; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213
        for (uint256 index = 0; index < totalBalance; index++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L82
        for (uint256 i = 0; i < punkIndexes.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L109
        for (uint256 i = 0; i < punkIndexes.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L113
        for (uint256 i = 0; i < punkIndexes.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L136
        for (uint256 i = 0; i < punkIndexes.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L174
        for (uint256 i = 0; i < punkIndexes.length; i++) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L442
            while (arr[uint256(i)] < pivot) i++;

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L449

### Mitigation
Add unchecked `++i` at the end of all the for loop where it's not expected to overflow and remove them from the for header





## Store using Struct over multiple mappings
### Summary
All these variables could be combine in a Struct in order to reduce the gas cost. 
### Details
As noticed in: 
https://gist.github.com/alexon1234/b101e3ac51bea3cbd9cf06f80eaa5bc2
When multiple mappings that access the same addresses, uints, etc, all of them can be mixed into an struct and then that data accessed like:
`mapping(datatype => newStructCreated) newStructMap;`
Also, you have this post where it explains the benefits of using Structs over mappings 
https://medium.com/@novablitz/storing-structs-is-costing-you-gas-774da988895e

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L74
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L88
- - - - -
all the token id -> to something 
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L24-L44
- - - - -
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L38
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L26
- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L24-L28

### Mitigation
Consider mixing different mappings into an struct when able in order to save gas.


## Pack structs tightly
### Summary
Gas efficiency can be achieved by tightly packing the struct. Struct variables are stored in 32 bytes each so you can group smaller types to occupy less storage. 

### Details
You can read more here: https://fravoll.github.io/solidity-patterns/tight_variable_packing.html or in the official documentation: https://docs.soliditylang.org/en/v0.4.21/miscellaneous.html

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L32-L41

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L50-L61


### Mitigation
Order structs fields from less data size to bigger size to reduce gas usage.


## `abi.encode()` is less gas efficient than `abi.encodePacked()`
### Summary
In general, `abi.encodePacked` is more gas-efficient.

### Details
Changing the abi.encode function to `abi.encodePacked` can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.
### Github Permalinks


https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1124
                abi.encode(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1136
                abi.encode(


### Mitigation
Consider changing abi.encode to `abi.encodePacked`


## Unnecesary storage read
### NOTE
These weren't found at C4udit / Publicly Known Issues (GAS-4):
https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba

### Summary
A value which value is already known can be used directly rather than reading it from the storage

### Details
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

Saves 100 gas per instance.

### Example
```
    function _finalizePrice(address _asset, uint256 _twap) internal {
        PriceInformation storage assetPriceMapEntry = assetPriceMap[_asset];
        assetPriceMapEntry.twap = _twap;
        assetPriceMapEntry.updatedAt = block.number;
        assetPriceMapEntry.updatedTimestamp = block.timestamp;
        emit AssetDataSet(
            _asset,
            assetPriceMapEntry.twap,
            assetPriceMapEntry.updatedAt
        );
    }

```

Recommendation
Change to:

```
    function _finalizePrice(address _asset, uint256 _twap) internal {
        PriceInformation storage assetPriceMapEntry = assetPriceMap[_asset];
        assetPriceMapEntry.twap = _twap;
        assetPriceMapEntry.updatedAt = block.number;
        assetPriceMapEntry.updatedTimestamp = block.timestamp;
        emit AssetDataSet(
            _asset, //@audit here
            _twap, //@audit and here
            block.number
        );
    }

```
### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381-L385

### Mitigation
Set directly the value to avoid unnecessary storage read to save some gas






## Internal functions only called once can be inlined to save gas
### Summary
Not inlining costs `20` to `40` gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.
### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L265
    function _whenNotPaused(address _asset) internal view {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L388
    function _addRawValue(address _asset, uint256 _twap) internal {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L218
    function _onlyAssetListingOrPoolAdmins() internal view {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L552
    function _checkAllowance(address token, address operator) internal {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1133
    function getDomainSeparator() internal view returns (bytes32) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413
    function setSApeUseAsCollateral(address user) internal {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69
    function _onlyPoolConfigurator() internal view virtual {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L76
    function _onlyPoolAdmin() internal view virtual {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L130
    function initializeStakingData() internal {

### Mitigation
Consider changing internal function only called once to inline code for gas savings


## Unused named returns
### Summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 
### Details
Also as returns variable is ignored, it wastes extra gas

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L240
        returns (uint256 price)

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L256
        returns (uint256 timestamp)

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L231
        returns (

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L114
        returns (

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L194
        returns (

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L267
        returns (

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L218
    ) external view returns (uint256 ltv, uint256 lt) {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L256
        returns (uint256 ltv, uint256 lt)

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L370
        returns (

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L387
        returns (

### Mitigation
Remove return or returns when both used


## Retrieve internal variables directly
### Summary
Save `~30 gas` per call to each function

### Example
        `return getAddress(POOL);`
to
    `return _addresses[POOL];`

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L101
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L117
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L138
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L154
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L166
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L178
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L197
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L202
### Mitigation
Retrieve internal variables directly rather than calling the retrieving function






## Superfluous event fields
### Summary
`block.timestamp` and `block.number` are added to event information by default so adding them manually wastes gas.
### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381-L385
### Mitigation
Consider not using a field for a value that are already known in order to save gas

## Functions guaranteed to revert when called by normal users can be marked `payable`
### Summary
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function.

Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Details
The extra opcodes avoided are:
CALLVALUE (2), DUP1 (3), ISZERO (3), PUSH2 (3), JUMPI (10), PUSH1 (3), DUP1 (3), REVERT(0), JUMPDEST (1), POP (2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

### Github Permalinks

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L59
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L73
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L84
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L109
    ) external override onlyOwner {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L124
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L145
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158
    function setACLManager(address newAclManager) external override onlyOwner {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170
    function setACLAdmin(address newAclAdmin) external override onlyOwner {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L185
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L227
        onlyOwner

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235
    function setWETH(address newWETH) external override onlyOwner {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L248
    ) external override onlyOwner {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L206
    ) external onlyOwner {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L219
        onlyOwner
### Mitigation
Consider adding payable to save gas


## `<X> -= <Y>` costs more gas than `<X> = <X> - <Y>` for state variables
### Summary
`x-=y` costs more gas than x=x-y for state variables

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L318
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L146

### Mitigation
Don't use `-=` for state variables as it cost more gas.



## duplicated `require()` check should be refactored
### Summary
duplicated `require()` / `revert()` checks should be
refactored to a modifier or function to save gas
### Details
Event appears twice and can be reduced

### Github Permalinks

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L51
        require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84
        require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);


https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L38
        require(runInput.details.length == 1, Errors.INVALID_MARKETPLACE_ORDER);


https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L51
        require(nfts.length == 1, Errors.INVALID_MARKETPLACE_ORDER);

- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L440
        require(vars.xTokenAddress != address(0), Errors.ASSET_NOT_LISTED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L489
                require(isNToken, Errors.ASSET_NOT_LISTED);
- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L68
        require(amount != 0, Errors.INVALID_AMOUNT);
        
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L160
        require(amount != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L258
        require(params.amount != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L377
        require(amountSent != 0, Errors.INVALID_AMOUNT);
- - - - -
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L275
        require(!vars.isFrozen, Errors.RESERVE_FROZEN);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L87
        require(!isFrozen, Errors.RESERVE_FROZEN);
- - - - -
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L273
        require(vars.isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L390
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L85
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L185
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L207
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L460
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L438
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L824
        require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
        
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1029
            require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1057
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L768
        require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
- - - - -
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L274
        require(!vars.isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L208
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L86
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L186
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L439
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L391
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L461
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L769
        require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L825
        require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L937
        require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L950
        require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1030
            require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1058
        require(!isPaused, Errors.RESERVE_PAUSED);
- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1068
        require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1074
        require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);
- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L271
        require(user != address(0), Errors.ZERO_ADDRESS_NOT_VALID);


- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L70
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L102
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L85
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L110
        revert(Errors.CALL_MARKETPLACE_FAILED);
- - - - -

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L152
        revert(Errors.ORACLE_PRICE_NOT_READY);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L169
        revert(Errors.ORACLE_PRICE_NOT_READY);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L186
        revert(Errors.ORACLE_PRICE_NOT_READY);

- - - - -
### Recommendation
Refactor to a function or modifier when used multiple times in a contract

## use of custom errors rather than `revert()` / `require()` error message
### NOTE
These weren't found at C4udit / Publicly Known Issues (GAS-6):
https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba

### Summary
Custom errors reduce `38` gas if the condition is met and `22` gas otherwise.
Also reduces contract size and deployment costs.

Strings are being used in all application, not only under "regular string" but also in constants saved in Errors.
### Details
Since version `0.8.4` the use of custom errors rather than `revert()` / `require()` saves gas as noticed in
https://blog.soliditylang.org/2021/04/21/custom-errors/
https://github.com/code-423n4/2022-04-pooltogether-findings/issues/13

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L51
        require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L54
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L71
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84
        require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L87
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L38
        require(runInput.details.length == 1, Errors.INVALID_MARKETPLACE_ORDER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L44
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L51
        require(nfts.length == 1, Errors.INVALID_MARKETPLACE_ORDER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L225
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L243
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L91
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L96
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L134
        require(price != 0, Errors.ORACLE_PRICE_NOT_READY);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L222
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L86
        require(id != POOL, Errors.INVALID_ADDRESSES_PROVIDER_ID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L268
        require(newAddress != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L46
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L86
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L245
        require(vars.orderInfo.taker == onBehalfOf, Errors.INVALID_ORDER_TAKER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L294
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L384
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L388
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L393
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L412
            require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L440
        require(vars.xTokenAddress != address(0), Errors.ASSET_NOT_LISTED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L472
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L489
                require(isNToken, Errors.ASSET_NOT_LISTED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L494
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L45
            require(Address.isContract(params.asset), Errors.NOT_CONTRACT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L56
        require(!reserveAlreadyAdded, Errors.RESERVE_ALREADY_ADDED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L66
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L409
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L68
        require(amount != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L71
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L84
        require(reserveAssetType == assetType, Errors.INVALID_ASSET_TYPE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L85
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L86
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L87
        require(!isFrozen, Errors.RESERVE_FROZEN);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L92
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L102
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L123
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L133
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L140
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L160
        require(amount != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L162
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L168
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L181
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L185
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L186
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L203
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L207
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L208
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L258
        require(params.amount != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L269
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L273
        require(vars.isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L274
        require(!vars.isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L275
        require(!vars.isFrozen, Errors.RESERVE_FROZEN);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L276
        require(vars.borrowingEnabled, Errors.BORROWING_NOT_ENABLED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L278
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L306
                require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L335
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L339
        require(vars.currentLtv != 0, Errors.LTV_VALIDATION_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L341
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L357
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L377
        require(amountSent != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L378
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L390
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L391
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L392
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L401
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L406
        require((variableDebt != 0), Errors.NO_DEBT_OF_SELECTED_TYPE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L418
        require(userBalance != 0, Errors.UNDERLYING_BALANCE_ZERO);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L421
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L434
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L438
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L439
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L456
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L460
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L461
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L515
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L520
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L525
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L533
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L550
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L555
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L564
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L574
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L578
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L596
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L611
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L636
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L640
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L645
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L655
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L659
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L666
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L672
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L686
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L690
        require(params.globalDebt != 0, Errors.GLOBAL_DEBT_IS_ZERO);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L732
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L757
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L762
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L768
        require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L769
        require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L771
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L775
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L782
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L795
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L815
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L819
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L824
        require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L825
        require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L826
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L833
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L869
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L918
                    require(assetLTV == 0, Errors.LTV_VALIDATION_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L921
                require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L937
        require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L950
        require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L975
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L976
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L980
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L984
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1000
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1004
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1011
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1029
            require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1030
            require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1035
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1057
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1058
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1059
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1068
        require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1074
        require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1075
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1080
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1196
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1202
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1208
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L73
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L85
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L104
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L112
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L139
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L180
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L200
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L280
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L290
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L338
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L356
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L380
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L453
        require(isActive, Errors.RESERVE_INACTIVE);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L454
        require(!isPaused, Errors.RESERVE_PAUSED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L80
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L692
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L726
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L761
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L803
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L70
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L77
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L158
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L173
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187
        require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L188
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L214
        require(value != 0, Errors.INVALID_AMOUNT);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L271
        require(user != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L281
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L49
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L60
        require(_msgSender() == address(POOL), Errors.CALLER_MUST_BE_POOL);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L193
        require(to != owner, "ERC721
 approval to old owner");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L195
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L213
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L259
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L296
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L354
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L594
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L618
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L60
        require(initializingPool == POOL, Errors.POOL_ADDRESSES_DO_NOT_MATCH);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L64
        require(underlyingAsset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L141
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L172
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L176
        require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L70
        require(msg.sender == _underlyingAsset, Errors.OPERATION_NOT_SUPPORTED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L98
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L131
        require(user == ownerOf(tokenId), Errors.NOT_THE_OWNER);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L146
        require(success, "ETH_TRANSFER_FAILED");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L88
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92
        require(to != address(0), "ERC721
 transfer to the zero address");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L93
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L164
        require(owner == sender, "not owner");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L167
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L199
        require(to != address(0), "ERC721
 mint to the zero address");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L210
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L283
            require(owner == user, "not the owner of Ntoken");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L284
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L363
        require(owner != operator, "ERC721
 approve to caller");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L373
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L377
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L391
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L395
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L409
            require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L67
        require(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L72
        require(oldValue != incentive, "Same Value");

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L70
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L102
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L85
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L110
        revert(Errors.CALL_MARKETPLACE_FAILED);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L152
        revert(Errors.ORACLE_PRICE_NOT_READY);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L169
        revert(Errors.ORACLE_PRICE_NOT_READY);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L186
        revert(Errors.ORACLE_PRICE_NOT_READY);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L219
            revert(Errors.INVALID_MARKETPLACE_ID);




### Mitigation
Replace each error message in a require by a custom error

### Mitigation
refactor this checks to different functions to save gas

## splitting `require()` statements that use `&&` saves gas
### Summary
Instead of using the && operator in a single require statement to check multiple conditions, consider using multiple require statements with 1 condition per require statement (saving 3 gas per & ):
### Github Permalinks

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L43
            resolvers.length == 0 && isBasicOrder(advancedOrder),

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L73
            advancedOrders.length == 2 &&

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L74
                isBasicOrder(advancedOrders[0]) &&

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L172
            marketplaceIds.length == payloads.length &&

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L280
            marketplaceIds.length == payloads.length &&

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L390
                    (vars.isETH && item.itemType == ItemType.NATIVE),

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L547
            vars.collateralReserveActive && vars.principalReserveActive,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L551
            !vars.collateralReservePaused && !vars.principalReservePaused,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L637
            vars.collateralReserveActive && vars.principalReserveActive,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L641
            !vars.collateralReservePaused && !vars.principalReservePaused,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L673
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1197
                vars.token0IsActive && vars.token1IsActive,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1203
                !vars.token0IsPaused && !vars.token1IsPaused,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1209
                !vars.token0IsFrozen && !vars.token1IsFrozen,

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L217
            value > MIN_AUCTION_HEALTH_FACTOR &&



### Mitigation
Split require statements


## `--i` costs less gas compared to `i--` or `i-=1`
### Summary
`--i` costs less gas compared to `i--` or `i -= 1` for unsigned integer, as pre-increment is cheaper (about `5 gas` per iteration). 
This statement is true even with the optimizer enabled. 

### Details
`i--` decrements `i` and returns the initial value of `i` . 
Which means:
```
uint i = 1;
i--; // == 0 but i == 1
```

But `--i` returns the actual decremented value:

```
uint i = 1;
--i; // == 0 and i == 0 too, so no need for a temporary variable
```

In the first case, the compiler has to create a temporary variable (when used) for returning `0` instead of `1`

### Github Permalinks
`-=` 

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L146

`--` 
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L443
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L450
### Mitigation
Replace to `--i` as needed.



## Reduce the size of error messages (Long revert Strings)
### Summary
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

### Details
Although I recommend mostly to just use custom errors, in case of wanting to keep the design with a file with the error strings, I would suggest to keep it consistent adding the other require with strings in the file with the same style used in [Errors.sol](https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol#L125) that has less than 32 bytes.

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L227

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L356

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L197

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L215

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L261

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L298

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L356

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L596

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L620

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L100

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L90

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L379

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L397

### Mitigation
Consider shortening the revert strings to fit in 32 bytes
