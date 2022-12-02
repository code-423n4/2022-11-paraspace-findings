# [G-01] UNNECESSARY STORAGE READ ON EVENT EMITTING

paraspace-core/contracts/misc/NFTFloorOracle.sol: [381](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381)

reading from storage like ```assetPriceMapEntry.twap``` and ```assetPriceMapEntry.updatedAt``` consumes more gas.

## Suggest (saves ~213 gas)

```solidity
        emit AssetDataSet(
            _asset,
            _twap,
            block.number
        );
```

# [G-02] OPTIMIZE ```_quickSort``` FUNCTION WITH ```unchecked```

paraspace-core/contracts/misc/NFTFloorOracle.sol: [432](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L432)

## Suggest (saves ~3900 gas) Tested on array with length `10`

```solidity
function _quickSort(
        uint256[] memory arr,
        int256 left,
        int256 right
    ) internal pure {
        if (left == right) return; // check before assigning
        ...
        while (i <= j) {
            while (arr[uint256(i)] < pivot) { 
                unchecked {
                    i++;
                }
            }
            while (pivot < arr[uint256(j)]) {
                unchecked {
                    j--;
                }
            }
            if (i <= j) {
                ...
                unchecked {
                    i++;
                    j--;
                }
            }
        }
        ...
    }
```