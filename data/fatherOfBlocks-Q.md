**paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol**
- L7/9/11/13/15 - SeaportInterface, SignatureChecker, AdvancedOrder, CriteriaResolver, Fulfillment, IERC1271 and PoolStorage are imported but are never used, so they should be removed.


**paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol**
- L6/8/9/10/11/13/15 - OrderTypes, ILooksRareExchange, SignatureChecker, ConsiderationItem, OfferItem, ItemType, IERC1271 and PoolStorage are imported but they are never used, therefore they should be removed.


**paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol**
- L6/7/8/10/12/14/16 - OrderTypes, SeaportInterface, ILooksRareExchange, SignatureChecker, AdvancedOrder, CriteriaResolver, Fulfillment, IERC1271 and PoolStorage are imported but are never used, so they should be removed.


**paraspace-core/contracts/misc/NFTFloorOracle.sol**
- L370/371/372/373 - Instead of using one if and two returns, you could directly do this on one line: "return !(priceDeviation >= config.maxPriceDeviation);"


**paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol**
- L8 - IReserveAuctionStrategy is imported, but is never used, so it should be removed.


**paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol**
- L5/10 - SafeCast and ReserveConfiguration are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol**
- L10/11 - INTokenApeStaking, XTokenType and IXTokenType are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol**
- L4 - IERC20 is imported, but never used, so it should be removed.


**paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol**
- L7/23/28/29 - WadRayMath, PRBMath, IPoolAddressesProvider and Errors are imported, but they are never used, so they should be removed.

- L306 - There are commented lines of code that do not add any value, they should be removed.


**paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol**
- L4/18 - The same interface is imported twice, one of them should be removed.

- L8/15/21 - ICollateralizableERC721, SeaportInterface, AdvancedOrder, CriteriaResolver and Fulfillment are imported, but they are never used, therefore they should be removed.


**paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol**
- L8/10/12/17 - INonfungiblePositionManager, INTokenApeStaking, IAuctionableERC721 and PercentageMath are imported, but they are never used, therefore they should be removed.


**paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol**
- L4/6/7/8/25/30 - IERC20, Address, GPv2SafeERC20, IReserveInterestRateStrategy, SafeCast, and INTokenApeStaking are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol**
- L4/5/6/7/9/10/11 - IERC20, WadRayMath, PercentageMath, DataTypes, IPoolAddressesProvider, IToken and Errors are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/pool/PoolApeStaking.sol**
- L10/15 - IXTokenType and WadRayMath are imported, but they are never used, so they should be removed.

- L42 - The ReserveUsedAsCollateralDisabled event is created, but it is never used, therefore it should be eliminated.


**paraspace-core/contracts/protocol/pool/PoolCore.sol**
- L5/25 - The same library is imported twice, one of them should be deleted.

- L7/10/19/22/23/24/29 - PoolLogic, MarketplaceLogic, IACLManager, Address, IERC721Receiver, IMarketplace and IWETH are imported, but they are never used, therefore they should be removed.


**paraspace-core/contracts/protocol/pool/PoolParameters.sol**
- L5/24 - The same library is imported twice, one of them should be deleted.

- L9/10/11/12 - SupplyLogic, MarketplaceLogic, BorrowLogic and LiquidationLogic are imported, but they are never used, therefore they should be removed.


**paraspace-core/contracts/protocol/pool/PoolStorage.sol**
- L4/5/6 - UserConfiguration, ReserveConfiguration and ReserveLogic are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol**
- L11/13 - PercentageMath and Errors are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol**
- L4/5/6/7/9/10/11/14/17/18 - IERC20, IERC721, IERC1155, IERC721Metadata, GPv2SafeERC20, SafeCast, IMoonBirdBase, WadRayMath, IRewardController and IncentivizedERC20 are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol**
- L6/7/10/12/15 - IERC1155, IERC721Metadata, SafeCast, WadRayMath, and DataTypes are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol**
- L3/4/5/6 - ApeCoinStaking, IERC721, SafeERC20 and IERC20 are imported, but they are never used, so they should be removed.


**paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol**
- L4/7/11 - IERC721, IPool and MintableERC721Logic are imported, but they are never used, so they should be removed.

- L26 - A struct with a single parameter does not make much sense, since the objective of a struct is to group a certain number of parameters because they have a specific relationship. In this case, it should just be a variable.
