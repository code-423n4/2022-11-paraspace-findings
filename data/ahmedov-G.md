# [G-01] UNNECESSARY STORAGE READ ON EVENT EMITTING

paraspace-core/contracts/misc/NFTFloorOracle.sol: [381](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381)

reading from storage like ```assetPriceMapEntry.twap``` and ```assetPriceMapEntry.updatedAt``` consumes more gas.

## Suggest (saves ~213 gas)

```diff
@@ -380,8 +380,8 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
         assetPriceMapEntry.updatedTimestamp = block.timestamp;
         emit AssetDataSet(
             _asset,
-            assetPriceMapEntry.twap,
-            assetPriceMapEntry.updatedAt
+            _twap,
+            block.number
         );
     }
```

# [G-02] OPTIMIZE ```_quickSort``` FUNCTION WITH ```unchecked``` blocks

paraspace-core/contracts/misc/NFTFloorOracle.sol: [432](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L432)

## Suggest (saves ~4295 gas) Tested on array with length `10`

```diff
diff --git a/paraspace-core/contracts/misc/NFTFloorOracle.sol b/paraspace-core/contracts/misc/NFTFloorOracle.sol
index b261d57..46958f5 100644
--- a/paraspace-core/contracts/misc/NFTFloorOracle.sol
+++ b/paraspace-core/contracts/misc/NFTFloorOracle.sol
@@ -434,20 +434,26 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
         int256 left,
         int256 right
     ) internal pure {
+        if (left == right) return; // check before assigning to save extra gas
         int256 i = left;
         int256 j = right;
-        if (i == j) return;
         uint256 pivot = arr[uint256(left + (right - left) / 2)];
         while (i <= j) {
-            while (arr[uint256(i)] < pivot) i++;
-            while (pivot < arr[uint256(j)]) j--;
+            while (arr[uint256(i)] < pivot) {
+               unchecked{ ++i; }
+            }
+            while (pivot < arr[uint256(j)]) {
+                unchecked { --j; }
+            }
             if (i <= j) {
                 (arr[uint256(i)], arr[uint256(j)]) = (
                     arr[uint256(j)],
                     arr[uint256(i)]
                 );
-                i++;
-                j--;
+                unchecked {
+                    ++i;
+                    --j;
+                }
             }
         }
         if (left < j) _quickSort(arr, left, j);
```

# [G-03] UNCHECKING ARITHMETICS OPERATIONS THAT CAN’T UNDERFLOW/OVERFLOW (44 INSTANCES)

paraspace-core/contracts/misc/NFTFloorOracle.sol: [229](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229)

```diff
-        for (uint256 i = 0; i < _assets.length; i++) {
+        for (uint256 i = 0; i < _assets.length;) {
             setPrice(_assets[i], _twaps[i]);
+
+            unchecked { ++i; }
         }
```

paraspace-core/contracts/misc/NFTFloorOracle.sol: [293](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291)

```diff
     function _addAssets(address[] memory _assets) internal {
-        for (uint256 i = 0; i < _assets.length; i++) {
+        for (uint256 i = 0; i < _assets.length;) {
             _addAsset(_assets[i]);
+
+            unchecked { ++i; }
         }
     }
```

paraspace-core/contracts/misc/NFTFloorOracle.sol: [320](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L320)

```diff
     function _addFeeders(address[] memory _feeders) internal {
-        for (uint256 i = 0; i < _feeders.length; i++) {
+        for (uint256 i = 0; i < _feeders.length;) {
             _addFeeder(_feeders[i]);
+
+            unchecked { ++i; }
         }
     }
```

paraspace-core/contracts/misc/NFTFloorOracle.sol: [413](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413)

```diff
-        for (uint256 i = 0; i < feederSize; i++) {
+        for (uint256 i = 0; i < feederSize;) {
             PriceInformation memory priceInfo = feederRegistrar.feederPrice[
                 feeders[i]
             ];
@@ -421,10 +428,13 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
                     validNum++;
                 }
             }
+
+            unchecked { ++i; }
         }
```

paraspace-core/contracts/misc/ParaSpaceOracle.so: [95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95)
```diff
-        for (uint256 i = 0; i < assets.length; i++) {
+        for (uint256 i = 0; i < assets.length;) {
             require(
                 assets[i] != BASE_CURRENCY,
                 Errors.SET_ORACLE_SOURCE_NOT_ALLOWED
             );
             assetsSources[assets[i]] = sources[i];
             emit AssetSourceUpdated(assets[i], sources[i]);
+
+            unchecked { ++i; }
         }
     }
```

paraspace-core/contracts/misc/ParaSpaceOracle.sol: [197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197)

```diff
-        for (uint256 i = 0; i < assets.length; i++) {
+        for (uint256 i = 0; i < assets.length;) {
             prices[i] = getAssetPrice(assets[i]);
+
+            unchecked { ++i; }
         }
```

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol: [193](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193)

```diff
-        for (uint256 index = 0; index < tokenIds.length; index++) {
+        for (uint256 index = 0; index < tokenIds.length;) {
             prices[index] = getTokenPrice(tokenIds[index]);
+
+            unchecked { ++index; }
         }
```

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol: [210](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210)

```diff
-        for (uint256 index = 0; index < tokenIds.length; index++) {
+        for (uint256 index = 0; index < tokenIds.length;) {
             sum += getTokenPrice(tokenIds[index]);
+
+            unchecked { ++index; }
         }
```

paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.so: [38](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38)

```diff
-        for (i = 0; i < params.nftTokenIds.length; i++) {
+        for (i = 0; i < params.nftTokenIds.length;) {
             INToken(nTokenAddress).transferUnderlyingTo(
                 params.receiverAddress,
                 params.nftTokenIds[i]
             );
+
+            unchecked { ++i; }
         }
```

paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol: [56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56)

```diff
-        for (i = 0; i < params.nftTokenIds.length; i++) {
+        for (i = 0; i < params.nftTokenIds.length;) {
             IERC721(params.nftAsset).safeTransferFrom(
                 params.receiverAddress,
                 nTokenAddress,
@@ -66,6 +68,8 @@ library FlashClaimLogic {
                 params.nftAsset,
                 params.nftTokenIds[i]
             );
+
+            unchecked { ++i; }
```

paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol: [371](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371), [466](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466)
paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol: [177](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177), [284](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284), [382](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382), [470](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470)
paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol: [58](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58), [104](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104)
paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol: [184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184), [195](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195)
paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol: [131](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131), [212](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212), [465](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465), [910](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910), [1034](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034)
paraspace-core/contracts/protocol/pool/PoolApeStaking.sol: [72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72), [103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103), [138](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138), [172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172), [199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199), [215](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215), [279](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279), [289](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289), [305](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305)
paraspace-core/contracts/protocol/pool/PoolCore.sol: [634](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634)
paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol: [493](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493)
paraspace-core/contracts/protocol/tokenization/NToken.sol: [106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106), [145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145)
paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol: [107](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107)
paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol: [51](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51), [97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97)
paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol: [207](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207), [280](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280)
paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol: [213](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213)

# [G-04] ```X = X + Y``` IS MORE EFFICIENT, THAN ```X += Y``` (20 INSTANCES)

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol: [149](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L149), [150](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L150), [211](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L211)
paraspace-core/contracts/protocol/pool/PoolApeStaking.sol: [77](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L77), [166](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L166)
paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol: [169](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L169), [176](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L176), [178](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L178), [185](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L185), [189](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L189), [231](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L231), [233](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L233), [235](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L235), [237](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L237), [238](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L237), [380](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L380), [479](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L479), [496](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L496), [497](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L497)
paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol: [397](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L397)

# [G-05] USE FUNCTION INSTEAD OF MODIFIERS (2 INSTANCES)

paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol: [45](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L45), [59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L59)

```diff
--- a/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
+++ b/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol
@@ -42,7 +42,7 @@ abstract contract MintableIncentivizedERC721 is
     /**
      * @dev Only pool admin can call functions marked by this modifier.
      **/
-    modifier onlyPoolAdmin() {
+    function onlyPoolAdmin() private {
         IACLManager aclManager = IACLManager(
             _addressesProvider.getACLManager()
         );
@@ -50,15 +50,13 @@ abstract contract MintableIncentivizedERC721 is
             aclManager.isPoolAdmin(msg.sender),
             Errors.CALLER_NOT_POOL_ADMIN
         );
-        _;
     }
 
     /**
      * @dev Only pool can call functions marked by this modifier.
      **/
-    modifier onlyPool() {
+    function onlyPool() private {
         require(_msgSender() == address(POOL), Errors.CALLER_MUST_BE_POOL);
-        _;
     }
 
     /**
@@ -128,10 +126,8 @@ abstract contract MintableIncentivizedERC721 is
      * @notice Sets a new Incentives Controller
      * @param controller the new Incentives controller
      **/
-    function setIncentivesController(IRewardController controller)
-        external
-        onlyPoolAdmin
-    {
+    function setIncentivesController(IRewardController controller) external {
+        onlyPoolAdmin();
         _ERC721Data.rewardController = controller;
     }
 
@@ -139,7 +135,8 @@ abstract contract MintableIncentivizedERC721 is
      * @notice Sets new Balance Limit
      * @param limit the new Balance Limit
      **/
-    function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
+    function setBalanceLimit(uint64 limit) external {
+        onlyPoolAdmin();
         _ERC721Data.balanceLimit = limit;
     }
 
@@ -459,7 +456,8 @@ abstract contract MintableIncentivizedERC721 is
         uint256 tokenId,
         bool useAsCollateral,
         address sender
-    ) external virtual override onlyPool nonReentrant returns (bool) {
+    ) external virtual override nonReentrant returns (bool) {
+        onlyPool();
         return
             MintableERC721Logic.executeSetIsUsedAsCollateral(
                 _ERC721Data,
@@ -479,13 +477,13 @@ abstract contract MintableIncentivizedERC721 is
         external
         virtual
         override
-        onlyPool
         nonReentrant
         returns (
             uint256 oldCollateralizedBalance,
             uint256 newCollateralizedBalance
         )
     {
+        onlyPool();
         oldCollateralizedBalance = _ERC721Data
             .userState[sender]
             .collateralizedBalance;
@@ -530,9 +528,9 @@ abstract contract MintableIncentivizedERC721 is
         external
         virtual
         override
-        onlyPool
         nonReentrant
     {
+        onlyPool();
         MintableERC721Logic.executeStartAuction(_ERC721Data, POOL, tokenId);
     }
 
@@ -541,9 +539,9 @@ abstract contract MintableIncentivizedERC721 is
         external
         virtual
         override
-        onlyPool
         nonReentrant
     {
+        onlyPool();
         MintableERC721Logic.executeEndAuction(_ERC721Data, POOL, tokenId);
     }
```

