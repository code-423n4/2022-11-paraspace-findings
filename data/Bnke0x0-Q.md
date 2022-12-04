


### [L01] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::97 => function initialize(
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::101 => ) public initializer {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::75 => function initialize(IPoolAddressesProvider provider)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::78 => initializer
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::52 => function initialize(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::59 => ) public virtual override initializer {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::36 => function initialize(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::43 => ) public virtual override initializer {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::49 => super.initialize(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::58 => initializeStakingData();
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::130 => function initializeStakingData() internal {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::57 => function initialize() external initializer {
```



### [L02] `safeApprove()` is deprecated

#### Impact
Deprecated in favor of safeIncreaseAllowance() and safeDecreaseAllowance()
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::555 => IERC20(token).safeApprove(operator, type(uint256).max);
```





### [L03] Unused `receive()` function will lock Ether in contract

#### Impact
If the intention is for the Ether to be used, the function should call another function, otherwise, it should revert
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::149 => receive() external payable {}
```



### [L04] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

#### Impact
approve is subject to a known front-running attack. Consider using safeApprove instead:
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::45 => _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::46 => _apeCoin.approve(address(POOL), type(uint256).max);
```












### Non-Critical Issues




### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::122 => return BASE_CURRENCY_UNIT;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::135 => return price;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::200 => return prices;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::197 => return prices;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::214 => return sum;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::279 => return oracleData;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::52 => return _marketId;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::217 => return marketplace;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::393 => return amountToWithdraw;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::67 => return _maxPriceMultiplier;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::71 => return _minExpPriceMultiplier;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::75 => return _minPriceMultiplier;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::79 => return _stepLinear;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::83 => return _stepExp;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::87 => return _tickLength;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::107 => return _maxPriceMultiplier;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::440 => return healthFactor;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::57 => return POOL_REVISION;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::646 => return reservesList;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::67 => return POOL_REVISION;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::94 => return POOL_REVISION;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::195 => return _underlyingAsset;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::324 => return ATOMIC_PRICING;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::334 => return XTokenType.NToken;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::72 => return _apeCoinStaking;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::222 => return totalAmount;
```



### [N02] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::134 => require(price != 0, Errors.ORACLE_PRICE_NOT_READY);
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::152 => revert(Errors.ORACLE_PRICE_NOT_READY);
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::169 => revert(Errors.ORACLE_PRICE_NOT_READY);
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::186 => revert(Errors.ORACLE_PRICE_NOT_READY);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::70 => revert(Errors.CALL_MARKETPLACE_FAILED);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::102 => revert(Errors.CALL_MARKETPLACE_FAILED);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::51 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::84 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::38 => require(runInput.details.length == 1, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::51 => require(nfts.length == 1, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::85 => revert(Errors.CALL_MARKETPLACE_FAILED);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::110 => revert(Errors.CALL_MARKETPLACE_FAILED);
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::86 => require(id != POOL, Errors.INVALID_ADDRESSES_PROVIDER_ID);
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::219 => revert(Errors.INVALID_MARKETPLACE_ID);
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::268 => require(newAddress != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::245 => require(vars.orderInfo.taker == onBehalfOf, Errors.INVALID_ORDER_TAKER);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::412 => require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::440 => require(vars.xTokenAddress != address(0), Errors.ASSET_NOT_LISTED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::489 => require(isNToken, Errors.ASSET_NOT_LISTED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::45 => require(Address.isContract(params.asset), Errors.NOT_CONTRACT);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::56 => require(!reserveAlreadyAdded, Errors.RESERVE_ALREADY_ADDED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::68 => require(amount != 0, Errors.INVALID_AMOUNT);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::84 => require(reserveAssetType == assetType, Errors.INVALID_ASSET_TYPE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::85 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::86 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::87 => require(!isFrozen, Errors.RESERVE_FROZEN);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::160 => require(amount != 0, Errors.INVALID_AMOUNT);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::185 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::186 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::207 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::208 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::258 => require(params.amount != 0, Errors.INVALID_AMOUNT);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::273 => require(vars.isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::274 => require(!vars.isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::275 => require(!vars.isFrozen, Errors.RESERVE_FROZEN);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::276 => require(vars.borrowingEnabled, Errors.BORROWING_NOT_ENABLED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::339 => require(vars.currentLtv != 0, Errors.LTV_VALIDATION_FAILED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::377 => require(amountSent != 0, Errors.INVALID_AMOUNT);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::390 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::391 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::406 => require((variableDebt != 0), Errors.NO_DEBT_OF_SELECTED_TYPE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::418 => require(userBalance != 0, Errors.UNDERLYING_BALANCE_ZERO);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::438 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::439 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::460 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::461 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::690 => require(params.globalDebt != 0, Errors.GLOBAL_DEBT_IS_ZERO);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::768 => require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::769 => require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::824 => require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::825 => require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::918 => require(assetLTV == 0, Errors.LTV_VALIDATION_FAILED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::937 => require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::950 => require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::975 => require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1029 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1030 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1057 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1058 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1068 => require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1074 => require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::453 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::454 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::157 => require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::172 => require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::187 => require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::214 => require(value != 0, Errors.INVALID_AMOUNT);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::271 => require(user != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::60 => require(initializingPool == POOL, Errors.POOL_ADDRESSES_DO_NOT_MATCH);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::64 => require(underlyingAsset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::176 => require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::70 => require(msg.sender == _underlyingAsset, Errors.OPERATION_NOT_SUPPORTED);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::131 => require(user == ownerOf(tokenId), Errors.NOT_THE_OWNER);
```



### [N03] constants should be defined rather than using magic numbers


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::12 => uint128 constant EXPIRATION_PERIOD = 1800;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::14 => uint128 constant MAX_DEVIATION_RATE = 150;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::366 => ? (_twap * 100) / _priorTwap
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::21 => uint256 internal constant Q128 = 0x100000000000000000000000000000000;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::245 => ((oracleData.token0Price * (10**18)) /
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::247 => ) * 2**96) / 1E9
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::254 => (10 **
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::255 => (18 +
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::259 => ) * 2**96) / 1E9
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::266 => (10 **
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::267 => (18 +
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::271 => ) * 2**96) /
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::272 => 10 **
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::143 => vars.assetUnit = 10**vars.decimals;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::42 => using PRBMathUD60x18 for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::690 => vars.collateralAssetUnit = 10**vars.collateralDecimals;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::691 => vars.liquidationAssetUnit = 10**vars.liquidationAssetDecimals;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::818 => vars.liquidationAssetUnit = 10**vars.liquidationAssetDecimals;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::50 => 0.95e18;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::98 => (10**reserveCache.reserveConfiguration.getDecimals()),
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::294 => vars.assetUnit = 10**vars.reserveDecimals;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1138 => 0x88d989289235fb06c18e3c2f7ea914f41f773e86fb0073d632539f566f4df353, // keccak256("ParaSpace")
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::50 => uint256 internal constant MAX_AUCTION_HEALTH_FACTOR = 2e18;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::51 => uint256 internal constant MIN_AUCTION_HEALTH_FACTOR = 1e18;
```



### [N04] Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::21 => uint256 internal constant Q128 = 0x100000000000000000000000000000000;
```





### [N05] Variable names that consist of all capital letters should be reserved for `const`/`immutable `variables

#### Impact
If the variable needs to be different based on which class it comes from, a view/pure function should be used insteadIssue Information: [NC006](https://github.com/Bnke0x0/c4-common-issues/blob/main/2-Low-Risk.md#n006---Variable-names-that-consist-of-all-capital-letters-should-be-reserved-for-const/immutable-variables)

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::75 => address internal _underlyingAsset;
```



### [N06] Use a more recent version of solidity

#### Impact
Use a solidity version of at least `0.8.13`` to get the ability to use `using for`` with a list of free functions
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::22 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::23 => using GPv2SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::24 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::26 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::27 => using WadRayMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::28 => using PercentageMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::29 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::30 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::37 => using PercentageMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::38 => using ReserveLogic for DataTypes.ReserveCache;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::39 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::40 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::41 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::42 => using PRBMathUD60x18 for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::43 => using GPv2SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::34 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::35 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::36 => using SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::24 => using GPv2SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::25 => using WadRayMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::26 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::27 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::29 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::30 => using GPv2SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::31 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::32 => using WadRayMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::38 => using WadRayMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::39 => using PercentageMath for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::40 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::41 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::21 => using PRBMathUD60x18 for uint256;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::29 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::30 => using UserConfiguration for DataTypes.UserConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::31 => using SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::32 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::51 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::52 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::53 => using SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::46 => using ReserveLogic for DataTypes.ReserveData;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::30 => using SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::27 => using SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::38 => using Address for address;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::19 => using SafeERC20 for IERC20;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::20 => using PercentageMath for uint256;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::25 => using ReserveConfiguration for DataTypes.ReserveConfigurationMap;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::26 => using UserConfiguration for DataTypes.UserConfigurationMap;
```


### [N07] Open TODOs

#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::238 => // TODO using bit shifting for the 2^96
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::59 => makerAsk.price, // TODO: take minPercentageToAsk into account
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::442 => // TODO: support PToken
```



### [N08] States/flags should use Enums rather than separate constants


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::35 => uint256 internal constant POOL_REVISION = 1;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::53 => uint256 public constant POOL_REVISION = 1;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::56 => uint256 internal constant POOL_REVISION = 1;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::49 => uint256 internal constant POOL_REVISION = 1;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::22 => uint256 constant BAYC_POOL_ID = 1;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::23 => uint256 constant MAYC_POOL_ID = 2;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::24 => uint256 constant BAKC_POOL_ID = 3;
```



### [N09] Unused file


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::1 => // SPDX-License-Identifier: MIT
```



### [N10] `2**<n> - 1` should be re-written as `type(uint<n>).max`

#### Impact
Earlier versions of solidity can use uint<n>(-1) instead. Expressions not including the - 1 can often be re-written to accomodate the change (e.g. by using a > rather than a >=, which will also save some gas)
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::247 => ) * 2**96) / 1E9
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::259 => ) * 2**96) / 1E9
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::271 => ) * 2**96) /
```




### [N11] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::261 => function getFeederSize() public view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::115 => ) public pure returns (uint256 token0Amount, uint256 token1Amount) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::146 => ) public view returns (uint256 token0Amount, uint256 token1Amount) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::156 => function getTokenPrice(uint256 tokenId) public view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::65 => function getAddress(bytes32 id) public view override returns (address) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::57 => ) public {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::59 => ) public virtual override initializer {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::43 => ) public virtual override initializer {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::64 => function getBAKC() public view returns (IERC721) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::96 => function name() public view override returns (string memory) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::604 => function totalSupply() public view virtual override returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::235 => ) public view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::87 => ) public {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::428 => ) public view returns (bool) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::237 => ) public virtual override returns (bytes4) {
```



### [N12] Constant redefined elsewhere

#### Impact
Consider defining in only one contract so that values cannot become out of sync when only one location is updated
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::70 => bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
```





