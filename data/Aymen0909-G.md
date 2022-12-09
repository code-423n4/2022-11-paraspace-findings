# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Variables inside struct should be packed to save gas  | 1  |
| 2      | Remove redundant checks to save gas |  2 |
| 3      | Remove redundant state variables  | 3 |
| 4      | Use `unchecked` blocks to save gas  |  1 |
| 5      | `require()` strings longer than 32 bytes cost extra gas  |  12 |
| 6      | Splitting `require()` statements that uses && saves gas  |  12 |
| 7      | `memory` values should be emitted in events instead of `storage` ones  |  1 |
| 8      | `++i/i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops |  46  |

## Findings

### 1- Variables inside `struct` should be packed to save gas :

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

There is 1 instance of this issue:

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 23-30](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L23-L30)

```
struct PriceInformation {
    // last reported floor price(offchain twap)
    uint256 twap;
    // last updated blocknumber
    uint256 updatedAt;
    // last updated timestamp
    uint256 updatedTimestamp;
}
```

As the `updatedAt` and `updatedTimestamp` parameters values can't really overflow 2^128 they should be packed together and the struct should be modified as follow to save gas :

```
struct PriceInformation {
    // last reported floor price(offchain twap)
    uint256 twap;
    // last updated blocknumber
    uint128 updatedAt;
    // last updated timestamp
    uint128 updatedTimestamp;
}
```

### 2- Remove redundant checks to save gas :

When a public/external function calls an internal function it is redundant to include the same modifier in both functions and it will cost more gas, so it's recommended to use the modifier only on one of the functions (either the internal or the public).

There are 2 instances of this issue:

The public function `removeAsset` calls the internal function `_removeAsset` and both contain the same modifier `onlyWhenAssetExisted(_asset)`, so this modifier should be removed from one of them.

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 148-154](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L148-L154)
```
function removeAsset(address _asset)
    external
    onlyRole(DEFAULT_ADMIN_ROLE)
    onlyWhenAssetExisted(_asset)
{
    _removeAsset(_asset);
}
```

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 296-298](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296-L298)
```
function _removeAsset(address _asset) internal onlyWhenAssetExisted(_asset)
```

The public function `removeFeeder` calls the internal function `_removeFeeder` and both contain the same modifier `onlyWhenFeederExisted(_feeder)`, so this modifier should be removed from one of them.

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 167-172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167-L172)
```
function removeFeeder(address _feeder)
    external
    onlyWhenFeederExisted(_feeder)
{
    _removeFeeder(_feeder);
}
```

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 326-328](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L326-L328)
```
function _removeFeeder(address _feeder) internal onlyWhenFeederExisted(_feeder)
```

### 3- Remove redundant state variables :

In the `WPunkGateway` contract the following state variables are redundant and can be removed to save gas :

File: paraspace-core/contracts/ui/WPunkGateway.sol [Line 33-35](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L33-L35)
```
address public immutable punk;
address public immutable wpunk;
address public immutable pool;
```

Those variables are unnecessary because for each one there existe another state variable that store the same value : 

```
IPunks internal immutable Punk;
IWrappedPunks internal immutable WPunk;
IPool internal immutable Pool;
```

And for example if we need to get the `pool` address we can obtain it by using `address(Pool)` directly (and the same apply for the other variables), thus the aformentioned variables should be removed to save deployemnt gas.


### 4- Use `unchecked` blocks to save gas :

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

There is 1 instance of this issue:

File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol [Line 317-319](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L317-L319)

```
availableBorrowsInBaseCurrency = availableBorrowsInBaseCurrency - totalDebtInBaseCurrency;
```

The above operation cannot underflow due to the check [if (availableBorrowsInBaseCurrency < totalDebtInBaseCurrency)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L313) It should be marked as `unchecked`. 

### 5- `require()` strings longer than 32 bytes cost extra gas :

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas.

There are 12 instances of this issue :

```
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

225     require(
            _assets.length == _twaps.length,
            "NFTOracle: Tokens and price length differ"
        );
267     require(!_paused, "NFTOracle: nft price feed paused");
356     require(_twap > 0, "NFTOracle: price should be more than 0");

File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

195     require(
            _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
            "ERC721: approve caller is not owner nor approved for all"
        );
213     require(
            _exists(tokenId),
            "ERC721: approved query for nonexistent token"
        );
259     require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );
296     require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );
354     require(
            _exists(tokenId),
            "ERC721: operator query for nonexistent token"
        );
594     require(
            index < balanceOf(owner),
            "ERC721Enumerable: owner index out of bounds"
        );
618     require(
            index < totalSupply(),
            "ERC721Enumerable: global index out of bounds"
        );

File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

377     require(
            _exists(erc721Data, tokenId),
            "ERC721: startAuction for nonexistent token"
        );
395     require(
            _exists(erc721Data, tokenId),
            "ERC721: endAuction for nonexistent token"
        );
```

### 6- Splitting `require()` statements that uses && saves gas :

There are 12 instances of this issue :

```
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

171     require(
            marketplaceIds.length == payloads.length &&
                payloads.length == credits.length,
            Errors.INCONSISTENT_PARAMS_LENGTH
        );
279     require(
            marketplaceIds.length == payloads.length &&
                payloads.length == credits.length,
            Errors.INCONSISTENT_PARAMS_LENGTH
        );
388     require(
            item.itemType == ItemType.ERC20 ||
                (vars.isETH && item.itemType == ItemType.NATIVE),
            Errors.INVALID_ASSET_TYPE
        );

File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

546     require(
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
        );
550     require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
        );
636     require(
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
        );
640     require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
        );
672     require(
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&
                (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
            Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
        );
1196    require(
            vars.token0IsActive && vars.token1IsActive,
            Errors.RESERVE_INACTIVE
        );
1202    require(
            !vars.token0IsPaused && !vars.token1IsPaused,
            Errors.RESERVE_PAUSED
        );
1208    require(
            !vars.token0IsFrozen && !vars.token1IsFrozen,
            Errors.RESERVE_FROZEN
        );

File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

216     require(
            value > MIN_AUCTION_HEALTH_FACTOR &&
                value <= MAX_AUCTION_HEALTH_FACTOR,
            Errors.INVALID_AMOUNT
        );
```

### 7- `memory` values should be emitted in events instead of `storage` ones :

The values emitted in events shouldn’t be read from storage but the existing memory values should be used instead, this will save **~101 GAS**.

There is 1 instance of this issue :

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 381-385](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381-L385)
```
emit AssetDataSet(
    _asset,
    assetPriceMapEntry.twap,
    assetPriceMapEntry.updatedAt
);
```

The memory values should be emitted as follow :

```
emit AssetDataSet(
    _asset,
    _twap,
    block.number
);
```

### 8- `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops :

There are 46 instances of this issue:
 
```
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

229     for (uint256 i = 0; i < _assets.length; i++)
291     for (uint256 i = 0; i < _assets.length; i++)
321     for (uint256 i = 0; i < _feeders.length; i++)
413     for (uint256 i = 0; i < feederSize; i++)

File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

95      for (uint256 i = 0; i < assets.length; i++)
197     for (uint256 i = 0; i < assets.length; i++)

File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

183     for (uint256 index = 0; index < tokenIds.length; index++)
210     for (uint256 index = 0; index < tokenIds.length; index++)

File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38      for (i = 0; i < params.nftTokenIds.length; i++)
56      for (i = 0; i < params.nftTokenIds.length; i++)

File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

371     for (uint256 index = 0; index < totalBalance; index++)
466     for (uint256 index = 0; index < totalBalance; index++)

File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177     for (uint256 i = 0; i < marketplaceIds.length; i++)
284     for (uint256 i = 0; i < marketplaceIds.length; i++)
382     for (uint256 i = 0; i < params.orderInfo.consideration.length; i++)
470     for (uint256 i = 0; i < params.orderInfo.offer.length; i++)

File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58      for (uint16 i = 0; i < params.reservesCount; i++)
104     for (uint256 i = 0; i < assets.length; i++)

File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

195     for (uint256 index = 0; index < params.tokenData.length; index++)

File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

131     for (uint256 index = 0; index < amount; index++)
465     for (uint256 index = 0; index < tokenIds.length; index++)
910     for (uint256 index = 0; index < tokenIds.length; index++)
1034    for (uint256 i = 0; i < params.nftTokenIds.length; i++)

File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72      for (uint256 index = 0; index < _nfts.length; index++)
103     for (uint256 index = 0; index < _nfts.length; index++)
138     for (uint256 index = 0; index < _nftPairs.length; index++)
172     for (uint256 index = 0; index < actualTransferAmount; index++)
199     for (uint256 index = 0; index < _nftPairs.length; index++)
215     for (uint256 index = 0; index < _nftPairs.length; index++)
279     for (uint256 index = 0; index < _nfts.length; index++)
289     for (uint256 index = 0; index < _nftPairs.length; index++)
305     for (uint256 index = 0; index < _nftPairs.length; index++)

File: paraspace-core/contracts/protocol/pool/PoolCore.sol

634     for (uint256 i = 0; i < reservesListCount; i++)

File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493     for (uint256 index = 0; index < tokenIds.length; index++)

File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213     for (uint256 index = 0; index < totalBalance; index++)

File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207     for (uint256 index = 0; index < tokenData.length; index++)
280     for (uint256 index = 0; index < tokenIds.length; index++)

File: paraspace-core/contracts/protocol/tokenization/NToken.sol

106     for (uint256 index = 0; index < tokenIds.length; index++)
145     for (uint256 i = 0; i < ids.length; i++)

File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51      for (uint256 index = 0; index < tokenIds.length; index++)
97      for (uint256 index = 0; index < tokenIds.length; index++)

File: paraspace-core/contracts/ui/WPunkGateway.sol

82      for (uint256 i = 0; i < punkIndexes.length; i++)
109     for (uint256 i = 0; i < punkIndexes.length; i++)
113     for (uint256 i = 0; i < punkIndexes.length; i++)
136     for (uint256 i = 0; i < punkIndexes.length; i++)
174     for (uint256 i = 0; i < punkIndexes.length; i++)
```
