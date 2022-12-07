

## [L-01] Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

```
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Pair.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::2 => pragma solidity ^0.8.10;
```

## [L-02] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::380 => assetPriceMapEntry.updatedTimestamp = block.timestamp;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol::87 => answer == 0 && block.timestamp - lastUpdateTimestamp > _gracePeriod;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::802 => block.timestamp
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::51 => if (timestamp == block.timestamp) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::80 => if (timestamp == block.timestamp) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::304 => reserve.lastUpdateTimestamp = uint40(block.timestamp);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol::30 => (block.timestamp - uint256(lastUpdateTimestamp));
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol::97 => * @dev Calculates the compounded interest between the timestamp of the last update and the current block timestamp
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol::100 => * @return The interest rate compounded between lastUpdateTimestamp and current block timestamp, in ray
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol::110 => block.timestamp
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::778 => .calculateAuctionPriceMultiplier(startTime, block.timestamp);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::285 => userConfig.auctionValidityTime = block.timestamp;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::69 => deadline: block.timestamp
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::225 => require(block.timestamp <= deadline, Errors.INVALID_EXPIRATION);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::59 => require(block.timestamp <= deadline, Errors.INVALID_EXPIRATION);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::382 => startTime: block.timestamp
```

## [L-03] Unused receive()/fallback() function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::73 => receive() external payable {}
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::149 => receive() external payable {}
```

## [L-04] safeApprove() is deprecated

safeApprove() is deprecated in favor of safeIncreaseAllowance() and safeDecreaseAllowance(). If only setting the initial allowance to the value that means infinite, safeIncreaseAllowance() can be used instead

```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::555 => IERC20(token).safeApprove(operator, type(uint256).max);
```

## [L-05] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::238 => // TODO using bit shifting for the 2^96
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::59 => makerAsk.price, // TODO: take minPercentageToAsk into account
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::442 => // TODO: support PToken
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::68 => // TODO: check that this is deployed correctly on contract and remove casting
```

## [L-06] decimals() not part of ERC20 standard

decimals() is not part of the official ERC20 standard and might fail for tokens that do not implement it. While in practice it is very unlikely, as usually most of the tokens implement it, this should still be considered as a potential issue.

```
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::53 => uint8 decimals = ERC20(asset).decimals();
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::74 => uint8 ethDecimals = ERC20(WETH).decimals();
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::75 => //uint8 usdcDecimals = ERC20(USDC).decimals();
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::234 => .decimals();
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::236 => .decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::104 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::115 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::165 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::176 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::258 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::272 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::325 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::339 => ).decimals();
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::224 => .decimals();
```

## [L-07] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1076 => keccak256(abi.encodePacked(params.orderInfo.id)) ==
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1077 => keccak256(abi.encodePacked(params.credit.orderId)),
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1128 => keccak256(abi.encodePacked(credit.orderId))
```

## [L-08] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

approve is subject to a known front-running attack. Consider using safeApprove() or safeIncreaseAllowance() or safeDecreaseAllowance() instead

```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::45 => _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::46 => _apeCoin.approve(address(POOL), type(uint256).max);
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::42 => WETH.approve(pool, type(uint256).max);
```

## [L-09] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::81 => pWETH.transferFrom(msg.sender, address(this), amountToWithdraw);
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::172 => pWETH.transferFrom(msg.sender, address(this), amountToWithdraw);
```

## [L-25] Events not emitted for important state changes

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

```
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::44 => function setOracle(address _oracleAddress)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::175 => function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::195 => function setPrice(address _asset, uint256 _twap)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::221 => function setMultiplePrices(
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::66 => function setAssetSources(
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::40 => function setRoleAdmin(bytes32 role, bytes32 adminRole)
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::56 => function setMarketId(string memory newMarketId)
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::65 => function setLtv(DataTypes.ReserveConfigurationMap memory self, uint256 ltv)
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::92 => function setLiquidationThreshold(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::124 => function setLiquidationBonus(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::155 => function setDecimals(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::185 => function setAssetType(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::220 => function setActive(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::247 => function setFrozen(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::274 => function setPaused(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::302 => function setSiloedBorrowing(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::330 => function setBorrowingEnabled(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::357 => function setReserveFactor(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::391 => function setBorrowCap(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::420 => function setSupplyCap(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::449 => function setLiquidationProtocolFee(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::27 => function setBorrowing(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::52 => function setUsingAsCollateral(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::345 => function setPoolPause(bool paused) external override onlyEmergencyAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::356 => function setAuctionRecoveryHealthFactor(uint64 value)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::151 => function setReserveInterestRateStrategyAddress(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::166 => function setReserveAuctionStrategyAddress(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::181 => function setConfiguration(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::206 => function setAuctionRecoveryHealthFactor(uint64 value)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::263 => function setAuctionValidityTime(address user)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::136 => function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::31 => function setNToken(address _nBAYC, address _nMAYC) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::138 => function setIncentivesController(IRewardController controller)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::131 => function setIncentivesController(IRewardController controller)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::142 => function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::224 => function setApprovalForAll(address operator, bool approved)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::458 => function setIsUsedAsCollateral(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::63 => function setClaimer(address user, address claimer) external;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::70 => function setTransferStrategy(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::84 => function setRewardOracle(address reward, IEACAggregatorProxy rewardOracle)
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::64 => function setDistributionEnd(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::76 => function setEmissionPerSecond(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::194 => function setEmissionManager(address emissionManager) external;
```

## [L-25] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::35 => function _mint(address account, uint128 amount) internal virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::106 => _mint(onBehalfOf, amountScaled.toUint128());
```

## [L-25] Upgradeable contract is missing a __gap[50] storage variable to allow for new storage variables in later versions

__gap is empty reserved space in storage that is recommended to be put in place in upgradeable contracts. It allows new state variables to be added in the future without compromising the storage compatibility with existing deployments

```
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::17 => contract WETHGateway is ReentrancyGuard, IWETHGateway, OwnableUpgradeable {
```

## [N-1] Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

```
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/UserFlashclaimRegistry.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IFlashClaimReceiver.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/INFTFloorOracle.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/INFTOracle.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IPunks.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Factory.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Pair.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Router01.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUserFlashclaimRegistry.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IWETH.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IWrappedPunks.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Helpers.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/PercentageMath.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/ConfiguratorInputTypes.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/ATokenDebtToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/DelegationAwarePToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenAToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/RebasingDebtToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/RebasingPToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IEACAggregatorProxy.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IERC20DetailedBytes.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/ITransferStrategyBase.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWETHGateway.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWPunkGateway.sol::2 => pragma solidity 0.8.10;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::2 => pragma solidity 0.8.10;
```

## [N-2] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::21 => uint256 internal constant Q128 = 0x100000000000000000000000000000000;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol::64 => * Example: BAYC + BAKC + 1 ApeCoin:  [[0, 0, "1000000000000000000"]]\
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol::64 => * Example: MAYC + BAKC + 1 ApeCoin:  [[0, 0, "1000000000000000000"]]\
```

## [N-3] type(uint<n>).max should be used instead of uint<n>(-1)

type(uint<n>).max should be used instead for better code readability

```
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::88 => * @dev repays a borrow on the WETH reserve, for the specified amount (or for the whole amount, if uint256(-1) is specified).
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::89 => * @param amount the amount to repay, or uint256(-1) if the user wants to repay everything
```

## [N-4] Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

Scientific notation should be used for better code readability

```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::245 => ((oracleData.token0Price * (10**18)) /
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::24 => // Returns amount of stETH corresponding to 10**27 stETH shares.
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::25 => // The 10**27 is picked to provide the same precision as the ParaSpace
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::26 => // liquidity index, which is in RAY (10**27).
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::23 => // Returns amount of stETH corresponding to 10**27 stETH shares.
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::24 => // The 10**27 is picked to provide the same precision as the ParaSpace
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::25 => // liquidity index, which is in RAY (10**27).
```

## [N-5] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::63 => event OracleConfigSet(uint128 expirationPeriod, uint128 maxPriceDeviation);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::29 => event UnstakeApeIncentiveUpdated(uint256 oldValue, uint256 newValue);
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWETHGateway.sol::7 => event EmergencyTokenTransfer(address token, address to, uint256 amount);
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWPunkGateway.sol::13 => event EmergencyPunkTransfer(address to, uint256 punkIndex);
```

## [N-6] Missing NatSpec

Code should include NatSpec

```
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::1 => // SPDX-License-Identifier: BUSL-1.1
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/INFTFloorOracle.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Factory.sol::1 => // SPDX-License-Identifier: AGPL-3.0
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Pair.sol::1 => // SPDX-License-Identifier: AGPL-3.0
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Router01.sol::1 => // SPDX-License-Identifier: AGPL-3.0
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUserFlashclaimRegistry.sol::1 => // SPDX-License-Identifier: AGPL-3.0
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IWETH.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::1 => // SPDX-License-Identifier: MIT
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::1 => // SPDX-License-Identifier: MIT
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/ConfiguratorInputTypes.sol::1 => // SPDX-License-Identifier: BUSL-1.1
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::1 => // SPDX-License-Identifier: AGPL-3.0
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IEACAggregatorProxy.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IERC20DetailedBytes.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWETHGateway.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWPunkGateway.sol::1 => // SPDX-License-Identifier: agpl-3.0
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::1 => // SPDX-License-Identifier: agpl-3.0
```

## [N-7] Missing event for critical parameter change

Emitting events after sensitive changes take place, to facilitate tracking and notify off-chain clients following changes to the contract.

```
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::44 => function setOracle(address _oracleAddress)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::175 => function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::195 => function setPrice(address _asset, uint256 _twap)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::221 => function setMultiplePrices(
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::66 => function setAssetSources(
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::40 => function setRoleAdmin(bytes32 role, bytes32 adminRole)
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::56 => function setMarketId(string memory newMarketId)
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::65 => function setLtv(DataTypes.ReserveConfigurationMap memory self, uint256 ltv)
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::92 => function setLiquidationThreshold(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::124 => function setLiquidationBonus(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::155 => function setDecimals(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::185 => function setAssetType(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::220 => function setActive(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::247 => function setFrozen(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::274 => function setPaused(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::302 => function setSiloedBorrowing(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::330 => function setBorrowingEnabled(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::357 => function setReserveFactor(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::391 => function setBorrowCap(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::420 => function setSupplyCap(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::449 => function setLiquidationProtocolFee(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::27 => function setBorrowing(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::52 => function setUsingAsCollateral(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::345 => function setPoolPause(bool paused) external override onlyEmergencyAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::356 => function setAuctionRecoveryHealthFactor(uint64 value)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::151 => function setReserveInterestRateStrategyAddress(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::166 => function setReserveAuctionStrategyAddress(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::181 => function setConfiguration(
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::206 => function setAuctionRecoveryHealthFactor(uint64 value)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::263 => function setAuctionValidityTime(address user)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::136 => function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::31 => function setNToken(address _nBAYC, address _nMAYC) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::138 => function setIncentivesController(IRewardController controller)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::131 => function setIncentivesController(IRewardController controller)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::142 => function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::224 => function setApprovalForAll(address operator, bool approved)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::458 => function setIsUsedAsCollateral(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::63 => function setClaimer(address user, address claimer) external;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::70 => function setTransferStrategy(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::84 => function setRewardOracle(address reward, IEACAggregatorProxy rewardOracle)
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::64 => function setDistributionEnd(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::76 => function setEmissionPerSecond(
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsDistributor.sol::194 => function setEmissionManager(address emissionManager) external;
```

## [N-8] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

instances:

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::70 => bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::15 => bytes32 public constant override POOL_ADMIN_ROLE = keccak256("POOL_ADMIN");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::18 => bytes32 public constant override RISK_ADMIN_ROLE = keccak256("RISK_ADMIN");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::21 => bytes32 public constant override BRIDGE_ROLE = keccak256("BRIDGE");
```

## [N-9] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::82 => string public constant POOL_ADDRESSES_DO_NOT_MATCH = "87"; // 'The token implementation pool address and the pool address provided by the initializing pool do not match'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::603 => * @return The actual debt that is getting liquidated. If liquidation amount passed in by the liquidator is greater then the total user debt, then use the user total debt as the actual debt getting liquidated. If the user total debt is greater than the liquidation amount getting passed in by the liquidator, then use the liquidation amount the user is passing in.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1137 => 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
```

