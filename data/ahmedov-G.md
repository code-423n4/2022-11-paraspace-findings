# UNNECESSARY STORAGE READ ON EVENT EMITTING

paraspace-core/contracts/misc/NFTFloorOracle.sol: [381](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381)

reading from storage like ```assetPriceMapEntry.twap``` and ```assetPriceMapEntry.updatedAt``` consumes more gas.

Consider this (saves ~213 gas)

```solidity
        emit AssetDataSet(
            _asset,
            _twap,
            block.number
        );
```