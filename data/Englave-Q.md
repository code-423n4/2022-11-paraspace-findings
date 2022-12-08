#### 1. Presence of unused imports
Unused imports are allowed in Solidity and do not pose a direct security issue. It is best practice though to avoid them as they can:
indicate bugs or malformed data structures and they are generally a sign of poor code quality
cause code noise and decrease the readability of the code

**Path:** ./LooksRareAdapter.sol : SeaportInterface, SignatureChecker, AdvancedOrder, CriteriaResolver, Fulfillment, IERC1271, PoolStorage;
./SeaportAdapter.sol : OrderTypes, ILooksRareExchange, SignatureChecker, ConsiderationItem, OfferItem, ItemType, IERC1271, PoolStorage;
./X2Y2Adapter.sol : OrderTypes, SeaportInterface, ILooksRareExchange, SignatureChecker, AdvancedOrder, CriteriaResolver, Fulfillment, IERC1271, PoolStorage;
./PoolApeStaking.sol: IXTokenType, WadRayMath;
./DefaultReserveAuctionStrategy.sol : IERC20, WadRayMath, PercentageMath, DataTypes, IPoolAddressesProvider, IToken, Errors;
./PoolMarketplace.sol : Errors, ReserveConfiguration, PoolLogic, SupplyLogic, BorrowLogic, LiquidationLogic, IERC20WithPermit, IWETH, ​​ItemType, INToken, IACLManager, FlashClaimLogic, Address, IERC721Receiver, IMarketplace, IAuctionableERC721, IReserveAuctionStrategy;
./MarketplaceLogic.sol : ICollateralizableERC721, SeaportInterface, AdvancedOrder, CriteriaResolver, Fulfillment;
./NTokenMoonBirds.sol : IERC20, IERC721, IERC1155, IERC721Metadata, Address, GPv2SafeERC20, SafeCast, WadRayMath, IRewardController, IncentivizedERC20; 
./NToken.sol : GPv2SafeERC20, SafeCast, WadRayMath, IInitializableNToken, IncentivizedERC20; 
./MintableERC721Logic.sol : ApeCoinStaking, IERC721, SafeERC20, IERC20;
./MintableIncentivizedERC721.sol : Strings, IERC721Receiver, SafeCast, WadRayMath;
./PoolParameters.sol : SupplyLogic, MarketplaceLogic, BorrowLogic, LiquidationLogic, IERC20WithPermit, INToken, FlashClaimLogic, Address, IERC721Receiver, IMarketplace, IAuctionableERC721, IReserveAuctionStrategy;
./ReserveLogic.sol : IERC20, GPv2SafeERC20;  
./PoolCore.sol : PoolLogic, MarketplaceLogic, IACLManager, Address, IERC721Receiver, IMarketplace, IWETH;
 ./MarketplaceLogic.sol : ICollateralizableERC721, SeaportInterface, AdvancedOrder, CriteriaResolver, Fulfillment
./ValidationLogic.sol : IERC20, Address, GPv2SafeERC20, IReserveInterestRateStrategy, ReserveLogic, SafeCast, Helpers;
./SupplyLogic.sol : INonfungiblePositionManager, INTokenApeStaking, IAuctionableERC721, PercentageMath;
./LiquidationLogic.sol : WadRayMath , PRBMath IPoolAddressesProvider;
./GenericLogic.sol : IERC20;
./BorrowLogic.sol : SafeCast, ReserveConfiguration;
./AuctionLogic.sol : IReserveAuctionStrategy;  
**Recommendation:** Remove unused imports.

#### 2. Presence of unused events
Unused events are allowed in Solidity and do not pose a direct security issue. It is best practice though to avoid them as they can:
indicate bugs or malformed data structures and they are generally a sign of poor code quality
cause code noise and decrease the readability of the code

**Path:** ./PoolApeStaking.sol: ReserveUsedAsCollateralDisabled 
**Recommendation:** Remove or implement emit for unused events.

#### 3. Presence of unused using
Unused usings are allowed in Solidity and do not pose a direct security issue. It is best practice though to avoid them as they can:
indicate bugs or malformed data structures and they are generally a sign of poor code quality
cause code noise and decrease the readability of the code

**Path:** ./PoolMarketplace.sol: ReserveLogic for DataTypes.ReserveData, SafeERC20 for IERC20;
./MintableIncentivizedERC721.sol :  Address for address;
**Recommendation:** Remove or implement emit for unused usings.
