#### IT COSTS MORE GAS TO INITIALIZE NON-CONSTANT/NON-IMMUTABLE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED (13 INSTANCES)
###### If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

paraspace-core/contracts/misc/NFTFloorOracle.sol: [201](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L201), [411](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L411)
```
diff --git a/NFTFloorOracle.sol.orig b/NFTFloorOracle.sol
index b261d57..333ec52 100644
--- a/NFTFloorOracle.sol.orig
+++ b/NFTFloorOracle.sol
@@ -198,7 +198,7 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
         onlyWhenAssetExisted(_asset)
         whenNotPaused(_asset)
     {
-        bool dataValidity = false;
+        bool dataValidity;
         if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) {
             _finalizePrice(_asset, _twap);
             return;
@@ -408,7 +408,7 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
         //use memory here so allocate with maximum length
         uint256 feederSize = feeders.length;
         uint256[] memory validPriceList = new uint256[](feederSize);
-        uint256 validNum = 0;
+        uint256 validNum;
         //aggeregate with price from all feeders
         for (uint256 i = 0; i < feederSize; i++) {
             PriceInformation memory priceInfo = feederRegistrar.feederPrice[
```
paraspace-core/contracts/misc/ParaSpaceOracle.sol: [125](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L125)
```
diff --git a/ParaSpaceOracle.sol.orig b/ParaSpaceOracle.sol
index fbaf52d..fbad25d 100644
--- a/ParaSpaceOracle.sol.orig
+++ b/ParaSpaceOracle.sol
@@ -122,7 +122,7 @@ contract ParaSpaceOracle is IParaSpaceOracle {
             return BASE_CURRENCY_UNIT;
         }

-        uint256 price = 0;
+        uint256 price;
         IEACAggregatorProxy source = IEACAggregatorProxy(assetsSources[asset]);
         if (address(source) != address(0)) {
             price = uint256(source.latestAnswer());
```
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol: [208](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L208)
```
diff --git a/UniswapV3OracleWrapper.sol.orig b/UniswapV3OracleWrapper.sol
index be9ffbe..9714549 100644
--- a/UniswapV3OracleWrapper.sol.orig
+++ b/UniswapV3OracleWrapper.sol
@@ -205,7 +205,7 @@ contract UniswapV3OracleWrapper is IUniswapV3OracleWrapper {
         view
         returns (uint256)
     {
-        uint256 sum = 0;
+        uint256 sum;

         for (uint256 index = 0; index < tokenIds.length; index++) {
             sum += getTokenPrice(tokenIds[index]);
```
paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol: [380](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L380)
```
diff --git a/MarketplaceLogic.sol.orig b/MarketplaceLogic.sol
index 36b0a0b..57f5842 100644
--- a/MarketplaceLogic.sol.orig
+++ b/MarketplaceLogic.sol
@@ -377,7 +377,7 @@ library MarketplaceLogic {
         DataTypes.ExecuteMarketplaceParams memory params,
         MarketplaceLocalVars memory vars
     ) internal returns (uint256, uint256) {
-        uint256 price = 0;
+        uint256 price;

         for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
             ConsiderationItem memory item = params.orderInfo.consideration[i];
```
paraspace-core/contracts/protocol/pool/PoolApeStaking.sol: [71](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L71), [135](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L135), [137](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L137), [353](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L353)
```
diff --git a/PoolApeStaking.sol.orig b/PoolApeStaking.sol
index d9c033d..9cbdb99 100644
--- a/PoolApeStaking.sol.orig
+++ b/PoolApeStaking.sol
@@ -68,7 +68,7 @@ contract PoolApeStaking is
         address xTokenAddress = nftReserve.xTokenAddress;
         INToken nToken = INToken(xTokenAddress);

-        uint256 amountToWithdraw = 0;
+        uint256 amountToWithdraw;
         for (uint256 index = 0; index < _nfts.length; index++) {
             require(
                 nToken.ownerOf(_nfts[index].tokenId) == msg.sender,
@@ -132,9 +132,9 @@ contract PoolApeStaking is

         INTokenApeStaking nTokenApeStaking = INTokenApeStaking(xTokenAddress);
         IERC721 bakcContract = nTokenApeStaking.getBAKC();
-        uint256 amountToWithdraw = 0;
+        uint256 amountToWithdraw;
         uint256[] memory transferredTokenIds = new uint256[](_nftPairs.length);
-        uint256 actualTransferAmount = 0;
+        uint256 actualTransferAmount;
         for (uint256 index = 0; index < _nftPairs.length; index++) {
             require(
                 INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
@@ -350,7 +350,7 @@ contract PoolApeStaking is
         DataTypes.PoolStorage storage ps = poolStorage();
         DataTypes.ReserveData storage nftReserve = ps._reserves[nftAsset];
         address xTokenAddress = nftReserve.xTokenAddress;
-        address incentiveReceiver = address(0);
+        address incentiveReceiver;
         address positionOwner = INToken(xTokenAddress).ownerOf(tokenId);
         if (msg.sender != positionOwner) {
             require(
```
paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol: [205](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L205), [272](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272)
```
diff --git a/MintableERC721Logic.sol.orig b/MintableERC721Logic.sol
index e8345fa..d02d74c 100644
--- a/MintableERC721Logic.sol.orig
+++ b/MintableERC721Logic.sol
@@ -202,7 +202,7 @@ library MintableERC721Logic {
             .userState[to]
             .collateralizedBalance;
         uint256 oldTotalSupply = erc721Data.allTokens.length;
-        uint64 collateralizedTokens = 0;
+        uint64 collateralizedTokens;

         for (uint256 index = 0; index < tokenData.length; index++) {
             uint256 tokenId = tokenData[index].tokenId;
@@ -269,7 +269,7 @@ library MintableERC721Logic {
             uint64 newCollateralizedBalance
         )
     {
-        uint64 burntCollateralizedTokens = 0;
+        uint64 burntCollateralizedTokens;
         uint64 balanceToBurn;
         uint256 oldTotalSupply = erc721Data.allTokens.length;
         uint256 oldBalance = erc721Data.userState[user].balance;
```
paraspace-core/contracts/protocol/pool/PoolCore.sol: [631](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L631)
```
diff --git a/PoolCore.sol.orig b/PoolCore.sol
index 115a728..e55dd80 100644
--- a/PoolCore.sol.orig
+++ b/PoolCore.sol
@@ -628,7 +628,7 @@ contract PoolCore is
         DataTypes.PoolStorage storage ps = poolStorage();

         uint256 reservesListCount = ps._reservesCount;
-        uint256 droppedReservesCount = 0;
+        uint256 droppedReservesCount;
         address[] memory reservesList = new address[](reservesListCount);

         for (uint256 i = 0; i < reservesListCount; i++) {
```
paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol: [78](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L78)
```
diff --git a/BorrowLogic.sol.orig b/BorrowLogic.sol
index 4e46843..6bbaa86 100644
--- a/BorrowLogic.sol.orig
+++ b/BorrowLogic.sol
@@ -75,7 +75,7 @@ library BorrowLogic {
             })
         );

-        bool isFirstBorrowing = false;
+        bool isFirstBorrowing;

         (
             isFirstBorrowing,
```