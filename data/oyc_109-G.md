## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::201 => bool dataValidity = false;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::411 => uint256 validNum = 0;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::413 => for (uint256 i = 0; i < feederSize; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::125 => uint256 price = 0;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::50 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::91 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::193 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::208 => uint256 sum = 0;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::210 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::121 => uint256 typeIndex = 0;
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::146 => for (uint256 i = 0; i < vars.airdropBalance; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::78 => bool isFirstBorrowing = false;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::371 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::466 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::177 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::284 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::380 => uint256 price = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::382 => for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::470 => for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::58 => for (uint16 i = 0; i < params.reservesCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::104 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::184 => for (uint256 index = 0; index < params.tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::195 => for (uint256 index = 0; index < params.tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::131 => for (uint256 index = 0; index < amount; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::212 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::465 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::910 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1034 => for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::71 => uint256 amountToWithdraw = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::72 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::103 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::135 => uint256 amountToWithdraw = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::137 => uint256 actualTransferAmount = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::138 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::172 => for (uint256 index = 0; index < actualTransferAmount; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::199 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::215 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::279 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::289 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::305 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::86 => for (uint256 i = 0; i < input.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::348 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::631 => uint256 droppedReservesCount = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::634 => for (uint256 i = 0; i < reservesListCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::106 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::107 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::51 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::97 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::493 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::213 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::205 => uint64 collateralizedTokens = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::207 => for (uint256 index = 0; index < tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::272 => uint64 burntCollateralizedTokens = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::280 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::58 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::85 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::146 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::214 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::237 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::304 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::89 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::262 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::267 => for (uint256 j = 0; j < size; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::284 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::289 => for (uint256 j = 0; j < size; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::370 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::418 => uint8 i = 0;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::462 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::480 => for (uint256 j = 0; j < tokenLength; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::73 => for (uint256 i = 0; i < users.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::74 => for (uint256 j = 0; j < tokens.length; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::97 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::104 => for (uint256 j = 0; j < reserves.length; j++) {
```

## [G-02] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::50 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::91 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::193 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::210 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::38 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::56 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::177 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::284 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::382 => for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::470 => for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::104 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::184 => for (uint256 index = 0; index < params.tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::195 => for (uint256 index = 0; index < params.tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::212 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::465 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::910 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1034 => for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::72 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::103 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::138 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::199 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::215 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::279 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::289 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::305 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::86 => for (uint256 i = 0; i < input.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::348 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::106 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::107 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::51 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::97 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::493 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::207 => for (uint256 index = 0; index < tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::280 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::58 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::85 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::146 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::214 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::237 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::304 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::89 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::262 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::284 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::370 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::462 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::73 => for (uint256 i = 0; i < users.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::74 => for (uint256 j = 0; j < tokens.length; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::97 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::104 => for (uint256 j = 0; j < reserves.length; j++) {
```

## [G-03] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::66 => require(nftTokenIds.length > 0, "empty token list");
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::51 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::84 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::365 => require(contractSize > 0, _errorMessage);
```

## [G-04] Long Revert Strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::92 => require(to != address(0), "ERC721: transfer to the zero address");
```

## [G-05] Use Shift Right/Left instead of Division/Multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::440 => uint256 pivot = arr[uint256(left + (right - left) / 2)];
```

## [G-06] Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::13 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::12 => address public immutable BEND_DAO;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::13 => address public immutable UNISWAP_FACTORY;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::14 => address public immutable UNISWAP_ROUTER;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::15 => address public immutable WETH;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::16 => address public immutable USDC;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::22 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::28 => address public immutable override BASE_CURRENCY;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::29 => uint256 public immutable override BASE_CURRENCY_UNIT;
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::33 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::20 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::25 => address public immutable pool;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::25 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol::47 => IPoolAddressesProvider public immutable override ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::30 => uint256 public immutable OPTIMAL_USAGE_RATIO;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::37 => uint256 public immutable MAX_EXCESS_USAGE_RATIO;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::39 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::54 => IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::68 => IPool public immutable POOL;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::72 => IPool public immutable POOL;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::73 => bool public immutable ATOMIC_PRICING;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::34 => public immutable networkBaseTokenPriceInUsdProxyAggregator;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::36 => public immutable marketReferenceCurrencyPriceInUsdProxyAggregator;
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::24 => address public immutable weth;
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::25 => address public immutable pool;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::33 => address public immutable punk;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::34 => address public immutable wpunk;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::35 => address public immutable pool;
```

## [G-07] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::158 => function setACLManager(address newAclManager) external override onlyOwner {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::170 => function setACLAdmin(address newAclAdmin) external override onlyOwner {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::235 => function setWETH(address newWETH) external override onlyOwner {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::92 => function dropReserve(address asset) external override onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::345 => function setPoolPause(bool paused) external override onlyEmergencyAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/DelegationAwarePToken.sol::34 => function delegateUnderlyingTo(address delegatee) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::136 => function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::31 => function setNToken(address _nBAYC, address _nMAYC) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::142 => function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
```

## [G-08] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::9 => uint8 constant MIN_ORACLES_NUM = 3;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::12 => uint128 constant EXPIRATION_PERIOD = 1800;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::14 => uint128 constant MAX_DEVIATION_RATE = 150;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::18 => uint128 expirationPeriod;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::20 => uint128 maxPriceDeviation;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::36 => uint8 index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::47 => uint8 index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::300 => uint8 assetIndex = assetFeederMap[_asset].index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::330 => uint8 feederIndex = feederPositionMap[_feeder].index;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::53 => uint8 decimals = ERC20(asset).decimals();
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::74 => uint8 ethDecimals = ERC20(WETH).decimals();
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::43 => uint8 token0Decimal;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::44 => uint8 token1Decimal;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::45 => uint160 sqrtPriceX96;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol::58 => uint16 public constant MAX_RESERVES_COUNT = 128;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::125 => uint16 liquidationAssetReserveId;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/ConfiguratorInputTypes.sol::10 => uint8 underlyingAssetDecimals;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::19 => uint128 liquidityIndex;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::21 => uint128 currentLiquidityRate;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::23 => uint128 variableBorrowIndex;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::25 => uint128 currentVariableBorrowRate;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::29 => uint16 id;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::39 => uint128 accruedToTreasury;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::133 => uint16 referralCode;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::141 => uint16 referralCode;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::149 => uint16 referralCode;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::184 => uint128 liquidityDecrease;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::284 => uint16 reservesCount;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::285 => uint16 maxNumberReserves;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::300 => uint8 v;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::313 => uint16 referralCode;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::365 => uint16 _reservesCount;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::367 => uint64 _auctionRecoveryHealthFactor;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::53 => uint128 balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::54 => uint128 additionalData;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::65 => uint8 private _decimals;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::152 => uint128 castAmount = amount.toUint128();
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::185 => uint128 castAmount = amount.toUint128();
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::244 => uint128 oldSenderBalance = _userState[sender].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::246 => uint128 oldRecipientBalance = _userState[recipient].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::39 => uint128 oldAccountBalance = _userState[account].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::61 => uint128 oldAccountBalance = _userState[account].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::13 => uint64 balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::14 => uint64 collateralizedBalance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::15 => uint128 additionalData;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::42 => uint64 balanceLimit;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::103 => uint64 oldSenderBalance = erc721Data.userState[from].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::105 => uint64 oldRecipientBalance = erc721Data.userState[to].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::106 => uint64 newRecipientBalance = oldRecipientBalance + 1;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::200 => uint64 oldBalance = erc721Data.userState[to].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::205 => uint64 collateralizedTokens = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::247 => uint64 newBalance = oldBalance + uint64(tokenData.length);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::272 => uint64 burntCollateralizedTokens = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::273 => uint64 balanceToBurn;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::418 => uint8 i = 0;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::29 => uint8 rewardTokenDecimals;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::30 => uint8 precision;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::31 => uint8 priceFeedDecimals;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::54 => uint8 priceFeedDecimals;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::55 => uint8 rewardTokenDecimals;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::32 => uint128 liquidityIndex;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::33 => uint128 variableBorrowIndex;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::34 => uint128 liquidityRate;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::35 => uint128 variableBorrowRate;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::49 => uint128 accruedToTreasury;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::69 => uint8 networkBaseTokenPriceDecimals;
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::79 => uint128 liquidity;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::9 => uint88 emissionPerSecond;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::11 => uint32 distributionEnd;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::26 => uint128 accrued;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::31 => uint88 emissionPerSecond;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::32 => uint32 lastUpdateTimestamp;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::33 => uint32 distributionEnd;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::40 => uint128 availableRewardsCount;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::41 => uint8 decimals;
```

## [G-09] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::26 => mapping(bytes32 => bool) public airdropClaimRecords;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol::25 => bool private initializing;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::73 => bool public immutable ATOMIC_PRICING;
```

## [G-10] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::413 => for (uint256 i = 0; i < feederSize; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::442 => while (arr[uint256(i)] < pivot) i++;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::50 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::91 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::193 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::210 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::146 => for (uint256 i = 0; i < vars.airdropBalance; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::38 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::56 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::371 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::466 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::177 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::284 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::382 => for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::470 => for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::58 => for (uint16 i = 0; i < params.reservesCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::104 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::184 => for (uint256 index = 0; index < params.tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::195 => for (uint256 index = 0; index < params.tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::131 => for (uint256 index = 0; index < amount; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::212 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::465 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::910 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1034 => for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::86 => implIndex++
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::140 => selectorIndex++
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::151 => selectorPosition++;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::181 => selectorIndex++
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::193 => selectorPosition++;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::214 => selectorIndex++
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::72 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::103 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::138 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::164 => actualTransferAmount++;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::172 => for (uint256 index = 0; index < actualTransferAmount; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::199 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::215 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::279 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::289 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::305 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::86 => for (uint256 i = 0; i < input.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::348 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::634 => for (uint256 i = 0; i < reservesListCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::106 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::107 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::51 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::97 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::493 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::213 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::207 => for (uint256 index = 0; index < tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::234 => collateralizedTokens++;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::280 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::304 => balanceToBurn++;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::313 => burntCollateralizedTokens++;
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::58 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::85 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::146 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::214 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::237 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::304 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::89 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::262 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::267 => for (uint256 j = 0; j < size; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::284 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::289 => for (uint256 j = 0; j < size; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::370 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::423 => for (i = 0; i < 32 && _bytes32[i] != 0; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::462 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::480 => for (uint256 j = 0; j < tokenLength; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::73 => for (uint256 i = 0; i < users.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::74 => for (uint256 j = 0; j < tokens.length; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::97 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::104 => for (uint256 j = 0; j < reserves.length; j++) {
```

## [G-11] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::149 => token0Amount += positionData.tokensOwed0;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::150 => token1Amount += positionData.tokensOwed1;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::211 => sum += getTokenPrice(tokenIds[index]);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::237 => id += 1;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::169 => vars.payableDebtByERC20Assets += vars
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::176 => vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::178 => vars.totalCollateralInBaseCurrency += vars
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::185 => vars.avgLiquidationThreshold += vars.liquidationThreshold;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::189 => vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::231 => vars.avgERC721LiquidationThreshold += vars
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::233 => vars.totalERC721CollateralInBaseCurrency += vars
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::235 => vars.totalCollateralInBaseCurrency += vars
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::237 => vars.avgLtv += vars.ltv;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::238 => vars.avgLiquidationThreshold += vars.liquidationThreshold;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::380 => totalValue += _getTokenPrice(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::479 => totalValue += tokenPrice;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::496 => totalLTV += tmpLTV * tokenPrice;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::497 => totalLiquidationThreshold +=
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::83 => vars.ethLeft -= _buyWithCredit(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::205 => vars.ethLeft -= _buyWithCredit(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::397 => price += item.startAmount;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::252 => reserve.accruedToTreasury += vars
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::158 => vars.currentVariableBorrowRate +=
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::162 => vars.currentVariableBorrowRate += _variableRateSlope1
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::77 => amountToWithdraw += _nfts[index].amount;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::166 => amountToWithdraw += _nftPairs[index].amount;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::215 => totalAmount += getTokenIdStakingAmount(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::257 => apeStakedAmount += bakcStakedAmount;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::146 => erc721Data.userState[from].collateralizedBalance -= 1;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::318 => erc721Data.userState[user].balance -= balanceToBurn;
```

## [G-12] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::286 => abi.encode(
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::308 => return keccak256(abi.encode(initiator, nftAsset, nftTokenIds, params));
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1124 => abi.encode(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1136 => abi.encode(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::232 => abi.encode(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::66 => abi.encode(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::57 => abi.encode(
```

## [G-13] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::118 => require(_isAssetExisted(_asset), "NFTOracle: asset not existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::123 => require(!_isAssetExisted(_asset), "NFTOracle: asset existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::128 => require(_isFeederExisted(_feeder), "NFTOracle: feeder not existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::133 => require(!_isFeederExisted(_feeder), "NFTOracle: feeder existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::207 => require(dataValidity, "NFTOracle: invalid price data");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::267 => require(!_paused, "NFTOracle: nft price feed paused");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::47 => require(pairAddress != address(0x00), "pair not found");
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::68 => require(pairAddress != address(0x00), "pair not found");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::29 => require(owner_ != address(0), "zero owner address");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::30 => require(pool_ != address(0), "zero pool address");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::40 => require(_msgSender() == pool, "caller must be pool");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::66 => require(nftTokenIds.length > 0, "empty token list");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::99 => require(vars.airdropParams.length >= 4, "invalid airdrop parameters");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::245 => require(success, "ETH_TRANSFER_FAILED");
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol::64 => require(rgs._status != _ENTERED, "ReentrancyGuard: reentrant call");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::146 => require(success, "ETH_TRANSFER_FAILED");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::193 => require(to != owner, "ERC721: approval to old owner");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::72 => require(oldValue != incentive, "Same Value");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::92 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::164 => require(owner == sender, "not owner");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::199 => require(to != address(0), "ERC721: mint to the zero address");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::283 => require(owner == user, "not the owner of Ntoken");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::363 => require(owner != operator, "ERC721: approve to caller");
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::185 => require(success, "ETH_TRANSFER_FAILED");
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::228 => require(msg.sender == address(WETH), "Receive not allowed");
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::35 => require(msg.sender.isContract(), "22");
```

## [G-14] Use a more recent version of solidity

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Pair.sol::2 => pragma solidity >=0.5.0;
```

## [G-15] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::413 => for (uint256 i = 0; i < feederSize; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::442 => while (arr[uint256(i)] < pivot) i++;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::449 => i++;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::50 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::91 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::146 => for (uint256 i = 0; i < vars.airdropBalance; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::38 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::56 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::177 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::284 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::382 => for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::470 => for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::58 => for (uint16 i = 0; i < params.reservesCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::104 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1034 => for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::86 => for (uint256 i = 0; i < input.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::348 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::634 => for (uint256 i = 0; i < reservesListCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::58 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::214 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::89 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::262 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::284 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::370 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::420 => i++;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::423 => for (i = 0; i < 32 && _bytes32[i] != 0; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::462 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::73 => for (uint256 i = 0; i < users.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::97 => for (uint256 i = 0; i < reserves.length; i++) {
```

## [G-16] Don't compare boolean expressions to boolean literals

The extran comparison wastes gas
if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)

```
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::38 => if (supported == true) {
```

## [G-17] Use bytes32 instead of string

Use bytes32 instead of string to save gas whenever possible. String is a dynamic data structure and therefore is more gas consuming then bytes32.

```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::10 => string public constant CALLER_NOT_POOL_ADMIN = "1"; // 'The caller of the function is not a pool admin'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::11 => string public constant CALLER_NOT_EMERGENCY_ADMIN = "2"; // 'The caller of the function is not an emergency admin'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::12 => string public constant CALLER_NOT_POOL_OR_EMERGENCY_ADMIN = "3"; // 'The caller of the function is not a pool or emergency admin'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::13 => string public constant CALLER_NOT_RISK_OR_POOL_ADMIN = "4"; // 'The caller of the function is not a risk or pool admin'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::14 => string public constant CALLER_NOT_ASSET_LISTING_OR_POOL_ADMIN = "5"; // 'The caller of the function is not an asset listing or pool admin'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::15 => string public constant CALLER_NOT_BRIDGE = "6"; // 'The caller of the function is not a bridge'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::16 => string public constant ADDRESSES_PROVIDER_NOT_REGISTERED = "7"; // 'Pool addresses provider is not registered'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::17 => string public constant INVALID_ADDRESSES_PROVIDER_ID = "8"; // 'Invalid id for the pool addresses provider'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::18 => string public constant NOT_CONTRACT = "9"; // 'Address is not a contract'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::19 => string public constant CALLER_NOT_POOL_CONFIGURATOR = "10"; // 'The caller of the function is not the pool configurator'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::20 => string public constant CALLER_NOT_XTOKEN = "11"; // 'The caller of the function is not an PToken or NToken'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::21 => string public constant INVALID_ADDRESSES_PROVIDER = "12"; // 'The address of the pool addresses provider is invalid'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::22 => string public constant RESERVE_ALREADY_ADDED = "14"; // 'Reserve has already been added to reserve list'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::23 => string public constant NO_MORE_RESERVES_ALLOWED = "15"; // 'Maximum amount of reserves in the pool reached'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::24 => string public constant RESERVE_LIQUIDITY_NOT_ZERO = "18"; // 'The liquidity of the reserve needs to be 0'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::25 => string public constant INVALID_RESERVE_PARAMS = "20"; // 'Invalid risk parameters for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::26 => string public constant CALLER_MUST_BE_POOL = "23"; // 'The caller of this function must be a pool'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::27 => string public constant INVALID_MINT_AMOUNT = "24"; // 'Invalid amount to mint'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::28 => string public constant INVALID_BURN_AMOUNT = "25"; // 'Invalid amount to burn'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::29 => string public constant INVALID_AMOUNT = "26"; // 'Amount must be greater than 0'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::30 => string public constant RESERVE_INACTIVE = "27"; // 'Action requires an active reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::31 => string public constant RESERVE_FROZEN = "28"; // 'Action cannot be performed because the reserve is frozen'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::32 => string public constant RESERVE_PAUSED = "29"; // 'Action cannot be performed because the reserve is paused'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::33 => string public constant BORROWING_NOT_ENABLED = "30"; // 'Borrowing is not enabled'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::34 => string public constant STABLE_BORROWING_NOT_ENABLED = "31"; // 'Stable borrowing is not enabled'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::35 => string public constant NOT_ENOUGH_AVAILABLE_USER_BALANCE = "32"; // 'User cannot withdraw more than the available balance'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::36 => string public constant INVALID_INTEREST_RATE_MODE_SELECTED = "33"; // 'Invalid interest rate mode selected'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::37 => string public constant COLLATERAL_BALANCE_IS_ZERO = "34"; // 'The collateral balance is 0'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::38 => string public constant HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD =
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::40 => string public constant COLLATERAL_CANNOT_COVER_NEW_BORROW = "36"; // 'There is not enough collateral to cover a new borrow'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::41 => string public constant COLLATERAL_SAME_AS_BORROWING_CURRENCY = "37"; // 'Collateral is (mostly) the same currency that is being borrowed'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::42 => string public constant AMOUNT_BIGGER_THAN_MAX_LOAN_SIZE_STABLE = "38"; // 'The requested amount is greater than the max loan size in stable rate mode'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::43 => string public constant NO_DEBT_OF_SELECTED_TYPE = "39"; // 'For repayment of a specific type of debt, the user needs to have debt that type'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::44 => string public constant NO_EXPLICIT_AMOUNT_TO_REPAY_ON_BEHALF = "40"; // 'To repay on behalf of a user an explicit amount to repay is needed'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::45 => string public constant NO_OUTSTANDING_STABLE_DEBT = "41"; // 'User does not have outstanding stable rate debt on this reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::46 => string public constant NO_OUTSTANDING_VARIABLE_DEBT = "42"; // 'User does not have outstanding variable rate debt on this reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::47 => string public constant UNDERLYING_BALANCE_ZERO = "43"; // 'The underlying balance needs to be greater than 0'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::48 => string public constant INTEREST_RATE_REBALANCE_CONDITIONS_NOT_MET = "44"; // 'Interest rate rebalance conditions were not met'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::49 => string public constant HEALTH_FACTOR_NOT_BELOW_THRESHOLD = "45"; // 'Health factor is not below the threshold'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::50 => string public constant COLLATERAL_CANNOT_BE_AUCTIONED_OR_LIQUIDATED = "46"; // 'The collateral chosen cannot be auctioned OR liquidated'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::51 => string public constant SPECIFIED_CURRENCY_NOT_BORROWED_BY_USER = "47"; // 'User did not borrow the specified currency'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::52 => string public constant SAME_BLOCK_BORROW_REPAY = "48"; // 'Borrow and repay in same block is not allowed'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::53 => string public constant BORROW_CAP_EXCEEDED = "50"; // 'Borrow cap is exceeded'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::54 => string public constant SUPPLY_CAP_EXCEEDED = "51"; // 'Supply cap is exceeded'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::55 => string public constant XTOKEN_SUPPLY_NOT_ZERO = "54"; // 'PToken supply is not zero'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::56 => string public constant STABLE_DEBT_NOT_ZERO = "55"; // 'Stable debt supply is not zero'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::57 => string public constant VARIABLE_DEBT_SUPPLY_NOT_ZERO = "56"; // 'Variable debt supply is not zero'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::58 => string public constant LTV_VALIDATION_FAILED = "57"; // 'Ltv validation failed'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::59 => string public constant PRICE_ORACLE_SENTINEL_CHECK_FAILED = "59"; // 'Price oracle sentinel validation failed'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::60 => string public constant RESERVE_ALREADY_INITIALIZED = "61"; // 'Reserve has already been initialized'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::61 => string public constant INVALID_LTV = "63"; // 'Invalid ltv parameter for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::62 => string public constant INVALID_LIQ_THRESHOLD = "64"; // 'Invalid liquidity threshold parameter for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::63 => string public constant INVALID_LIQ_BONUS = "65"; // 'Invalid liquidity bonus parameter for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::64 => string public constant INVALID_DECIMALS = "66"; // 'Invalid decimals parameter of the underlying asset of the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::65 => string public constant INVALID_RESERVE_FACTOR = "67"; // 'Invalid reserve factor parameter for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::66 => string public constant INVALID_BORROW_CAP = "68"; // 'Invalid borrow cap for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::67 => string public constant INVALID_SUPPLY_CAP = "69"; // 'Invalid supply cap for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::68 => string public constant INVALID_LIQUIDATION_PROTOCOL_FEE = "70"; // 'Invalid liquidation protocol fee for the reserve'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::69 => string public constant INVALID_DEBT_CEILING = "73"; // 'Invalid debt ceiling for the reserve
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::70 => string public constant INVALID_RESERVE_INDEX = "74"; // 'Invalid reserve index'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::71 => string public constant ACL_ADMIN_CANNOT_BE_ZERO = "75"; // 'ACL admin cannot be set to the zero address'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::72 => string public constant INCONSISTENT_PARAMS_LENGTH = "76"; // 'Array parameters that should be equal length are not'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::73 => string public constant ZERO_ADDRESS_NOT_VALID = "77"; // 'Zero address not valid'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::74 => string public constant INVALID_EXPIRATION = "78"; // 'Invalid expiration'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::75 => string public constant INVALID_SIGNATURE = "79"; // 'Invalid signature'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::76 => string public constant OPERATION_NOT_SUPPORTED = "80"; // 'Operation not supported'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::77 => string public constant ASSET_NOT_LISTED = "82"; // 'Asset is not listed'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::78 => string public constant INVALID_OPTIMAL_USAGE_RATIO = "83"; // 'Invalid optimal usage ratio'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::79 => string public constant INVALID_OPTIMAL_STABLE_TO_TOTAL_DEBT_RATIO = "84"; // 'Invalid optimal stable to total debt ratio'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::80 => string public constant UNDERLYING_CANNOT_BE_RESCUED = "85"; // 'The underlying asset cannot be rescued'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::81 => string public constant ADDRESSES_PROVIDER_ALREADY_ADDED = "86"; // 'Reserve has already been added to reserve list'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::82 => string public constant POOL_ADDRESSES_DO_NOT_MATCH = "87"; // 'The token implementation pool address and the pool address provided by the initializing pool do not match'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::83 => string public constant STABLE_BORROWING_ENABLED = "88"; // 'Stable borrowing is enabled'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::84 => string public constant SILOED_BORROWING_VIOLATION = "89"; // 'User is trying to borrow multiple assets including a siloed one'
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::85 => string public constant RESERVE_DEBT_NOT_ZERO = "90"; // the total debt of the reserve needs to be 0
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::86 => string public constant NOT_THE_OWNER = "91"; // user is not the owner of a given asset
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::87 => string public constant LIQUIDATION_AMOUNT_NOT_ENOUGH = "92";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::88 => string public constant INVALID_ASSET_TYPE = "93"; // invalid asset type for action.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::89 => string public constant INVALID_FLASH_CLAIM_RECEIVER = "94"; // invalid flash claim receiver.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::90 => string public constant ERC721_HEALTH_FACTOR_NOT_BELOW_THRESHOLD = "95"; // ERC721 Health factor is not below the threshold. Can only liquidate ERC20.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::91 => string public constant UNDERLYING_ASSET_CAN_NOT_BE_TRANSFERRED = "96"; //underlying asset can not be transferred.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::92 => string public constant TOKEN_TRANSFERRED_CAN_NOT_BE_SELF_ADDRESS = "97"; //token transferred can not be self address.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::93 => string public constant INVALID_AIRDROP_CONTRACT_ADDRESS = "98"; //invalid airdrop contract address.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::94 => string public constant INVALID_AIRDROP_PARAMETERS = "99"; //invalid airdrop parameters.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::95 => string public constant CALL_AIRDROP_METHOD_FAILED = "100"; //call airdrop method failed.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::96 => string public constant SUPPLIER_NOT_NTOKEN = "101"; //supplier is not the NToken contract
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::97 => string public constant CALL_MARKETPLACE_FAILED = "102"; //call marketplace failed.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::98 => string public constant INVALID_MARKETPLACE_ID = "103"; //invalid marketplace id.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::99 => string public constant INVALID_MARKETPLACE_ORDER = "104"; //invalid marketplace id.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::100 => string public constant CREDIT_DOES_NOT_MATCH_ORDER = "105"; //credit doesn't match order.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::101 => string public constant PAYNOW_NOT_ENOUGH = "106"; //paynow not enough.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::102 => string public constant INVALID_CREDIT_SIGNATURE = "107"; //invalid credit signature.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::103 => string public constant INVALID_ORDER_TAKER = "108"; //invalid order taker.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::104 => string public constant MARKETPLACE_PAUSED = "109"; //marketplace paused.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::105 => string public constant INVALID_AUCTION_RECOVERY_HEALTH_FACTOR = "110"; //invalid auction recovery health factor.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::106 => string public constant AUCTION_ALREADY_STARTED = "111"; //auction already started.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::107 => string public constant AUCTION_NOT_STARTED = "112"; //auction not started yet.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::108 => string public constant AUCTION_NOT_ENABLED = "113"; //auction not enabled on the reserve.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::109 => string public constant ERC721_HEALTH_FACTOR_NOT_ABOVE_THRESHOLD = "114"; //ERC721 Health factor is not above the threshold.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::110 => string public constant TOKEN_IN_AUCTION = "115"; //tokenId is in auction.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::111 => string public constant AUCTIONED_BALANCE_NOT_ZERO = "116"; //auctioned balance not zero.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::112 => string public constant LIQUIDATOR_CAN_NOT_BE_SELF = "117"; //user can not liquidate himself.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::113 => string public constant INVALID_RECIPIENT = "118"; //invalid recipient specified in order.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::114 => string public constant UNIV3_NOT_ALLOWED = "119"; //flash claim is not allowed for UniswapV3.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::115 => string public constant NTOKEN_BALANCE_EXCEEDED = "120"; //ntoken balance exceed limit.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::116 => string public constant ORACLE_PRICE_NOT_READY = "121"; //oracle price not ready.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::117 => string public constant SET_ORACLE_SOURCE_NOT_ALLOWED = "122"; //source of oracle not allowed to set.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::118 => string public constant INVALID_LIQUIDATION_ASSET = "123"; //invalid liquidation asset.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::119 => string public constant ONLY_UNIV3_ALLOWED = "124"; //only UniswapV3 allowed.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::120 => string public constant GLOBAL_DEBT_IS_ZERO = "125"; //liquidation is not allowed when global debt is zero.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::121 => string public constant ORACLE_PRICE_EXPIRED = "126"; //oracle price expired.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::122 => string public constant APE_STAKING_POSITION_EXISTED = "127"; //ape staking position is existed.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::123 => string public constant SAPE_NOT_ALLOWED = "128"; //operation is not allow for sApe.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::124 => string public constant TOTAL_STAKING_AMOUNT_WRONG = "129"; //cash plus borrow amount not equal to total staking amount.
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::125 => string public constant APE_STAKING_AMOUNT_NON_ZERO = "130"; //ape staking amount should be zero when supply bayc/mayc.
```

## [G-18] Splitting require() statements that use && saves gas

Saves 16 gas per instance.
If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:

```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::55 => require(address(nBAYC) != address(0) && address(nMAYC) != address(0));
```

## [G-19] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::261 => function getFeederSize() public view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::63 => function getEthUsdPrice() public view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/UserFlashclaimRegistry.sol::18 => function createReceiver() public virtual override {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/RebasingDebtToken.sol::25 => function balanceOf(address user) public view override returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/RebasingDebtToken.sol::78 => function totalSupply() public view override returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/RebasingPToken.sol::26 => function balanceOf(address user) public view override returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/RebasingPToken.sol::73 => function totalSupply() public view override returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::34 => function DOMAIN_SEPARATOR() public view virtual returns (bytes32) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::46 => function nonces(address owner) public view virtual returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::91 => function name() public view override returns (string memory) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::106 => function totalSupply() public view virtual override returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::96 => function name() public view override returns (string memory) {
```

## [G-20] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::240 => returns (uint256 price)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::256 => returns (uint256 timestamp)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::400 => returns (bool, uint256)
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::290 => returns (address xTokenAddress, address variableDebtTokenAddress)
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/configuration/UserConfiguration.sol::210 => ) internal view returns (bool, address) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::608 => ) internal view returns (uint256, uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::637 => ) internal view returns (address, uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::379 => ) internal returns (uint256, uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::218 => ) external view returns (uint256 ltv, uint256 lt) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::709 => ) internal view returns (uint256, bool) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::129 => ) external view override returns (uint256, uint256) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::256 => returns (uint256 ltv, uint256 lt)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::82 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::91 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::99 => ) internal returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::106 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::44 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::56 => returns (uint256, uint256)
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::226 => try oracle.BASE_CURRENCY_UNIT() returns (uint256 baseCurrencyUnit) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::328 => try source.getTokenPrice(tokenId) returns (uint256 tokenPrice) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::476 => try oracle.getAssetPrice(assets[i]) returns (uint256 price) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::500 => returns (uint256 price) {
```

## [G-21] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::40 => mapping(address => PriceInformation) feederPrice;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::74 => mapping(address => PriceInformation) public assetPriceMap;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::81 => mapping(address => FeederPosition) private feederPositionMap;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::88 => mapping(address => FeederRegistrar) public assetFeederMap;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::20 => mapping(address => uint256) private _addressesProviderToId;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::26 => mapping(address => uint256) private _addressesProvidersIndexes;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::42 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::44 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::102 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::104 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::53 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::128 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::75 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::400 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::451 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::162 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::164 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::287 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::289 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::489 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::565 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::115 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::332 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::428 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::463 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::594 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::40 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::101 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::145 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::82 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::168 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::229 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::271 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::336 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::397 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::463 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::465 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::524 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::526 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::587 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::640 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::683 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::190 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::254 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::443 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::591 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::703 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::850 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::887 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::945 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1156 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::358 => mapping(address => ReserveData) _reserves;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::360 => mapping(address => UserConfigurationMap) _usersConfig;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::22 => mapping(address => mapping(address => uint256)) internal _borrowAllowances;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::57 => mapping(address => UserState) internal _userState;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::60 => mapping(address => mapping(address => uint256)) private _allowances;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::205 => mapping(address => UserState) storage userState,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::206 => mapping(address => mapping(uint256 => uint256)) storage ownedTokens,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::26 => mapping(address => mapping(uint256 => uint256)) ownedTokens;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::34 => mapping(address => UserState) userState;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::38 => mapping(address => mapping(address => bool)) operatorApprovals;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::40 => mapping(address => mapping(address => uint256)) allowances;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::34 => mapping(address => UserData) usersData;
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::38 => mapping(address => RewardData) rewards;
```

## [G-22] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::23 => function _onlyAssetListingOrPoolAdmins() internal view {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::265 => function _whenNotPaused(address _asset) internal view {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::388 => function _addRawValue(address _asset, uint256 _twap) internal {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::218 => function _onlyAssetListingOrPoolAdmins() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::114 => function _addToAddressesProvidersList(address provider) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::123 => function _removeFromAddressesProvidersList(address provider) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::552 => function _checkAllowance(address token, address operator) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1133 => function getDomainSeparator() internal view returns (bytes32) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::29 => function wadMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::49 => function wadDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::70 => function rayMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::90 => function rayDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::110 => function rayToWad(uint256 a) internal pure returns (uint256 b) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::126 => function wadToRay(uint256 a) internal pure returns (uint256 b) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol::43 => function rgStorage() internal pure returns (RGStorage storage rgs) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol::74 => function getRevision() internal pure virtual returns (uint256);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol::57 => function getRevision() internal pure virtual returns (uint256);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::53 => function setContractOwner(address _newOwner) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::60 => function contractOwner() internal view returns (address contractOwner_) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::64 => function enforceIsContractOwner() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::413 => function setSApeUseAsCollateral(address user) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::371 => function _checkNoBorrowers(address asset) internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::378 => function _onlyPoolAdmin() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::388 => function _onlyEmergencyAdmin() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::398 => function _onlyPoolOrEmergencyAdmin() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::409 => function _onlyAssetListingOrPoolAdmins() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::420 => function _onlyRiskOrPoolAdmins() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::69 => function _onlyPoolConfigurator() internal view virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::76 => function _onlyPoolAdmin() internal view virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::130 => function initializeStakingData() internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::144 => function _safeTransferETH(address to, uint256 value) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::54 => function _calculateDomainSeparator() internal view returns (bytes32) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::71 => function _EIP712BaseId() internal view virtual returns (string memory);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::287 => function _setName(string memory newName) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::295 => function _setSymbol(string memory newSymbol) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::303 => function _setDecimals(uint8 newDecimals) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::35 => function _mint(address account, uint128 amount) internal virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::57 => function _burn(address account, uint128 amount) internal virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::150 => function _setName(string memory newName) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::158 => function _setSymbol(string memory newSymbol) internal {
```

## [G-23] internal functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::29 => function wadMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::49 => function wadDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::70 => function rayMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::90 => function rayDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::110 => function rayToWad(uint256 a) internal pure returns (uint256 b) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/WadRayMath.sol::126 => function wadToRay(uint256 a) internal pure returns (uint256 b) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::53 => function setContractOwner(address _newOwner) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::60 => function contractOwner() internal view returns (address contractOwner_) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::64 => function enforceIsContractOwner() internal view {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::287 => function _setName(string memory newName) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::295 => function _setSymbol(string memory newSymbol) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::303 => function _setDecimals(uint8 newDecimals) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::35 => function _mint(address account, uint128 amount) internal virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::57 => function _burn(address account, uint128 amount) internal virtual {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::150 => function _setName(string memory newName) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::158 => function _setSymbol(string memory newSymbol) internal {
```