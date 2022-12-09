**Overview**

Risk Rating | Number of issues
--- | ---
Gas Issues | 9

**Codebase Impressions**

Unfortunately, `hardhat-gas-reporter` didn't work on the contest repo, so the savings are only estimations.

The sponsor's code is well packed and storage writing/reading are decently made. Except for some gotchas (that the sponsor seems aware about but isn't always consistently avoiding), this is a pretty well optimized code.

In the future, I'd suggest paying more attention to what happens in loops as the gas cost can increase exponentially in there (like, by reading a storage value repeatedly)

**Table of Contents:**

- [1. (2100 fixed gas + 2100-4200 gas per iteration) `struct PriceInformation` is misread as `memory` instead of `storage`](#1-2100-fixed-gas--2100-4200-gas-per-iteration-struct-priceinformation-is-misread-as-memory-instead-of-storage)
- [2. (200 gas per iteration) - Avoid storage readings in for-loops](#2-200-gas-per-iteration---avoid-storage-readings-in-for-loops)
- [3. (168 fixed gas + 84 gas per iteration) - Multiple accesses of a mapping/array should use a local variable cache](#3-168-fixed-gas--84-gas-per-iteration---multiple-accesses-of-a-mappingarray-should-use-a-local-variable-cache)
- [4. (25 gas per iteration) - Unchecking arithmetics operations that can't underflow/overflow](#4-25-gas-per-iteration---unchecking-arithmetics-operations-that-cant-underflowoverflow)
- [5. (33 gas per iteration) - Usage of `uint16` incurs overhead in for-loop](#5-33-gas-per-iteration---usage-of-uint16-incurs-overhead-in-for-loop)
- [6. (100 gas) - Missed by C4udit: State variables should be cached in stack variables rather than re-reading them from storage](#6-100-gas---missed-by-c4udit-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage)
- [7. (300 gas) - Avoid emitting a storage variable when a memory value is available](#7-300-gas---avoid-emitting-a-storage-variable-when-a-memory-value-is-available)
- [8. (Slot packing) - Proposal: reduce the size of some variables in structs](#8-slot-packing---proposal-reduce-the-size-of-some-variables-in-structs)
- [9. (Should be in C4udit) - Reduce the size of error messages (Long revert Strings)](#9-should-be-in-c4udit---reduce-the-size-of-error-messages-long-revert-strings)

## 1. (2100 fixed gas + 2100-4200 gas per iteration) `struct PriceInformation` is misread as `memory` instead of `storage`

This is `struct PriceInformation`:

```solidity
File: NFTFloorOracle.sol
23: struct PriceInformation {
24:     // last reported floor price(offchain twap)
25:     uint256 twap;
26:     // last updated blocknumber
27:     uint256 updatedAt;
28:     // last updated timestamp
29:     uint256 updatedTimestamp;
30: }
```

When copying a state struct in memory, there are as many SLOADs and MSTOREs as there are slots (so, here, a `memory` copy would read 3 SLOTS). Sometimes, this is too much, just like described in the cases below:

- [NFTFloorOracle.sol#L351-L374](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L351-L374): using `storage` vs `memory` would save **2100 gas** (as `updatedTimestamp` is never actually read but costs an extra first SLOAD by using `memory`)

```diff
File: NFTFloorOracle.sol
- 357:         PriceInformation memory assetPriceMapEntry = assetPriceMap[_asset];  //@audit use storage instead of memory here
+ 357:         PriceInformation storage assetPriceMapEntry = assetPriceMap[_asset]; 
358:         uint256 _priorTwap = assetPriceMapEntry.twap;
359:         uint256 _updatedAt = assetPriceMapEntry.updatedAt;
```

- [NFTFloorOracle.sol#L414-L423](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L414-L423): using `storage` vs `memory` would save **between 2100 gas and 4200 gas per iteration** (as `updatedTimestamp` is never actually read in this for-loop and `twap` could be left unread under condition)

```diff
File: NFTFloorOracle.sol
413:         for (uint256 i = 0; i < feederSize; i++) {
- 414:             PriceInformation memory priceInfo = feederRegistrar.feederPrice[ //@audit this costs 3 full SLOADs to copy into memory
+ 414:             PriceInformation storage priceInfo = feederRegistrar.feederPrice[
415:                 feeders[i]
416:             ];
417:             if (priceInfo.updatedAt > 0) {
418:                 uint256 diffBlock = currentBlock - priceInfo.updatedAt; 
419:                 if (diffBlock <= config.expirationPeriod) {
420:                     validPriceList[validNum] = priceInfo.twap; //@audit this is not always read
421:                     validNum++;
422:                 }
423:             }
424:         }
```

## 2. (200 gas per iteration) - Avoid storage readings in for-loops

[2 variables](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L109-L114) are read in a for-loop repeatedly. Consider caching them before the for-loop to save around **200 gas per iteration**:

```diff
File: NTokenApeStaking.sol
102:     function burn(
103:         address from,
104:         address receiverOfUnderlying,
105:         uint256[] calldata tokenIds
106:     ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
+ 107:         IPool pool_ = POOL;
+ 107:         address underlyingAsset_ = _underlyingAsset;
107:         for (uint256 index = 0; index < tokenIds.length; index++) {
108:             ApeStakingLogic.executeUnstakePositionAndRepay(
109:                 _ERC721Data.owners, //@audit-ok : the library actually expects a storage variable
110:                 apeStakingDataStorage(),
111:                 ApeStakingLogic.UnstakeAndRepayParams({
- 112:                     POOL: POOL, //@audit this is a SLOAD in a for-loop
+ 112:                     POOL: pool_,
113:                     _apeCoinStaking: _apeCoinStaking,
- 114:                     _underlyingAsset: _underlyingAsset, //@audit this is a SLOAD in a for-loop
+ 114:                     _underlyingAsset: underlyingAsset_,
115:                     poolId: POOL_ID(),
116:                     tokenId: tokenIds[index],
117:                     incentiveReceiver: address(0)
118:                 })
119:             );
120:         }
```

## 3. (168 fixed gas + 84 gas per iteration) - Multiple accesses of a mapping/array should use a local variable cache

Caching a mapping's value in a local `storage` or `calldata` variable when the value is accessed multiple times saves **~42 gas per access** due to not having to perform the same offset calculation every time.

Affected code:

- **storage in NFTFloorOracle.sol** (42 gas * 4 access = 168 gas)

```solidity
  242:         uint256 updatedAt = assetPriceMap[_asset].updatedAt; //@audit gas: use PriceInformation storage
  247:         return assetPriceMap[_asset].twap; //@audit gas: use PriceInformation storage
```

```solidity
  282:         assetFeederMap[_asset].registered = true; //@audit gas: use FeederRegistrar storage
  284:         assetFeederMap[_asset].index = uint8(assets.length - 1); //@audit gas: use FeederRegistrar storage
```

```solidity
  312:         feederPositionMap[_feeder].index = uint8(feeders.length - 1); //@audit gas: use FeederPosition storage
  313:         feederPositionMap[_feeder].registered = true; //@audit gas: use FeederPosition storage
```

```solidity
  405:         if (assetPriceMap[_asset].twap == 0) {  //@audit gas: use PriceInformation storage
  426:             return (false, assetPriceMap[_asset].twap); //@audit gas: use PriceInformation storage
```

- **calldata in [WPunkGateway.sol](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L77-L87)** (42 gas * 2 access per iteration)

```diff
77:     function supplyPunk(
78:         DataTypes.ERC721SupplyParams[] calldata punkIndexes,
79:         address onBehalfOf,
80:         uint16 referralCode
81:     ) external nonReentrant {
82:         for (uint256 i = 0; i < punkIndexes.length; i++) {
+ 82:             DataTypes.ERC721SupplyParams calldata punk_ = punkIndexes[i];
- 83:             Punk.buyPunk(punkIndexes[i].tokenId);
+ 83:             Punk.buyPunk(punk_.tokenId);
- 84:             Punk.transferPunk(proxy, punkIndexes[i].tokenId);
+ 84:             Punk.transferPunk(proxy, punk_.tokenId);
85:             // gatewayProxy is the sender of this function, not the original gateway
- 86:             WPunk.mint(punkIndexes[i].tokenId);
+ 86:             WPunk.mint(punk_.tokenId);
87:         }
```

## 4. (25 gas per iteration) - Unchecking arithmetics operations that can't underflow/overflow

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn't possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an `unchecked` block: <https://docs.soliditylang.org/en/v0.8.10/control-structures.html#checked-or-unchecked-arithmetic>

Consider wrapping with an `unchecked` block here (around **25 gas saved** per instance):

```solidity
File: GenericLogic.sol
313:         if (availableBorrowsInBaseCurrency < totalDebtInBaseCurrency) {
314:             return 0;
315:         }
316:        //@audit below can be unchecked as otherwise it would've returned 0 above
317:         availableBorrowsInBaseCurrency = 
318:             availableBorrowsInBaseCurrency -
319:             totalDebtInBaseCurrency;
```

```solidity
File: PoolCore.sol
634:         for (uint256 i = 0; i < reservesListCount; i++) {
635:             if (ps._reservesList[i] != address(0)) {
636:                 reservesList[i - droppedReservesCount] = ps._reservesList[i]; //@audit can be unchecked as droppedReservesCount is at most i
637:             } else {
638:                 droppedReservesCount++; //@audit can be unchecked
639:             }
640:         }
```

```solidity
File: ApeStakingLogic.sol
182:         if (repayAmount > 0) {
183:             repayAmount = Math.min(repayAmount, unstakedAmount);
184:         }
185:         uint256 supplyAmount = unstakedAmount - repayAmount; //@audit can be unchecked due to L182-183
```

Similarly (odd that C4udit isn't catching those yet), increments can be unchecked in for-loops:

```solidity
contracts/misc/NFTFloorOracle.sol:229:        for (uint256 i = 0; i < _assets.length; i++) {
contracts/misc/NFTFloorOracle.sol:291:        for (uint256 i = 0; i < _assets.length; i++) {
contracts/misc/NFTFloorOracle.sol:321:        for (uint256 i = 0; i < _feeders.length; i++) {
contracts/misc/NFTFloorOracle.sol:413:        for (uint256 i = 0; i < feederSize; i++) {
contracts/misc/ParaSpaceOracle.sol:95:        for (uint256 i = 0; i < assets.length; i++) {
contracts/misc/ParaSpaceOracle.sol:197:        for (uint256 i = 0; i < assets.length; i++) {
contracts/misc/UniswapV3OracleWrapper.sol:193:        for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/misc/UniswapV3OracleWrapper.sol:210:        for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/ui/WPunkGateway.sol:82:        for (uint256 i = 0; i < punkIndexes.length; i++) {
contracts/ui/WPunkGateway.sol:109:        for (uint256 i = 0; i < punkIndexes.length; i++) {
contracts/ui/WPunkGateway.sol:113:        for (uint256 i = 0; i < punkIndexes.length; i++) {
contracts/ui/WPunkGateway.sol:136:        for (uint256 i = 0; i < punkIndexes.length; i++) {
contracts/ui/WPunkGateway.sol:174:        for (uint256 i = 0; i < punkIndexes.length; i++) {
contracts/protocol/libraries/logic/FlashClaimLogic.sol:38:        for (i = 0; i < params.nftTokenIds.length; i++) {
contracts/protocol/libraries/logic/FlashClaimLogic.sol:56:        for (i = 0; i < params.nftTokenIds.length; i++) {
contracts/protocol/libraries/logic/GenericLogic.sol:371:            for (uint256 index = 0; index < totalBalance; index++) {
contracts/protocol/libraries/logic/GenericLogic.sol:466:        for (uint256 index = 0; index < totalBalance; index++) {
contracts/protocol/libraries/logic/MarketplaceLogic.sol:177:        for (uint256 i = 0; i < marketplaceIds.length; i++) {
contracts/protocol/libraries/logic/MarketplaceLogic.sol:284:        for (uint256 i = 0; i < marketplaceIds.length; i++) {
contracts/protocol/libraries/logic/MarketplaceLogic.sol:382:        for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
contracts/protocol/libraries/logic/MarketplaceLogic.sol:470:        for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
contracts/protocol/libraries/logic/PoolLogic.sol:58:        for (uint16 i = 0; i < params.reservesCount; i++) {
contracts/protocol/libraries/logic/PoolLogic.sol:104:        for (uint256 i = 0; i < assets.length; i++) {
contracts/protocol/libraries/logic/SupplyLogic.sol:184:            for (uint256 index = 0; index < params.tokenData.length; index++) {
contracts/protocol/libraries/logic/SupplyLogic.sol:195:        for (uint256 index = 0; index < params.tokenData.length; index++) {
contracts/protocol/libraries/logic/ValidationLogic.sol:131:        for (uint256 index = 0; index < amount; index++) {
contracts/protocol/libraries/logic/ValidationLogic.sol:212:            for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/libraries/logic/ValidationLogic.sol:465:            for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/libraries/logic/ValidationLogic.sol:910:                for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/libraries/logic/ValidationLogic.sol:1034:        for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
contracts/protocol/tokenization/NToken.sol:106:            for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/tokenization/NToken.sol:145:        for (uint256 i = 0; i < ids.length; i++) {
contracts/protocol/tokenization/NTokenApeStaking.sol:107:        for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/tokenization/NTokenMoonBirds.sol:51:            for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/tokenization/NTokenMoonBirds.sol:97:        for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:213:        for (uint256 index = 0; index < totalBalance; index++) {
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:207:        for (uint256 index = 0; index < tokenData.length; index++) {
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:280:        for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:493:        for (uint256 index = 0; index < tokenIds.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:72:        for (uint256 index = 0; index < _nfts.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:103:        for (uint256 index = 0; index < _nfts.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:138:        for (uint256 index = 0; index < _nftPairs.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:172:        for (uint256 index = 0; index < actualTransferAmount; index++) {
contracts/protocol/pool/PoolApeStaking.sol:199:        for (uint256 index = 0; index < _nftPairs.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:215:        for (uint256 index = 0; index < _nftPairs.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:279:        for (uint256 index = 0; index < _nfts.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:289:        for (uint256 index = 0; index < _nftPairs.length; index++) {
contracts/protocol/pool/PoolApeStaking.sol:305:        for (uint256 index = 0; index < _nftPairs.length; index++) {
contracts/protocol/pool/PoolCore.sol:634:        for (uint256 i = 0; i < reservesListCount; i++) {
```

The change would be:  
  
```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

## 5. (33 gas per iteration) - Usage of `uint16` incurs overhead in for-loop

- <https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html> :

> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Each operation involving a `uint16` costs an extra **33 gas** as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint16`, as well as the associated stack operations of doing so.

**POC with 200 Optimizer runs**:

```solidity
    function test() external view returns (uint16) { 
        uint16 varr = uint16(block.number >> 255);
        varr = varr + 1; // 21464 gas
        varr = varr + 2; // 21572 (adds 108 gas)
        return varr;
    }

    function test2() external view returns (uint16) {
        uint256 varr = block.number >> 255;
        varr = varr + 1; // 21460 gas
        varr = varr + 2; // 21535 (adds 75 gas)
        return uint8(varr);
    }
```

Use a larger size then downcast where needed:

```diff
File: PoolLogic.sol
- 58:         for (uint16 i = 0; i < params.reservesCount; i++) {
+ 58:         for (uint256 i = 0; i < params.reservesCount; i++) { //@audit reservesCount is uint16 so "i" can't go above 
59:             if (reservesList[i] == address(0)) {
- 60:                 reservesData[params.asset].id = i;
+ 60:                 reservesData[params.asset].id = uint16(i);
61:                 reservesList[i] = params.asset;
62:                 return false;
63:             }
64:         }
```

## 6. (100 gas) - Missed by C4udit: State variables should be cached in stack variables rather than re-reading them from storage

Link to C4udit's output: <https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba#gas-4-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage>

The following line is missing and can save **100 gas**:

```diff
File: NTokenApeStaking.sol
36:     function initialize(
...
+ 45:         IPool pool_ = POOL;
- 46:         _apeCoin.approve(address(POOL), type(uint256).max); //@audit SLOAD 1 (POOL)
+ 46:         _apeCoin.approve(address(pool_), type(uint256).max);
- 47:         getBAKC().setApprovalForAll(address(POOL), true); //@audit SLOAD 2 (POOL)
+ 47:         getBAKC().setApprovalForAll(address(pool_), true);
```

## 7. (300 gas) - Avoid emitting a storage variable when a memory value is available

When they are the same, consider emitting the memory value instead of the storage value:

- (**200 gas**) [NFTFloorOracle.sol#L383-L384](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L383-L384)

```diff
376:     function _finalizePrice(address _asset, uint256 _twap) internal {
377:         PriceInformation storage assetPriceMapEntry = assetPriceMap[_asset];
378:         assetPriceMapEntry.twap = _twap;
379:         assetPriceMapEntry.updatedAt = block.number;
380:         assetPriceMapEntry.updatedTimestamp = block.timestamp;
381:         emit AssetDataSet(
382:             _asset,
- 383:             assetPriceMapEntry.twap, //@audit-issue gas: emit _twap
+ 383:             _twap
- 384:             assetPriceMapEntry.updatedAt //@audit-issue gas: emit block.number
+ 384:             block.number
385:         );
386:     }
```

- (**100 gas**) [NToken.sol#L68-L75](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L68-L75)

```diff
52:     function initialize(
53:         IPool initializingPool,
...
60:         require(initializingPool == POOL, Errors.POOL_ADDRESSES_DO_NOT_MATCH);
...
68:         emit Initialized(
69:             underlyingAsset,
- 70:             address(POOL), //@audit should emit initializingPool to avoid a SLOAD
+ 70:             address(initializingPool)
71:             address(incentivesController),
72:             nTokenName,
73:             nTokenSymbol,
74:             params
75:         );
```

## 8. (Slot packing) - Proposal: reduce the size of some variables in structs

The following variables can probably be smaller and packed (as an example: decimals would just need to be `uint8` and timestamps `uint48`):

```diff
paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol:
  32      struct CalculateUserAccountDataVars {
  33          uint256 assetPrice;
  34          uint256 assetUnit;
  35          DataTypes.ReserveConfigurationMap reserveConfiguration;
  36          uint256 userBalanceInBaseCurrency;
-  37:         uint256 decimals;//@audit : gas should be uint8  
  38          uint256 ltv;
  39          uint256 liquidationThreshold;
  40          uint256 liquidationBonus;
  41          uint256 i;
  42          uint256 healthFactor;
  43          uint256 erc721HealthFactor;
  44          uint256 totalERC721CollateralInBaseCurrency;
  45          uint256 payableDebtByERC20Assets;
  46          uint256 totalCollateralInBaseCurrency;
  47          uint256 totalDebtInBaseCurrency;
  48          uint256 avgLtv;
  49          uint256 avgLiquidationThreshold;
  50          uint256 avgERC721LiquidationThreshold;
+  51:         uint8 decimals;  
  51          address currentReserveAddress;
  52          bool hasZeroLtvCollateral;
  53          address xTokenAddress;
  54          XTokenType xTokenType;
  55      }
  56  

paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol:
  132      struct LiquidateParametersLocalVars {
  133          uint256 userCollateral;
  134          uint256 collateralPrice;
  135          uint256 liquidationAssetPrice;
-  136:         uint256 liquidationAssetDecimals;//@audit : gas should be uint8  
+  136:         uint8 liquidationAssetDecimals;
-  137:         uint256 collateralDecimals;//@audit : gas should be uint8
+  137:         uint8 collateralDecimals
+  137:         uint48 auctionStartTime
  138          uint256 collateralAssetUnit;
  139          uint256 liquidationAssetUnit;
  140          uint256 actualCollateralToLiquidate;
  141          uint256 actualLiquidationAmount;
  142          uint256 actualLiquidationBonus;
  143          uint256 liquidationProtocolFeePercentage;
  144          uint256 liquidationProtocolFee;
  145          // Auction related
  146          uint256 auctionMultiplier;
-  147:         uint256 auctionStartTime;//@audit : gas should be uint48
  148      }

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:
  225      struct ValidateBorrowLocalVars {
  226          uint256 currentLtv;
  227          uint256 collateralNeededInBaseCurrency;
  228          uint256 userCollateralInBaseCurrency;
  229          uint256 userDebtInBaseCurrency;
  230          uint256 availableLiquidity;
  231          uint256 healthFactor;
  232          uint256 totalDebt;
  233          uint256 totalSupplyVariableDebt;
-   234:         uint256 reserveDecimals;//@audit : gas should be uint8  
  235          uint256 borrowCap;
  236          uint256 amountInBaseCurrency;
  237          uint256 assetUnit;
+   238:         uint8 reserveDecimals;
  238          address siloedBorrowingAddress;
  239          bool isActive;
  240          bool isFrozen;
  241          bool isPaused;
  242          bool borrowingEnabled;
  243          bool siloedBorrowingEnabled;
  244          DataTypes.AssetType assetType;
  245      }

```

## 9. (Should be in C4udit) - Reduce the size of error messages (Long revert Strings)

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

Revert strings > 32 bytes:

```solidity
contracts/misc/NFTFloorOracle.sol:227:            "NFTOracle: Tokens and price length differ"
contracts/misc/NFTFloorOracle.sol:356:        require(_twap > 0, "NFTOracle: price should be more than 0");
contracts/protocol/tokenization/NTokenMoonBirds.sol:100:                "ERC721: transfer caller is not owner nor approved"
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:90:            "ERC721: transfer from incorrect owner"
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:92:        require(to != address(0), "ERC721: transfer to the zero address");
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:379:            "ERC721: startAuction for nonexistent token"
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:397:            "ERC721: endAuction for nonexistent token"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:197:            "ERC721: approve caller is not owner nor approved for all"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:215:            "ERC721: approved query for nonexistent token"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:261:            "ERC721: transfer caller is not owner nor approved"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:298:            "ERC721: transfer caller is not owner nor approved"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:356:            "ERC721: operator query for nonexistent token"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:596:            "ERC721Enumerable: owner index out of bounds"
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:620:            "ERC721Enumerable: global index out of bounds"
```

Consider shortening the revert strings to fit in 32 bytes.
