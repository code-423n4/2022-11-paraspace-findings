# Unsafe Downcasting Operation

#### line of code.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L284

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L312

#### vulnerability detail and recommended fix

The unsafe downcasting can truncated the arithmic value unexpected.

```solidity
assetFeederMap[_asset].index = uint8(assets.length - 1);
```

and

```solidity
feederPositionMap[_feeder].index = uint8(feeders.length - 1);
```

We recommend using Openzeppelin safeCast.

https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast

# Credit Signature never expires.

#### line of code.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1117

#### vulnerability detail and recommended fix

The credit signature schema does not have expiration timestamp so the credit signature never expires.

```solidity
function hashCredit(DataTypes.Credit memory credit)
	private
	pure
	returns (bytes32)
{
	bytes32 typeHash = keccak256(
		abi.encodePacked(
			"Credit(address token,uint256 amount,bytes orderId)"
		)
	);

	// https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md#definition-of-encodedata
	return
		keccak256(
			abi.encode(
				typeHash,
				credit.token,
				credit.amount,
				keccak256(abi.encodePacked(credit.orderId))
			)
		);
}
```

the user can sign a credit order, and the order is never executed, then after 10 days, it is no longer profitable to execute the order or the user does not execute the order, but the signature does not expire.

This is QA because the Opensea order, looksrare and X2Y2 both has deadine and expiration time. 

We recommend the project add nonce to make sure the signature cannot be reused and also add timestamp to make sure the credit signature has expiraton time.

# UniswapV3OracleWrapper.sol latestAnswer() alwasy revert.

#### line of code.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L218

#### vulnerability detail and recommended fix

below is the current implementation of the Paraspace oracle,

```solidity
/// @inheritdoc IPriceOracleGetter
function getAssetPrice(address asset)
	public
	view
	override
	returns (uint256)
{
	if (asset == BASE_CURRENCY) {
		return BASE_CURRENCY_UNIT;
	}

	uint256 price = 0;
	IEACAggregatorProxy source = IEACAggregatorProxy(assetsSources[asset]);
	if (address(source) != address(0)) {
		price = uint256(source.latestAnswer());
	}
	if (price == 0 && address(_fallbackOracle) != address(0)) {
		price = _fallbackOracle.getAssetPrice(asset);
	}

	require(price != 0, Errors.ORACLE_PRICE_NOT_READY);
	return price;
}
```

note that the oracle source is expected to implement source.latestAnswer, however, the UniswapV3OracleWrapper.sol does not implement this function.

```solidity
function latestAnswer() external pure returns (int256) {
	revert("unimplemented");
}
```

We recommend the project implement latestAnswer function in UniswapV3Oracle to avoid transactoinr revert when using UniswapV3OracleWrapper.


# For PoolApeStaking.sol: lack of method to preview the claimable reward amount.

#### line of code.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L23

#### vulnerability detail and recommended fix

There is no method to preview the claimable reward amount in PoolApeStaking.sol.

We recommend the project add the method to preview the claimable amount of reward to let user keep track of their position properly.

The method is

https://github.com/HorizenLabs/ape-staking-public/blob/6b86a6d92153455fe9f33b0bda16e4141099fc7d/contracts/ApeCoinStaking.sol#L897

```solidity
function pendingRewards(uint256 _poolId, address _address, uint256 _tokenId) external view returns (uint256) {
	Pool memory pool = pools[_poolId];
	Position memory position = _poolId == 0 ? addressPosition[_address]: nftPosition[_poolId][_tokenId];

	(uint256 rewardsSinceLastCalculated,) = rewardsBy(_poolId, pool.lastRewardedTimestampHour, getPreviousTimestampHour());
	uint256 accumulatedRewardsPerShare = pool.accumulatedRewardsPerShare;

	if (block.timestamp > pool.lastRewardedTimestampHour + SECONDS_PER_HOUR && pool.stakedAmount != 0) {
		accumulatedRewardsPerShare = accumulatedRewardsPerShare + rewardsSinceLastCalculated * APE_COIN_PRECISION / pool.stakedAmount;
	}
	return ((position.stakedAmount * accumulatedRewardsPerShare).toInt256() - position.rewardsDebt).toUint256() / APE_COIN_PRECISION;
}
```


# Function return too many parameters and the developer needs to worry about the order of the parameter.

#### line of code.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L74

#### vulnerability detail and recommended fix

the function calculateUserAccountData return too many parameters, the developer has to calibrate the order of the parameter, which is very error-prone.

```solidity
(, , , , , , , uint256 healthFactor, , ) = GenericLogic
	.calculateUserAccountData(ps._reserves, ps._reservesList, params);
return healthFactor;
```

We recommend the project wrap the returned parameter in a struct so the function can access a parameter using struct.xxx, instead of figuring the order of the parameter.

# outdated soliditiy version

#### line of code.

All file

#### vulnerability detail and recommended fix

The code use

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity 0.8.10;
```

We recommend the use the latest version (0.8.17) to make sure the bugs in the oudated version does not affect the implementation.

# onERC721Received Hook is missing PoolMarketPlace.sol

#### line of code.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

#### vulnerability detail and recommended fix

We need to trace the function call in PoolMarketPlace.sol

```solidity
/// @inheritdoc IPoolMarketplace
function batchAcceptBidWithCredit(
	bytes32[] calldata marketplaceIds,
	bytes[] calldata payloads,
	DataTypes.Credit[] calldata credits,
	address onBehalfOf,
	uint16 referralCode
) external virtual override nonReentrant {
	DataTypes.PoolStorage storage ps = poolStorage();

	MarketplaceLogic.executeBatchAcceptBidWithCredit(
		marketplaceIds,
		payloads,
		credits,
		onBehalfOf,
		ps,
		ADDRESSES_PROVIDER,
		referralCode
	);
}
```

which calls:

```solidity
function executeBatchAcceptBidWithCredit(
	bytes32[] calldata marketplaceIds,
	bytes[] calldata payloads,
	DataTypes.Credit[] calldata credits,
	address onBehalfOf,
	DataTypes.PoolStorage storage ps,
	IPoolAddressesProvider poolAddressProvider,
	uint16 referralCode
) external {
```

which calls:

```solidity
_acceptBidWithCredit(
	ps._reserves,
	ps._reservesList,
	ps._usersConfig[vars.orderInfo.maker],
	DataTypes.ExecuteMarketplaceParams({
		marketplaceId: vars.marketplaceId,
		payload: vars.payload,
		credit: credit,
		ethLeft: 0,
		marketplace: vars.marketplace,
		orderInfo: vars.orderInfo,
		weth: vars.weth,
		referralCode: referralCode,
		reservesCount: ps._reservesCount,
		oracle: poolAddressProvider.getPriceOracle(),
		priceOracleSentinel: poolAddressProvider
			.getPriceOracleSentinel()
	})
);
```

which calls:

```solidity
_borrowTo(reservesData, params, vars, params.orderInfo.maker);

// delegateCall to avoid extra token transfer
Address.functionDelegateCall(
	params.marketplace.adapter,
	abi.encodeWithSelector(
		IMarketplace.matchBidWithTakerAsk.selector,
		params.marketplace.marketplace,
		params.payload
	)
);

_repay(
	reservesData,
	reservesList,
	userConfig,
	params,
	vars,
	params.orderInfo.maker
);
```

which calls:

```solidity
_repay(
	reservesData,
	reservesList,
	userConfig,
	params,
	vars,
	params.orderInfo.maker
);
```

note the logic in the _repay function:

```solidity
 // item.token == underlyingAsset but supplied after listing/offering
// so NToken is transferred instead
if (INToken(vars.xTokenAddress).ownerOf(tokenId) == address(this)) {
	_transferAndCollateralize(
		reservesData,
		userConfig,
		vars,
		token,
		tokenId,
		onBehalfOf
	);
	// item.token == underlyingAsset and underlyingAsset stays in wallet
} else {
	DataTypes.ERC721SupplyParams[]
		memory tokenData = new DataTypes.ERC721SupplyParams[](1);
	tokenData[0] = DataTypes.ERC721SupplyParams(tokenId, true);
	SupplyLogic.executeSupplyERC721(
		reservesData,
		userConfig,
		DataTypes.ExecuteSupplyERC721Params({
			asset: token,
			tokenData: tokenData,
			onBehalfOf: onBehalfOf,
			payer: address(this),
		   referralCode: params.referralCode
		})
	);
}
```

note the section:

```solidity
DataTypes.ERC721SupplyParams[]
		memory tokenData = new DataTypes.ERC721SupplyParams[](1);
	tokenData[0] = DataTypes.ERC721SupplyParams(tokenId, true);
	SupplyLogic.executeSupplyERC721(
		reservesData,
		userConfig,
		DataTypes.ExecuteSupplyERC721Params({
			asset: token,
			tokenData: tokenData,
			onBehalfOf: onBehalfOf,
			payer: address(this),
		   referralCode: params.referralCode
		})
	);
```

the code assumes that the PoolMarketPlace hold NFT because payer is address(this), which is the PoolMarketPlace.sol

the problem is when trading, the Opensea NFT transfer does not use safeTransfer and does not require smart contract receiver to implement onERC721Received.

LooksRare NFT transfer does not use safeTransfer and does not require smart contract receiver to implement onERC721Received.

But X2Y2 market use safeTransfer actually. so without implement onERC721Received in PoolMarketPlace.sol, the PoolMarketPlace.sol contract is not capable of receiving NFT when the markplace use safeTransfer.

Thus we recommend the project add onERC721Received in the PoolMarketPlace.sol

