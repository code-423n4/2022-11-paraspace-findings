

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gusset (20000 gas)
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::77 => address[] public feeders;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::84 => address[] public assets;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::91 => OracleConfig public config;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::27 => IPriceOracleGetter private _fallbackOracle;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::22 => string private _marketId;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::31 => address public proxy;
```



### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::31 => address public proxy;
```





### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
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
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::128 => uint256 amount = params.tokenData.length;
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
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::106 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::107 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::51 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::97 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::493 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::207 => for (uint256 index = 0; index < tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::280 => for (uint256 index = 0; index < tokenIds.length; index++) {
```



### [G04] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::413 => for (uint256 i = 0; i < feederSize; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::193 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::210 => for (uint256 index = 0; index < tokenIds.length; index++) {
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
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::72 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::103 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::138 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::172 => for (uint256 index = 0; index < actualTransferAmount; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::199 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::215 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::279 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::289 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::305 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::634 => for (uint256 i = 0; i < reservesListCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::106 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::107 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::51 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::97 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::493 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::213 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::207 => for (uint256 index = 0; index < tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::280 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
```



### [G05] `require()`/`revert()` strings longer than 32 bytes cost extra gas


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::92 => require(to != address(0), "ERC721: transfer to the zero address");
```




### [G06] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::51 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::84 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
```



### [G07] It costs more gas to initialize variables to zero than to let the default of zero be applied

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::411 => uint256 validNum = 0;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::413 => for (uint256 i = 0; i < feederSize; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::125 => uint256 price = 0;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::193 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::208 => uint256 sum = 0;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::210 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::38 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::56 => for (i = 0; i < params.nftTokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::371 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::466 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::177 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::284 => for (uint256 i = 0; i < marketplaceIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::380 => uint256 price = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::382 => for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::409 => price = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::410 => downpayment = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::470 => for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::589 => vars.ethLeft = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::58 => for (uint16 i = 0; i < params.reservesCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::104 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::123 => reserve.accruedToTreasury = 0;
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
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
```



### [G08] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::229 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::291 => for (uint256 i = 0; i < _assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::321 => for (uint256 i = 0; i < _feeders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::413 => for (uint256 i = 0; i < feederSize; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::95 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::197 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::193 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::210 => for (uint256 index = 0; index < tokenIds.length; index++) {
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
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::72 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::103 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::138 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::172 => for (uint256 index = 0; index < actualTransferAmount; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::199 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::215 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::279 => for (uint256 index = 0; index < _nfts.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::289 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::305 => for (uint256 index = 0; index < _nftPairs.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::634 => for (uint256 i = 0; i < reservesListCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::106 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::145 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::107 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::51 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::97 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::493 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::213 => for (uint256 index = 0; index < totalBalance; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::207 => for (uint256 index = 0; index < tokenData.length; index++) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::280 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::82 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::109 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::113 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::136 => for (uint256 i = 0; i < punkIndexes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::174 => for (uint256 i = 0; i < punkIndexes.length; i++) {
```





### [G09] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::9 => uint8 constant MIN_ORACLES_NUM = 3;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::12 => uint128 constant EXPIRATION_PERIOD = 1800;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::14 => uint128 constant MAX_DEVIATION_RATE = 150;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::18 => uint128 expirationPeriod;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::20 => uint128 maxPriceDeviation;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::36 => uint8 index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::47 => uint8 index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::63 => event OracleConfigSet(uint128 expirationPeriod, uint128 maxPriceDeviation);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::175 => function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::284 => assetFeederMap[_asset].index = uint8(assets.length - 1);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::300 => uint8 assetIndex = assetFeederMap[_asset].index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::312 => feederPositionMap[_feeder].index = uint8(feeders.length - 1);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::330 => uint8 feederIndex = feederPositionMap[_feeder].index;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::343 => function _setConfig(uint128 _expirationPeriod, uint128 _maxPriceDeviation)
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::43 => uint8 token0Decimal;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::44 => uint8 token1Decimal;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::45 => uint160 sqrtPriceX96;
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::61 => uint24 fee,
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::64 => uint128 liquidity,
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::74 => (uint160 currentPrice, int24 currentTick, , , , , ) = pool.slot0();
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::243 => oracleData.sqrtPriceX96 = uint160(
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::251 => oracleData.sqrtPriceX96 = uint160(
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::263 => oracleData.sqrtPriceX96 = uint160(
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::362 => token0Amount = uint128(
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::371 => token1Amount = uint128(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::33 => uint16 indexed referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::125 => uint16 liquidationAssetReserveId;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::69 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::166 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::236 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::274 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::58 => for (uint16 i = 0; i < params.reservesCount; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::61 => uint16 indexed referralCode,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::135 => uint16 reserveId,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::144 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::145 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::356 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::357 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::95 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::117 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::160 => uint16 referralCode,
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::162 => uint8 permitV,
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::240 => uint128 liquidityDecrease,
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::270 => uint16 referralCode,
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::343 => uint8 permitV,
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::650 => function getReserveAddressById(uint16 id) external view returns (address) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::662 => returns (uint16)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::673 => returns (uint64)
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::75 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::94 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::114 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::135 => uint16 referralCode
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::82 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::91 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::99 => ) internal returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::101 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::102 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::44 => ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::46 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::47 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::54 => uint128 liquidityDecrease,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::100 => amount0Max: type(uint128).max,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::101 => amount1Max: type(uint128).max
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::126 => uint128 liquidityDecrease,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::142 => function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::371 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::372 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::388 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::389 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::13 => uint64 balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::14 => uint64 collateralizedBalance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::15 => uint128 additionalData;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::42 => uint64 balanceLimit;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::103 => uint64 oldSenderBalance = erc721Data.userState[from].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::105 => uint64 oldRecipientBalance = erc721Data.userState[to].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::106 => uint64 newRecipientBalance = oldRecipientBalance + 1;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::173 => uint64 collateralizedBalance = erc721Data
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::195 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::196 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::200 => uint64 oldBalance = erc721Data.userState[to].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::268 => uint64 oldCollateralizedBalance,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::269 => uint64 newCollateralizedBalance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::272 => uint64 burntCollateralizedTokens = 0;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::273 => uint64 balanceToBurn;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::405 => uint64 balance
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::408 => uint64 balanceLimit = erc721Data.balanceLimit;
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::172 => uint16 referralCode
```



### [G10] Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

#### Impact
See [this](https://github.com/ethereum/solidity/issues/9232) issue for a detailed description of the issue
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::70 => bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
```



### [G11] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::118 => require(_isAssetExisted(_asset), "NFTOracle: asset not existed");
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::152 => revert(Errors.ORACLE_PRICE_NOT_READY);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::70 => revert(Errors.CALL_MARKETPLACE_FAILED);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::51 => require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::110 => revert(Errors.CALL_MARKETPLACE_FAILED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::68 => require(amount != 0, Errors.INVALID_AMOUNT);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::85 => require(isActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::86 => require(!isPaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::768 => require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::769 => require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::937 => require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1074 => require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::157 => require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
```





### [G12] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::118 => require(_isAssetExisted(_asset), "NFTOracle: asset not existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::123 => require(!_isAssetExisted(_asset), "NFTOracle: asset existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::128 => require(_isFeederExisted(_feeder), "NFTOracle: feeder not existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::133 => require(!_isFeederExisted(_feeder), "NFTOracle: feeder existed");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::207 => require(dataValidity, "NFTOracle: invalid price data");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::267 => require(!_paused, "NFTOracle: nft price feed paused");
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::218 => revert("unimplemented");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::146 => require(success, "ETH_TRANSFER_FAILED");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::193 => require(to != owner, "ERC721: approval to old owner");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::72 => require(oldValue != incentive, "Same Value");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::92 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::164 => require(owner == sender, "not owner");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::199 => require(to != address(0), "ERC721: mint to the zero address");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::283 => require(owner == user, "not the owner of Ntoken");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::363 => require(owner != operator, "ERC721: approve to caller");
```



### [G13] Functions guaranteed to revert when called by normal users can be marked `payable`


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::158 => function setACLManager(address newAclManager) external override onlyOwner {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::170 => function setACLAdmin(address newAclAdmin) external override onlyOwner {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::235 => function setWETH(address newWETH) external override onlyOwner {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::136 => function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::142 => function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
```





### [G14] Bitshift for divide by 2

#### Impact
When multiplying or dividing by a power of two, it is cheaper to bitshift than to use standard math operations. 
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::429 => return (true, validPriceList[validNum / 2]);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::440 => uint256 pivot = arr[uint256(left + (right - left) / 2)];
```


### [G15] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.

There are several cases of function arguments using memory instead of
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::290 => function _addAssets(address[] memory _assets) internal {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::320 => function _addFeeders(address[] memory _feeders) internal {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::221 => function _getOracleData(UinswapV3PositionData memory positionData)
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::282 => function _getPendingFeeAmounts(UinswapV3PositionData memory positionData)
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::25 => function getAskOrderInfo(bytes memory params, address weth)
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::25 => function getAskOrderInfo(bytes memory params, address)
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::60 => function getBidOrderInfo(bytes memory params)
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::124 => function isBasicOrder(AdvancedOrder memory advancedOrder)
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::31 => function getAskOrderInfo(bytes memory params, address)
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::79 => function getBidOrderInfo(bytes memory)
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::56 => function setMarketId(string memory newMarketId)
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::329 => function _setMarketId(string memory newMarketId) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::559 => function _cache(DataTypes.ExecuteMarketplaceParams memory params)
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1110 => function hashCredit(DataTypes.Credit memory credit)
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::150 => function _setName(string memory newName) internal {
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::158 => function _setSymbol(string memory newSymbol) internal {
```







### [G16] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done in some places, it’s not consistently done in the solution.
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::55 => _apeCoinStaking.apeCoin().safeTransfer(_apeRecipient, balance);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::168 => _apeCoin.safeTransfer(
```






### [G17] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
aves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::40 => mapping(address => PriceInformation) feederPrice;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::74 => mapping(address => PriceInformation) public assetPriceMap;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::81 => mapping(address => FeederPosition) private feederPositionMap;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::88 => mapping(address => FeederRegistrar) public assetFeederMap;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::42 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::43 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::44 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::102 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::103 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::104 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::53 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::128 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::75 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::76 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::400 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::451 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::162 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::163 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::164 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::287 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::288 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::289 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::489 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::565 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::115 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::116 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::332 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::333 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::428 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::463 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::464 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::594 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::40 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::41 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::101 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::145 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::146 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::82 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::168 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::229 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::271 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::272 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::336 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::337 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::397 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::398 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::463 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::464 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::465 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::524 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::525 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::526 => mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::587 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::588 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::640 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::683 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::684 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::190 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::254 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::255 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::443 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::591 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::703 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::704 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::850 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::851 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::887 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::888 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::945 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::971 => mapping(uint256 => address) storage reservesList,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1156 => mapping(address => DataTypes.ReserveData) storage reservesData,
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::24 => mapping(uint256 => address) owners;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::28 => mapping(uint256 => uint256) ownedTokensIndex;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::32 => mapping(uint256 => uint256) allTokensIndex;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::36 => mapping(uint256 => address) tokenApprovals;
```



### [G18] Using `private` rather than `public` for constants, saves gas


#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::70 => bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::57 => uint256 public constant MAX_LIQUIDATION_CLOSE_FACTOR = 1e4;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::64 => uint256 public constant CLOSE_FACTOR_HF_THRESHOLD = 0.95e18;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::45 => uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::49 => uint256 public constant MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD =
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::56 => uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::53 => uint256 public constant POOL_REVISION = 1;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::32 => uint256 public constant NTOKEN_REVISION = 0x1;
```





### [G19] Empty blocks should be removed or emit something

#### Impact
The code should be refactored such that they no longer exist, or the 
block should do something useful, such as emitting an event or 
reverting. If the contract is meant to be extended, the contract should 
be abstract and the function signatures be added without 
any default implementation. If the block is an empty if-statement block 
to avoid doing subsequent checks in the else-if/else conditions, the 
else-if/else conditions should be nested under the negation of the 
if-statement, because they involve different classes of checks, which 
may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::23 => constructor() {}
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::23 => constructor() {}
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::24 => constructor() {}
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::50 => {}
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol::18 => {}
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol::18 => {}
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::149 => receive() external payable {}
```



### [G20] `abi.encode()` is less efficient than abi.encodePacked()



#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1124 => abi.encode(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1136 => abi.encode(
```
