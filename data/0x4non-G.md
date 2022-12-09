# Gas optimization

## Pack struct to save gas. Use `uint40` for representing timestamp.

Pack structs by putting data types that can be packed together next to each other.
As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, this saves gas when writing to storage ~20000 gas

### `contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol`

```diff
diff --git a/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol b/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol
index 903cf7f..d0d200c 100644
--- a/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol
+++ b/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol
@@ -19,16 +19,25 @@ import {
  *      be received by their respective recipient.
  */
 struct OrderComponents {
+    // slot 0
     address offerer;
+    // slot 1
     address zone;
+    uint40 startTime;
+    uint40 endTime;
+    // slot 2
     OfferItem[] offer;
+    // slot 3
     ConsiderationItem[] consideration;
+    // slot 4
     OrderType orderType;
-    uint256 startTime;
-    uint256 endTime;
+    // slot 5
     bytes32 zoneHash;
+    // slot 6
     uint256 salt;
+    // slot 7
     bytes32 conduitKey;
+    // slot 8
     uint256 counter;
 }
 
@@ -103,12 +112,12 @@ struct BasicOrderParameters {
     uint256 considerationAmount; // 0x64
     address payable offerer; // 0x84
     address zone; // 0xa4
+    uint40 startTime; // 0x144
+    uint40 endTime; // 0x164
     address offerToken; // 0xc4
     uint256 offerIdentifier; // 0xe4
     uint256 offerAmount; // 0x104
     BasicOrderType basicOrderType; // 0x124
-    uint256 startTime; // 0x144
-    uint256 endTime; // 0x164
     bytes32 zoneHash; // 0x184
     uint256 salt; // 0x1a4
     bytes32 offererConduitKey; // 0x1c4
@@ -141,8 +150,8 @@ struct OrderParameters {
     OfferItem[] offer; // 0x40
     ConsiderationItem[] consideration; // 0x60
     OrderType orderType; // 0x80
-    uint256 startTime; // 0xa0
-    uint256 endTime; // 0xc0
+    uint40 startTime; // 0xa0
+    uint40 endTime; // 0xc0
     bytes32 zoneHash; // 0xe0
     uint256 salt; // 0x100
     bytes32 conduitKey; // 0x120

```
### `contracts/dependencies/yoga-labs/ApeCoinStaking.sol`

```diff
diff --git a/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol b/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol
index b7904b9..73fd119 100644
--- a/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol
+++ b/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol
@@ -31,8 +31,8 @@ contract ApeCoinStaking is Ownable {
     /// @notice Pool rules valid for a given duration of time.
     /// @dev All TimeRange timestamp values must represent whole hours
     struct TimeRange {
-        uint256 startTimestampHour;
-        uint256 endTimestampHour;
+        uint40 startTimestampHour;
+        uint40 endTimestampHour;
         uint256 rewardsPerHour;
         uint256 capPerPosition;
     }
```

### `contracts/misc/NFTFloorOracle.sol`

Here i use `uint216` to represent `block.number`

```diff
diff --git a/paraspace-core/contracts/misc/NFTFloorOracle.sol b/paraspace-core/contracts/misc/NFTFloorOracle.sol
index b261d57..03f6bcf 100644
--- a/paraspace-core/contracts/misc/NFTFloorOracle.sol
+++ b/paraspace-core/contracts/misc/NFTFloorOracle.sol
@@ -24,9 +24,9 @@ struct PriceInformation {
     // last reported floor price(offchain twap)
     uint256 twap;
     // last updated blocknumber
-    uint256 updatedAt;
+    uint216 updatedAt;
     // last updated timestamp
-    uint256 updatedTimestamp;
+    uint40 updatedTimestamp;
 }
```

## Use immutable or constant for representing `WETH` address
On [PoolAddressesProvider.sol#L201-L203](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L201-L203) `WETH` should be declared as immutable. The `setWeth` function on [PoolAddressesProvider.sol#L235-L239](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235-L239) should be remove because WETH never changes, and the `WETH` should be setted in the constructor.

## When transfer WETH there is no need to use SafeTransfer, because WETH is fully ERC20 compliance
On;
- https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol#L196
- https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol#L494
- https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol#L506
- https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol#L517

Instead of `IERC20(WETH).safeTransfer(to, amount)` you could simple use `IERC20(WETH).transfer(to, amount)`, same goes for `IERC20(WETH).safeTransferFrom`, you coul just use `IERC20(WETH).transferFrom`

