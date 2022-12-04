# [G-01] UNNECESSARY STORAGE READ ON EVENT EMITTING

paraspace-core/contracts/misc/NFTFloorOracle.sol: [381](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381)

reading from storage like ```assetPriceMapEntry.twap``` and ```assetPriceMapEntry.updatedAt``` consumes more gas.

## Suggest (saves ~213 gas)

```diff
@@ -380,8 +380,8 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
         assetPriceMapEntry.updatedTimestamp = block.timestamp;
         emit AssetDataSet(
             _asset,
-            assetPriceMapEntry.twap,
-            assetPriceMapEntry.updatedAt
+            _twap,
+            block.number
         );
     }
```

# [G-02] OPTIMIZE ```_quickSort``` FUNCTION WITH ```unchecked``` blocks

paraspace-core/contracts/misc/NFTFloorOracle.sol: [432](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L432)

## Suggest (saves ~4295 gas) Tested on array with length `10`

```diff
diff --git a/paraspace-core/contracts/misc/NFTFloorOracle.sol b/paraspace-core/contracts/misc/NFTFloorOracle.sol
index b261d57..46958f5 100644
--- a/paraspace-core/contracts/misc/NFTFloorOracle.sol
+++ b/paraspace-core/contracts/misc/NFTFloorOracle.sol
@@ -434,20 +434,26 @@ contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
         int256 left,
         int256 right
     ) internal pure {
+        if (left == right) return; // check before assigning to save extra gas
         int256 i = left;
         int256 j = right;
-        if (i == j) return;
         uint256 pivot = arr[uint256(left + (right - left) / 2)];
         while (i <= j) {
-            while (arr[uint256(i)] < pivot) i++;
-            while (pivot < arr[uint256(j)]) j--;
+            while (arr[uint256(i)] < pivot) {
+               unchecked{ ++i; }
+            }
+            while (pivot < arr[uint256(j)]) {
+                unchecked { --j; }
+            }
             if (i <= j) {
                 (arr[uint256(i)], arr[uint256(j)]) = (
                     arr[uint256(j)],
                     arr[uint256(i)]
                 );
-                i++;
-                j--;
+                unchecked {
+                    ++i;
+                    --j;
+                }
             }
         }
         if (left < j) _quickSort(arr, left, j);
```

# [G-03] DOUBLE CHECK IN ```removeFeeder``` FOR ```feeder``` EXISTANCE

paraspace-core/contracts/misc/NFTFloorOracle.sol: [169](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L169)
paraspace-core/contracts/misc/NFTFloorOracle.sol: [328](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L328)

## Suggest 

```diff
     function removeFeeder(address _feeder)
         external
-        onlyWhenFeederExisted(_feeder)
     {
         _removeFeeder(_feeder);
     }
```