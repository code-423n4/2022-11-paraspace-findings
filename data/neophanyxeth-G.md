1. Unused state variables
REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45
_underlyingAsset
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L75
MAYC_POOL_ID
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L23

2. Unused event
ReserveUsedAsCollateralDisabled
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L42

3. USE CUSTOM ERRORS RATHER THAN REQUIRE() STRINGS TO SAVE GAS
This is the example of using custom error instead of first require in PoolApeStaking.sol.
@@ -44,6 +44,8 @@ contract PoolApeStaking is
         address indexed user
     );
 
+    error NotTheOwner();
+
     /**
      * @dev Constructor.
      * @param provider The address of the PoolAddressesProvider contract
@@ -69,11 +71,8 @@ contract PoolApeStaking is
         INToken nToken = INToken(xTokenAddress);
 
         uint256 amountToWithdraw = 0;
         for (uint256 index = 0; index < _nfts.length; index++) {
-            require(
-                nToken.ownerOf(_nfts[index].tokenId) == msg.sender,
-                Errors.NOT_THE_OWNER
-            );
+            if(nToken.ownerOf(_nfts[index].tokenId) != msg.sender) revert NotTheOwner();
             amountToWithdraw += _nfts[index].amount;
         }

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L73
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L85
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L104
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L112
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L139
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L180
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L200
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L280
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L290
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L338
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L356
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L380
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L453
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L454

4. UNCHECKING ARITHMETICS OPERATIONS THAT CAN’T UNDERFLOW/OVERFLOW
This is the example of using unchecked for the first i++ in FlashClaimLogic.sol.
@@ -53,13 +53,15 @@ library FlashClaimLogic {
         );
 
         // step 3: moving underlying asset backward from receiver contract
-        for (i = 0; i < params.nftTokenIds.length; i++) {
+        for (i = 0; i < params.nftTokenIds.length;) {
             IERC721(params.nftAsset).safeTransferFrom(
                 params.receiverAddress,
                 nTokenAddress,
                 params.nftTokenIds[i]
             );
-
+            unchecked {
+                i++;
+            }
             emit FlashClaim(

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L109
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L113
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L136
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L174

4. X = X + Y IS MORE EFFICIENT, THAN X += Y
This is the example of using X = X + Y instead of X +=Y in MarketplaceLogic.sol.
@@ -393,8 +393,8 @@ library MarketplaceLogic {
             require(
                 item.token == params.credit.token,
                 Errors.CREDIT_DOES_NOT_MATCH_ORDER
-            );
-            price += item.startAmount;
+            );            
+            price = price + item.startAmount;
         }
 
         uint256 downpayment = price - vars.creditAmount;
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L397
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L215
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L257
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L169
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L176
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L178
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L185
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L189
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L231
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L233
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L235
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L237
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L238
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L380
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L479
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L496
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L497
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L77
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L166

5. 4. X = X - Y IS MORE EFFICIENT, THAN X -= Y
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L146
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L318
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L83
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L205

6. IT COSTS MORE GAS TO INITIALIZE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272

7. ++i costs less gass compared to i++ for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration)
Proof of Concept
i++ increments i and returns the initial value of i. Which means:
uint i = 1;
i++; // == 1 but i == 2
But ++i returns the actual incremented value:
uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable
In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2
protocol/tokenization/NToken.sol:145:        for (uint256 i = 0; i < ids.length; i++) {
protocol/libraries/logic/FlashClaimLogic.sol:38:        for (i = 0; i < params.nftTokenIds.length; i++) {
protocol/libraries/logic/FlashClaimLogic.sol:56:        for (i = 0; i < params.nftTokenIds.length; i++) {
protocol/libraries/logic/ValidationLogic.sol:1034:        for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
protocol/libraries/logic/MarketplaceLogic.sol:177:        for (uint256 i = 0; i < marketplaceIds.length; i++) {
protocol/libraries/logic/MarketplaceLogic.sol:284:        for (uint256 i = 0; i < marketplaceIds.length; i++) {
protocol/libraries/logic/MarketplaceLogic.sol:382:        for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
protocol/libraries/logic/MarketplaceLogic.sol:470:        for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
protocol/libraries/logic/PoolLogic.sol:58:        for (uint16 i = 0; i < params.reservesCount; i++) {
protocol/libraries/logic/PoolLogic.sol:104:        for (uint256 i = 0; i < assets.length; i++) {
protocol/pool/PoolCore.sol:634:        for (uint256 i = 0; i < reservesListCount; i++) {
ui/WPunkGateway.sol:82:        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:109:        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:113:        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:136:        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:174:        for (uint256 i = 0; i < punkIndexes.length; i++) {