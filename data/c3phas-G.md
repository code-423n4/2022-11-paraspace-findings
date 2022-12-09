## FINDINGS
NB: *Some functions have been truncated where neccessary to just show affected parts of the code*

### Declare a constant for the type max rather than evaluate it everytime
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L45-L46
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
45:        _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
46        _apeCoin.approve(address(POOL), type(uint256).max);
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
index c570021..3fd6247 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
@@ -24,6 +24,7 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
         bytes32(
             uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
         );
+        uint256 constant UINT_MAX = type(uint256).max;

     /**
      * @dev Constructor.
@@ -42,8 +43,8 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
         bytes calldata params
     ) public virtual override initializer {
         IERC20 _apeCoin = _apeCoinStaking.apeCoin();
-        _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
-        _apeCoin.approve(address(POOL), type(uint256).max);
+        _apeCoin.approve(address(_apeCoinStaking), UINT_MAX);
+        _apeCoin.approve(address(POOL), UINT_MAX);
         getBAKC().setApprovalForAll(address(POOL), true);

```
### Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

NB: *Some functions have been truncated where neccessary to just show affected parts of the code*

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L78-L97
### NTokenApeStaking.sol.\_transfer(): \_underlyingAsset should be cached
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
78:    function _transfer(

90:                _underlyingAsset: _underlyingAsset,

97:    }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
index c570021..5230627 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
@@ -81,13 +81,14 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
         uint256 tokenId,
         bool validate
     ) internal override {
+        address asset = _underlyingAsset;
         ApeStakingLogic.executeUnstakePositionAndRepay(
             _ERC721Data.owners,
             apeStakingDataStorage(),
             ApeStakingLogic.UnstakeAndRepayParams({
                 POOL: POOL,
                 _apeCoinStaking: _apeCoinStaking,
-                _underlyingAsset: _underlyingAsset,
+                asset: asset,
                 poolId: POOL_ID(),
                 tokenId: tokenId,
                 incentiveReceiver: address(0)
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102-L123
### NTokenApeStaking.sol.burn(): \_underlyingAsset should be cached
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
102:    function burn(
    
114:                    _underlyingAsset: _underlyingAsset,

123:    }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
index c570021..0dac438 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
@@ -104,6 +104,7 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
         address receiverOfUnderlying,
         uint256[] calldata tokenIds
     ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
+        address asset = _underlyingAsset;
         for (uint256 index = 0; index < tokenIds.length; index++) {
             ApeStakingLogic.executeUnstakePositionAndRepay(
                 _ERC721Data.owners,
@@ -111,7 +112,7 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
                 ApeStakingLogic.UnstakeAndRepayParams({
                     POOL: POOL,
                     _apeCoinStaking: _apeCoinStaking,
-                    _underlyingAsset: _underlyingAsset,
+                    asset: asset,
                     poolId: POOL_ID(),
                     tokenId: tokenIds[index],
                     incentiveReceiver: address(0)
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L159-L176
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
159:    function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)

170:                _underlyingAsset: _underlyingAsset,

176:    }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
index c570021..95c2dd1 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
@@ -161,13 +161,14 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
         onlyPool
         nonReentrant
     {
+        address asset = _underlyingAsset;
         ApeStakingLogic.executeUnstakePositionAndRepay(
             _ERC721Data.owners,
             apeStakingDataStorage(),
             ApeStakingLogic.UnstakeAndRepayParams({
                 POOL: POOL,
                 _apeCoinStaking: _apeCoinStaking,
-                _underlyingAsset: _underlyingAsset,
+                asset: asset,
                 poolId: POOL_ID(),
                 tokenId: tokenId,
                 incentiveReceiver: incentiveReceiver
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L95-L116
### NToken.sol.\_burn(): \_underlyingAsset should be cached outside the loop
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NToken.sol
95:    function _burn(

105:        if (receiverOfUnderlying != address(this)) {
106:            for (uint256 index = 0; index < tokenIds.length; index++) {
107:                IERC721(_underlyingAsset).safeTransferFrom(
108:                    address(this),
109:                    receiverOfUnderlying,
110:                    tokenIds[index]
111:                );
112:            }
113:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NToken.sol b/paraspace-core/contracts/protocol/tokenization/NToken.sol
index 5a23c73..66b8b5d 100644
--- a/paraspace-core/contracts/protocol/tokenization/NToken.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NToken.sol
@@ -103,8 +103,9 @@ contract NToken is VersionedInitializable, MintableIncentivizedERC721, INToken {
         ) = _burnMultiple(from, tokenIds);

         if (receiverOfUnderlying != address(this)) {
+            address asset = _underlyingAsset;
             for (uint256 index = 0; index < tokenIds.length; index++) {
-                IERC721(_underlyingAsset).safeTransferFrom(
+                IERC721(asset).safeTransferFrom(
                     address(this),
                     receiverOfUnderlying,
                     tokenIds[index]
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L474-L506
```solidity
File: /paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
474:    function batchSetIsUsedAsCollateral(

489:        oldCollateralizedBalance = _ERC721Data
490:            .userState[sender]
491:            .collateralizedBalance;


493:        for (uint256 index = 0; index < tokenIds.length; index++) {
494:            MintableERC721Logic.executeSetIsUsedAsCollateral(
495:                _ERC721Data,
496:                POOL,
497:                tokenIds[index],
498:                useAsCollateral,
499:                sender
500:            );
501:        }

503:        newCollateralizedBalance = _ERC721Data
504:            .userState[sender]
505:            .collateralizedBalance;
506:    }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol b/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
index 61438db..87bf4fc 100644
--- a/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
+++ b/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
@@ -486,13 +486,14 @@ abstract contract MintableIncentivizedERC721 is
             uint256 newCollateralizedBalance
         )
     {
-        oldCollateralizedBalance = _ERC721Data
+        MintableERC721Data _ERCDATA = _ERC721Data;
+        oldCollateralizedBalance = _ERCDATA
             .userState[sender]
             .collateralizedBalance;

         for (uint256 index = 0; index < tokenIds.length; index++) {
             MintableERC721Logic.executeSetIsUsedAsCollateral(
-                _ERC721Data,
+                _ERCDATA,
                 POOL,
                 tokenIds[index],
                 useAsCollateral,
@@ -500,7 +501,7 @@ abstract contract MintableIncentivizedERC721 is
             );
         }

-        newCollateralizedBalance = _ERC721Data
+        newCollateralizedBalance = _ERCDATA
             .userState[sender]
             .collateralizedBalance;
     }
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L130-L132
### ParaSpaceOracle.sol.getAssetPrice(): \_fallbackOracle should be cached
```solidity
File: /paraspace-core/contracts/misc/ParaSpaceOracle.sol
130:        if (price == 0 && address(_fallbackOracle) != address(0)) {
131:            price = _fallbackOracle.getAssetPrice(asset);
        }
```

```diff
diff --git a/paraspace-core/contracts/misc/ParaSpaceOracle.sol b/paraspace-core/contracts/misc/ParaSpaceOracle.sol
index fbaf52d..9071a5c 100644
--- a/paraspace-core/contracts/misc/ParaSpaceOracle.sol
+++ b/paraspace-core/contracts/misc/ParaSpaceOracle.sol
@@ -127,8 +127,9 @@ contract ParaSpaceOracle is IParaSpaceOracle {
         if (address(source) != address(0)) {
             price = uint256(source.latestAnswer());
         }
-        if (price == 0 && address(_fallbackOracle) != address(0)) {
-            price = _fallbackOracle.getAssetPrice(asset);
+        IPriceOracleGetter  fallbackOracle = _fallbackOracle;
+        if (price == 0 && address(fallbackOracle) != address(0)) {
+            price = fallbackOracle.getAssetPrice(asset);
         }
```
### Internal/Private functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Affected code:

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L51-L58
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol
51:    function _decreaseLiquidity(
52:        address user,
53:        uint256 tokenId,
54:        uint128 liquidityDecrease,
55:        uint256 amount0Min,
56:        uint256 amount1Min,
57:        bool receiveEthAsWeth
58:    ) internal returns (uint256 amount0, uint256 amount1) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L144-L147
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol
144:    function _safeTransferETH(address to, uint256 value) internal {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L302-L307
```solidity
File: /paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol
302:    function _updateParaProxyImpl(
303:        bytes32 id,
304:        IParaProxy.ProxyImplementation[] calldata implementationParams,
305:        address _init,
306:        bytes calldata _calldata
307:    ) internal {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L221-L225
```solidity
File: /paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
221:    function _getOracleData(UinswapV3PositionData memory positionData)
222:        internal
223:        view
224:        returns (PairOracleData memory)
225:    {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L282-L286
```solidity
File: /paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
282:    function _getPendingFeeAmounts(UinswapV3PositionData memory positionData)
283:        internal
284:        view
285:        returns (uint256 token0Amount, uint256 token1Amount)
286:    {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L218
```solidity
File: /paraspace-core/contracts/misc/ParaSpaceOracle.sol
218:    function _onlyAssetListingOrPoolAdmins() internal view {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L265
```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol
265:    function _whenNotPaused(address _asset) internal view {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L278-L281
```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol
278:    function _addAsset(address _asset)
279:        internal
280:        onlyWhenAssetNotExisted(_asset)
281:    {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296-L299
```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol
296:    function _removeAsset(address _asset)
297:        internal
298:        onlyWhenAssetExisted(_asset)
299:    {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L307-L310
```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol
307:    function _addFeeder(address _feeder)
308:        internal
309:        onlyWhenFeederNotExisted(_feeder)
310:    {

326:    function _removeFeeder(address _feeder)
327:        internal
328:        onlyWhenFeederExisted(_feeder)
329:    {
```
### Use calldata instead of memory for function parameters
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

Note that I've also flagged instances where the function is public but can be marked as external since it's not called by the contract, and cases where a constructor is involved

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L38-L43
```solidity
File: /paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
38:    function withdrawBAKC(
39:        ApeCoinStaking _apeCoinStaking,
40:        uint256 poolId,
41:        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
42:        address _apeRecipient
43:    ) external {
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol b/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
index 1e7eb75..d89504e 100644
--- a/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
+++ b/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
@@ -38,7 +38,7 @@ library ApeStakingLogic {
     function withdrawBAKC(
         ApeCoinStaking _apeCoinStaking,
         uint256 poolId,
-        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
+        ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs,
         address _apeRecipient
     ) external {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L92-L96
```solidity
File: /paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
92:    function executeUnstakePositionAndRepay(
93:        mapping(uint256 => address) storage _owners,
94:        APEStakingParameter storage stakingParameter,
95:        UnstakeAndRepayParams memory params
96:    ) external {
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol b/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
index 1e7eb75..8378ec4 100644
--- a/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
+++ b/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
@@ -92,7 +92,7 @@ library ApeStakingLogic {
     function executeUnstakePositionAndRepay(
         mapping(uint256 => address) storage _owners,
         APEStakingParameter storage stakingParameter,
-        UnstakeAndRepayParams memory params
+        UnstakeAndRepayParams calldata params
     ) external {
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-L107
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol
97:    function withdrawBAKC(
98:        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99:        address _apeRecipient
100:    ) external onlyPool nonReentrant {
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol b/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol
index 7b4d90e..fdffb51 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol
@@ -95,7 +95,7 @@ contract NTokenMAYC is NTokenApeStaking {
      * @dev if pairs have split ownership and BAKC is attempting a withdraw, the withdraw must be for the total staked amount
      */
     function withdrawBAKC(
-        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
+        ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs,
         address _apeRecipient
     ) external onlyPool nonReentrant {
```


### Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L632
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolCore.sol
632:        address[] memory reservesList = new address[](reservesListCount);
```

```diff
diff --git a/paraspace-core/contracts/protocol/pool/PoolCore.sol b/paraspace-core/contracts/protocol/pool/PoolCore.sol
index 115a728..2fee477 100644
--- a/paraspace-core/contracts/protocol/pool/PoolCore.sol
+++ b/paraspace-core/contracts/protocol/pool/PoolCore.sol
@@ -629,7 +629,7 @@ contract PoolCore is

         uint256 reservesListCount = ps._reservesCount;
         uint256 droppedReservesCount = 0;
-        address[] memory reservesList = new address[](reservesListCount);
+        address[] storage reservesList = new address[](reservesListCount);

```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L136
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
136:        uint256[] memory transferredTokenIds = new uint256[](_nftPairs.length);
```

```diff
diff --git a/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol b/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
index d9c033d..4ec9397 100644
--- a/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
+++ b/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
@@ -133,7 +133,7 @@ contract PoolApeStaking is
         INTokenApeStaking nTokenApeStaking = INTokenApeStaking(xTokenAddress);
         IERC721 bakcContract = nTokenApeStaking.getBAKC();
         uint256 amountToWithdraw = 0;
-        uint256[] memory transferredTokenIds = new uint256[](_nftPairs.length);
+        uint256[] storage transferredTokenIds = new uint256[](_nftPairs.length);
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L287
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
287:            DataTypes.Credit memory credit = credits[i];
```

```diff
diff --git a/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol b/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
index 36b0a0b..f529e63 100644
--- a/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
+++ b/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
@@ -284,7 +284,7 @@ library MarketplaceLogic {
         for (uint256 i = 0; i < marketplaceIds.length; i++) {
             vars.marketplaceId = marketplaceIds[i];
             vars.payload = payloads[i];
-            DataTypes.Credit memory credit = credits[i];
+            DataTypes.Credit storage credit = credits[i];

             vars.marketplace = poolAddressProvider.getMarketplace(
                 vars.marketplaceId
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L383
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
383:            ConsiderationItem memory item = params.orderInfo.consideration[i];
```

```diff
diff --git a/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol b/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
index 36b0a0b..65a7f08 100644
--- a/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
+++ b/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
@@ -380,7 +380,7 @@ library MarketplaceLogic {
         uint256 price = 0;

         for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
-            ConsiderationItem memory item = params.orderInfo.consideration[i];
+            ConsiderationItem storage item = params.orderInfo.consideration[i];
             require(
                 item.startAmount == item.endAmount,
                 Errors.INVALID_MARKETPLACE_ORDER
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L471
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
471:            OfferItem memory item = params.orderInfo.offer[i];
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L601
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
601:        uint256[] memory tokenIds = new uint256[](1);
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L470
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol
470:        uint256[] memory tokenIds = new uint256[](1);
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L212
```solidity
File: /paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol
212:        DataTypes.Marketplace memory marketplace = _marketplaces[id];
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L191
```solidity
File:/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
191:        uint256[] memory prices = new uint256[](tokenIds.length);
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L357
```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol
357:        PriceInformation memory assetPriceMapEntry = assetPriceMap[_asset];
```
### Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

   When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L77-L81
```solidity
File: /paraspace-core/contracts/ui/WPunkGateway.sol

//@audit: uint16 referralCode
77:    function supplyPunk(
78:        DataTypes.ERC721SupplyParams[] calldata punkIndexes,
79:        address onBehalfOf,
80:        uint16 referralCode
81:    ) external nonReentrant {

//@audit: uint16 referralCode
129:    function acceptBidWithCredit(
130:        bytes32 marketplaceId,
131:        bytes calldata payload,
132:        DataTypes.Credit calldata credit,
133:        uint256[] calldata punkIndexes,
134:        uint16 referralCode
135:    ) external nonReentrant {

//@audit: uint16 referralCode
167:    function batchAcceptBidWithCredit(
168:        bytes32[] calldata marketplaceIds,
169:        bytes[] calldata payloads,
170:        DataTypes.Credit[] calldata credits,
171:        uint256[] calldata punkIndexes,
172:        uint16 referralCode
173:    ) external nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L51-L58
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

//@audit: uint128 liquidityDecrease
51:    function _decreaseLiquidity(
52:        address user,
53:        uint256 tokenId,
54:        uint128 liquidityDecrease,
55:        uint256 amount0Min,
56:        uint256 amount1Min,
57:        bool receiveEthAsWeth
58:    ) internal returns (uint256 amount0, uint256 amount1) {

//@audit: uint128 liquidityDecrease
123:    function decreaseUniswapV3Liquidity(
124:        address user,
125:        uint256 tokenId,
126:        uint128 liquidityDecrease,
127:        uint256 amount0Min,
128:        uint256 amount1Min,
129:        bool receiveEthAsWeth
130:    ) external onlyPool nonReentrant {
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142-L144
```solidity
File: /paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

//@audit: uint64 limit
142:    function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L650
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolCore.sol

//@audit: uint16 id
650:    function getReserveAddressById(uint16 id) external view returns (address) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L229-L237
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

//@audit: uint16 referralCode
229:    function executeAcceptBidWithCredit(
230:        bytes32 marketplaceId,
231:        bytes calldata payload,
232:        DataTypes.Credit calldata credit,
233:        address onBehalfOf,
234:        DataTypes.PoolStorage storage ps,
235:        IPoolAddressesProvider poolAddressProvider,
236:        uint16 referralCode
237:    ) external {


//@audit: uint16 referralCode
267:    function executeBatchAcceptBidWithCredit(
268:        bytes32[] calldata marketplaceIds,
269:        bytes[] calldata payloads,
270:        DataTypes.Credit[] calldata credits,
271:        address onBehalfOf,
272:        DataTypes.PoolStorage storage ps,
273:        IPoolAddressesProvider poolAddressProvider,
274:        uint16 referralCode
275:    ) external {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L175
```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol

//@audit: uint128 expirationPeriod
//@audit: uint128 maxPriceDeviation
175:    function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)

//@audit: uint128 expirationPeriod
//@audit: uint128 maxPriceDeviation
343:    function _setConfig(uint128 _expirationPeriod, uint128 _maxPriceDeviation)
```

### Using unchecked blocks to save gas - Increments in for loop can be unchecked  ( save 30-40 gas per loop iteration)
The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop . eg.

e.g Let's work with a sample loop below.

```solidity
for(uint256 i; i < 10; i++){
//doSomething
}

```
can be written as shown below.
```solidity
for(uint256 i; i < 10;) {
  // loop logic
  unchecked { i++; }
}
```

We can also write  it as an inlined function like below.

```solidity
function inc(i) internal pure returns (uint256) {
  unchecked { return i + 1; }
}
for(uint256 i; i < 10; i = inc(i)) {
  // doSomething
}
```

**Affected code**
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82-L87
```solidity
File: /paraspace-core/contracts/ui/WPunkGateway.sol
82:        for (uint256 i = 0; i < punkIndexes.length; i++) {

87:        }
```

```diff
diff --git a/paraspace-core/contracts/ui/WPunkGateway.sol b/paraspace-core/contracts/ui/WPunkGateway.sol
index 39ae88d..e6be133 100644
--- a/paraspace-core/contracts/ui/WPunkGateway.sol
+++ b/paraspace-core/contracts/ui/WPunkGateway.sol
@@ -79,11 +79,15 @@ contract WPunkGateway is
         address onBehalfOf,
         uint16 referralCode
     ) external nonReentrant {
-        for (uint256 i = 0; i < punkIndexes.length; i++) {
+        for (uint256 i = 0; i < punkIndexes.length;) {
             Punk.buyPunk(punkIndexes[i].tokenId);
             Punk.transferPunk(proxy, punkIndexes[i].tokenId);
             // gatewayProxy is the sender of this function, not the original gateway
             WPunk.mint(punkIndexes[i].tokenId);
+
+            unchecked {
+                ++i;
+            }
         }

```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L109-L116
```solidity
File:/paraspace-core/contracts/ui/WPunkGateway.sol
109:        for (uint256 i = 0; i < punkIndexes.length; i++) {
110:            nWPunk.safeTransferFrom(msg.sender, address(this), punkIndexes[i]);
111:        }
112:        Pool.withdrawERC721(address(WPunk), punkIndexes, address(this));
113:        for (uint256 i = 0; i < punkIndexes.length; i++) {
114:            WPunk.burn(punkIndexes[i]);
115:            Punk.transferPunk(to, punkIndexes[i]);
116:        }      }
```

```diff
diff --git a/paraspace-core/contracts/ui/WPunkGateway.sol b/paraspace-core/contracts/ui/WPunkGateway.sol
index 39ae88d..287365a 100644
--- a/paraspace-core/contracts/ui/WPunkGateway.sol
+++ b/paraspace-core/contracts/ui/WPunkGateway.sol
@@ -106,13 +106,19 @@ contract WPunkGateway is
         INToken nWPunk = INToken(
             Pool.getReserveData(address(WPunk)).xTokenAddress
         );
-        for (uint256 i = 0; i < punkIndexes.length; i++) {
+        for (uint256 i = 0; i < punkIndexes.length;) {
             nWPunk.safeTransferFrom(msg.sender, address(this), punkIndexes[i]);
+            unchecked {
+                ++i;
+            }
         }
         Pool.withdrawERC721(address(WPunk), punkIndexes, address(this));
-        for (uint256 i = 0; i < punkIndexes.length; i++) {
+        for (uint256 i = 0; i < punkIndexes.length;) {
             WPunk.burn(punkIndexes[i]);
             Punk.transferPunk(to, punkIndexes[i]);
+            unchecked {
+                ++i;
+            }
         }
     }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L136-L147
```solidity
File: /paraspace-core/contracts/ui/WPunkGateway.sol
136:        for (uint256 i = 0; i < punkIndexes.length; i++) {

147:        }
```

```diff
diff --git a/paraspace-core/contracts/ui/WPunkGateway.sol b/paraspace-core/contracts/ui/WPunkGateway.sol
index 39ae88d..1dd0ee6 100644
--- a/paraspace-core/contracts/ui/WPunkGateway.sol
+++ b/paraspace-core/contracts/ui/WPunkGateway.sol
@@ -133,7 +133,7 @@ contract WPunkGateway is
         uint256[] calldata punkIndexes,
         uint16 referralCode
     ) external nonReentrant {
-        for (uint256 i = 0; i < punkIndexes.length; i++) {
+        for (uint256 i = 0; i < punkIndexes.length;) {
             Punk.buyPunk(punkIndexes[i]);
             Punk.transferPunk(proxy, punkIndexes[i]);
             // gatewayProxy is the sender of this function, not the original gateway
@@ -144,6 +144,10 @@ contract WPunkGateway is
                 msg.sender,
                 punkIndexes[i]
             );
+
+            unchecked {
+                ++i;
+            }
         }
```


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213-L220
```solidity
File:/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
213:        for (uint256 index = 0; index < totalBalance; index++) {

220:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol b/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
index 1e7eb75..b2d1f20 100644
--- a/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
+++ b/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
@@ -210,13 +210,17 @@ library ApeStakingLogic {
     ) external view returns (uint256) {
         uint256 totalBalance = uint256(userState[user].balance);
         uint256 totalAmount;
-        for (uint256 index = 0; index < totalBalance; index++) {
+        for (uint256 index = 0; index < totalBalance;) {
             uint256 tokenId = ownedTokens[user][index];
             totalAmount += getTokenIdStakingAmount(
                 poolId,
                 _apeCoinStaking,
                 tokenId
             );
+
+            unchecked {
+                ++index;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207-L238
```solidity
File:/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
207:        for (uint256 index = 0; index < tokenData.length; index++) {

238:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol b/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
index e8345fa..671c45e 100644
--- a/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
+++ b/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
@@ -204,7 +204,7 @@ library MintableERC721Logic {
         uint256 oldTotalSupply = erc721Data.allTokens.length;
         uint64 collateralizedTokens = 0;

-        for (uint256 index = 0; index < tokenData.length; index++) {
+        for (uint256 index = 0; index < tokenData.length;) {
             uint256 tokenId = tokenData[index].tokenId;

             require(
@@ -235,6 +235,9 @@ library MintableERC721Logic {
             }

             emit Transfer(address(0), to, tokenId);
+            unchecked {
+                ++index;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280-L316
```solidity
File:/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
280:        for (uint256 index = 0; index < tokenIds.length; index++) {

316:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol b/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
index e8345fa..43975be 100644
--- a/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
+++ b/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
@@ -277,7 +277,7 @@ library MintableERC721Logic {
             .userState[user]
             .collateralizedBalance;

-        for (uint256 index = 0; index < tokenIds.length; index++) {
+        for (uint256 index = 0; index < tokenIds.length;) {
             uint256 tokenId = tokenIds[index];
             address owner = erc721Data.owners[tokenId];
             require(owner == user, "not the owner of Ntoken");
@@ -313,6 +313,9 @@ library MintableERC721Logic {
                 burntCollateralizedTokens++;
             }
             emit Transfer(owner, address(0), tokenId);
+            unchecked {
+                ++index;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51-L57
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
51:            for (uint256 index = 0; index < tokenIds.length; index++) {

57:            }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol b/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
index 3de3155..04f3d8e 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
@@ -48,12 +48,15 @@ contract NTokenMoonBirds is NToken, IMoonBirdBase {
         ) = _burnMultiple(from, tokenIds);

         if (receiverOfUnderlying != address(this)) {
-            for (uint256 index = 0; index < tokenIds.length; index++) {
+            for (uint256 index = 0; index < tokenIds.length;) {
                 IMoonBird(_underlyingAsset).safeTransferWhileNesting(
                     address(this),
                     receiverOfUnderlying,
                     tokenIds[index]
                 );
+                unchecked {
+                    ++index;
+                }
             }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97-L102
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
97:        for (uint256 index = 0; index < tokenIds.length; index++) {

102:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol b/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
index 3de3155..6241463 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
@@ -94,11 +94,14 @@ contract NTokenMoonBirds is NToken, IMoonBirdBase {
         This function allows NToken holders to toggle on/off the nesting the status for the underlying tokens
     */
     function toggleNesting(uint256[] calldata tokenIds) external {
-        for (uint256 index = 0; index < tokenIds.length; index++) {
+        for (uint256 index = 0; index < tokenIds.length;) {
             require(
                 _isApprovedOrOwner(_msgSender(), tokenIds[index]),
                 "ERC721: transfer caller is not owner nor approved"
             );
+            unchecked {
+                ++index;
+            }
         }

```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107-L120
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
107:        for (uint256 index = 0; index < tokenIds.length; index++) {

120:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
index c570021..6e7a9ae 100644
--- a/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
@@ -104,7 +104,7 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
         address receiverOfUnderlying,
         uint256[] calldata tokenIds
     ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
-        for (uint256 index = 0; index < tokenIds.length; index++) {
+        for (uint256 index = 0; index < tokenIds.length;) {
             ApeStakingLogic.executeUnstakePositionAndRepay(
                 _ERC721Data.owners,
                 apeStakingDataStorage(),
@@ -117,6 +117,9 @@ abstract contract NTokenApeStaking is NToken, INTokenApeStaking {
                     incentiveReceiver: address(0)
                 })
             );
+            unchecked {
+                ++index;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106-L112
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NToken.sol
106:            for (uint256 index = 0; index < tokenIds.length; index++) {

112:            }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NToken.sol b/paraspace-core/contracts/protocol/tokenization/NToken.sol
index 5a23c73..92940f1 100644
--- a/paraspace-core/contracts/protocol/tokenization/NToken.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NToken.sol
@@ -103,12 +103,15 @@ contract NToken is VersionedInitializable, MintableIncentivizedERC721, INToken {
         ) = _burnMultiple(from, tokenIds);

         if (receiverOfUnderlying != address(this)) {
-            for (uint256 index = 0; index < tokenIds.length; index++) {
+            for (uint256 index = 0; index < tokenIds.length;) {
                 IERC721(_underlyingAsset).safeTransferFrom(
                     address(this),
                     receiverOfUnderlying,
                     tokenIds[index]
                 );
+                unchecked {
+                    ++index;
+                }
             }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145-L147
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NToken.sol
145:        for (uint256 i = 0; i < ids.length; i++) {
146:            IERC721(token).safeTransferFrom(address(this), to, ids[i]);
147:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/NToken.sol b/paraspace-core/contracts/protocol/tokenization/NToken.sol
index 5a23c73..3d97620 100644
--- a/paraspace-core/contracts/protocol/tokenization/NToken.sol
+++ b/paraspace-core/contracts/protocol/tokenization/NToken.sol
@@ -142,8 +142,11 @@ contract NToken is VersionedInitializable, MintableIncentivizedERC721, INToken {
             token != _underlyingAsset,
             Errors.UNDERLYING_ASSET_CAN_NOT_BE_TRANSFERRED
         );
-        for (uint256 i = 0; i < ids.length; i++) {
+        for (uint256 i = 0; i < ids.length;) {
             IERC721(token).safeTransferFrom(address(this), to, ids[i]);
+            unchecked {
+                ++i;
+            }
         }
         emit RescueERC721(token, to, ids);
     }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493-L501
```solidity
File: /paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
493:        for (uint256 index = 0; index < tokenIds.length; index++) {

501:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol b/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
index 61438db..61c5541 100644
--- a/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
+++ b/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
@@ -490,7 +490,7 @@ abstract contract MintableIncentivizedERC721 is
             .userState[sender]
             .collateralizedBalance;

-        for (uint256 index = 0; index < tokenIds.length; index++) {
+        for (uint256 index = 0; index < tokenIds.length;) {
             MintableERC721Logic.executeSetIsUsedAsCollateral(
                 _ERC721Data,
                 POOL,
@@ -498,6 +498,9 @@ abstract contract MintableIncentivizedERC721 is
                 useAsCollateral,
                 sender
             );
+            unchecked {
+                ++index;
+            }
         }

```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634-L640
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolCore.sol
634:        for (uint256 i = 0; i < reservesListCount; i++) {
635:            if (ps._reservesList[i] != address(0)) {
636:                reservesList[i - droppedReservesCount] = ps._reservesList[i];
637:            } else {
638:                droppedReservesCount++;
639:            }
640:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/pool/PoolCore.sol b/paraspace-core/contracts/protocol/pool/PoolCore.sol
index 115a728..ebc6922 100644
--- a/paraspace-core/contracts/protocol/pool/PoolCore.sol
+++ b/paraspace-core/contracts/protocol/pool/PoolCore.sol
@@ -631,12 +631,15 @@ contract PoolCore is
         uint256 droppedReservesCount = 0;
         address[] memory reservesList = new address[](reservesListCount);

-        for (uint256 i = 0; i < reservesListCount; i++) {
+        for (uint256 i = 0; i < reservesListCount;) {
             if (ps._reservesList[i] != address(0)) {
                 reservesList[i - droppedReservesCount] = ps._reservesList[i];
             } else {
                 droppedReservesCount++;
             }
+            unchecked {
+                ++i;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72-L78
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
72:        for (uint256 index = 0; index < _nfts.length; index++) {

78:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol b/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
index d9c033d..81d2794 100644
--- a/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
+++ b/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
@@ -69,12 +69,15 @@ contract PoolApeStaking is
         INToken nToken = INToken(xTokenAddress);

         uint256 amountToWithdraw = 0;
-        for (uint256 index = 0; index < _nfts.length; index++) {
+        for (uint256 index = 0; index < _nfts.length;) {
             require(
                 nToken.ownerOf(_nfts[index].tokenId) == msg.sender,
                 Errors.NOT_THE_OWNER
             );
             amountToWithdraw += _nfts[index].amount;
+            unchecked {
+                ++index;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103-L108
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
103:        for (uint256 index = 0; index < _nfts.length; index++) {

108:        }
```

```diff
diff --git a/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol b/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
index d9c033d..aba8b11 100644
--- a/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
+++ b/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
@@ -100,11 +100,14 @@ contract PoolApeStaking is
         DataTypes.ReserveData storage nftReserve = ps._reserves[nftAsset];
         address xTokenAddress = nftReserve.xTokenAddress;
         INToken nToken = INToken(xTokenAddress);
-        for (uint256 index = 0; index < _nfts.length; index++) {
+        for (uint256 index = 0; index < _nfts.length;) {
             require(
                 nToken.ownerOf(_nfts[index]) == msg.sender,
                 Errors.NOT_THE_OWNER
             );
+            unchecked {
+                ++index;
+            }
         }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172-L178
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
172:        for (uint256 index = 0; index < actualTransferAmount; index++) {

178:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199-L210
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
199:        for (uint256 index = 0; index < _nftPairs.length; index++) {

210:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215-L221
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
215:        for (uint256 index = 0; index < _nftPairs.length; index++) {

221:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279-L285
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
279:        for (uint256 index = 0; index < _nfts.length; index++) {

285:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289-L302
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
289:        for (uint256 index = 0; index < _nftPairs.length; index++) {

302:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305-L311
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
305:        for (uint256 index = 0; index < _nftPairs.length; index++) {

311:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131-L147
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol
131:        for (uint256 index = 0; index < amount; index++) {

147:    }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212-L222
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol
212:            for (uint256 index = 0; index < tokenIds.length; index++) {

222:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465-L475
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol
465:            for (uint256 index = 0; index < tokenIds.length; index++) {

475:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034-L1039
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol
1034:        for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

1039:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177-L224
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
177:        for (uint256 i = 0; i < marketplaceIds.length; i++) {

224:        }
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
284:        for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:        for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:        for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371
```solidity
File:/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol
371:            for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol
38:        for (i = 0; i < params.nftTokenIds.length; i++) {

56:        for (i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193
```solidity
File: /paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
193:        for (uint256 index = 0; index < tokenIds.length; index++) {

210:        for (uint256 index = 0; index < tokenIds.length; index++) {
```


[see resource](https://github.com/ethereum/solidity/issues/10695)



### Splitting require() statements that use && saves gas - (saves 8 gas per &&)

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per &&
The gas difference would only be realized if the revert condition is realized(met).

**Proof**
**The following tests were carried out in remix with both optimization turned on and off**

```solidity
function multiple (uint a) public pure returns (uint){
	require ( a > 1 && a < 5, "Initialized");
	return  a + 2;
}
```
**Execution cost**
21617 with optimization and using &&
21976 without optimization and using &&


After splitting the require statement
```solidity
function multiple(uint a) public pure returns (uint){
	require (a > 1 ,"Initialized");
	require (a < 5 , "Initialized");
	return a + 2;
}
```
**Execution cost**
21609 with optimization and split require
21968 without optimization and using split require


**Instances**
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-L220
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolParameters.sol
216:        require(
217:            value > MIN_AUCTION_HEALTH_FACTOR &&
218:                value <= MAX_AUCTION_HEALTH_FACTOR,
219:            Errors.INVALID_AMOUNT
220:        );
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L553
```solidity
File:/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol
546:        require(
547:            vars.collateralReserveActive && vars.principalReserveActive,
548:            Errors.RESERVE_INACTIVE
549:        );
550:        require(
551:            !vars.collateralReservePaused && !vars.principalReservePaused,
552:            Errors.RESERVE_PAUSED
553:        );

636:        require(
637:            vars.collateralReserveActive && vars.principalReserveActive,
638:            Errors.RESERVE_INACTIVE
639:        );
640:        require(
641:            !vars.collateralReservePaused && !vars.principalReservePaused,
642:            Errors.RESERVE_PAUSED
643:        );

1196:            require(
1197:                vars.token0IsActive && vars.token1IsActive,
1198:                Errors.RESERVE_INACTIVE
1199:            );

1202:            require(
1203:                !vars.token0IsPaused && !vars.token1IsPaused,
1204:                Errors.RESERVE_PAUSED
1205:            );

1208:            require(
1209:                !vars.token0IsFrozen && !vars.token1IsFrozen,
1210:                Errors.RESERVE_FROZEN
1211:            );
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-L175
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
171:        require(
172:            marketplaceIds.length == payloads.length &&
173:                payloads.length == credits.length,
174:            Errors.INCONSISTENT_PARAMS_LENGTH
175:        );


279:        require(
280:            marketplaceIds.length == payloads.length &&
281:                payloads.length == credits.length,
282:            Errors.INCONSISTENT_PARAMS_LENGTH
283:        );
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41-L45
```solidity
File: /paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol
41:        require(
42:            // NOT criteria based and must be basic order
43:            resolvers.length == 0 && isBasicOrder(advancedOrder),
44:            Errors.INVALID_MARKETPLACE_ORDER
45:        );


71:        require(
72:            // NOT criteria based and must be basic order
73:            advancedOrders.length == 2 &&
74:                isBasicOrder(advancedOrders[0]) &&
75:                isBasicOrder(advancedOrders[1]),
76:            Errors.INVALID_MARKETPLACE_ORDER
77:        );
```

### Use shorter revert strings(less than 32 bytes) 
Every reason string takes at least 32 bytes so make sure your string fits in 32 bytes or it will become more expensive.Each extra chunk of byetes past the original 32 incurs an MSTORE which costs 3 gas

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L88-L92
```solidity
File: /paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
88:        require(
89:            erc721Data.owners[tokenId] == from,
90:            "ERC721: transfer from incorrect owner"
91:        );
92:        require(to != address(0), "ERC721: transfer to the zero address");


377:        require(
378:            _exists(erc721Data, tokenId),
379:            "ERC721: startAuction for nonexistent token"
380:        );

395:        require(
396:            _exists(erc721Data, tokenId),
397:            "ERC721: endAuction for nonexistent token"
398:        );
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L98-L101
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
98:            require(
99:                _isApprovedOrOwner(_msgSender(), tokenIds[index]),
100:                "ERC721: transfer caller is not owner nor approved"
101:            );
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L195-L198
```solidity
File: /paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
195:        require(
196:            _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
197:            "ERC721: approve caller is not owner nor approved for all"
198:        );

213:        require(
214:            _exists(tokenId),
215:            "ERC721: approved query for nonexistent token"
216:        );

259:        require(
260:            _isApprovedOrOwner(_msgSender(), tokenId),
261:            "ERC721: transfer caller is not owner nor approved"
262:        );


296:        require(
297:            _isApprovedOrOwner(_msgSender(), tokenId),
298:            "ERC721: transfer caller is not owner nor approved"
299:        );


354:        require(
355:            _exists(tokenId),
356:            "ERC721: operator query for nonexistent token"
357:        );

594:        require(
595:            index < balanceOf(owner),
596:            "ERC721Enumerable: owner index out of bounds"
597:        );


618:        require(
619:            index < totalSupply(),
620:            "ERC721Enumerable: global index out of bounds"
621:        );
```


I suggest shortening the revert strings to fit in 32 bytes, or using custom errors.
