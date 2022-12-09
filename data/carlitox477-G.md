# Split require with && operator to save gas
Instead of using the ```&&``` operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 3 GAS per ```&&```
```diff
// SeaportAdapter.getAskOrderInfo
-    require(
-        // NOT criteria based and must be basic order
-        resolvers.length == 0 && isBasicOrder(advancedOrder),
-        Errors.INVALID_MARKETPLACE_ORDER
-    );
+    require(resolvers.length == 0, Errors.INVALID_MARKETPLACE_ORDER);
+    require(isBasicOrder(advancedOrder), Errors.INVALID_MARKETPLACE_ORDER);
```

```diff
//getBidOrderInfo
- require(
    // NOT criteria based and must be basic order
-    advancedOrders.length == 2 &&
-        isBasicOrder(advancedOrders[0]) &&
-        isBasicOrder(advancedOrders[1]),
-    Errors.INVALID_MARKETPLACE_ORDER
-);
+   require(advancedOrders.length == 2, Errors.INVALID_MARKETPLACE_ORDER);
+   require(isBasicOrder(advancedOrders[0]) , Errors.INVALID_MARKETPLACE_ORDER);
+   require(isBasicOrder(advancedOrders[1]) , Errors.INVALID_MARKETPLACE_ORDER);
```

```diff
// MarketplaceLogic.executeBatchAcceptBidWithCredit
-   require(
-       marketplaceIds.length == payloads.length &&
-           payloads.length == credits.length,
-       Errors.INCONSISTENT_PARAMS_LENGTH
-   );
+   require(marketplaceIds.length == payloads.length, Errors.INCONSISTENT_PARAMS_LENGTH)
+   require(payloads.length == credits.length, Errors.INCONSISTENT_PARAMS_LENGTH)
```

```diff
// ValidationLogic.validateLiquidateERC20
-    require(
-        vars.collateralReserveActive && vars.principalReserveActive,
-        Errors.RESERVE_INACTIVE
-    );
+    require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
+    require(vars.principalReserveActive, Errors.RESERVE_INACTIVE);

-    require(
-        !vars.collateralReservePaused && !vars.principalReservePaused,
-        Errors.RESERVE_PAUSED
-    );
+    require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
+    require(!vars.principalReservePaused, Errors.RESERVE_PAUSED);
```

```diff
// ValidationLogic.validateLiquidateERC721
-    require(
-        vars.collateralReserveActive && vars.principalReserveActive,
-        Errors.RESERVE_INACTIVE
-    );
+    require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
+    require(vars.principalReserveActive, Errors.RESERVE_INACTIVE);

-    require(
-        !vars.collateralReservePaused && !vars.principalReservePaused,
-        Errors.RESERVE_PAUSED
-    );
+    require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
+    require(!vars.principalReservePaused, Errors.RESERVE_PAUSED);
```

```diff
// ValidationLogic.validateForUniswapV3
    if (checkActive) {
-        require(
-            vars.token0IsActive && vars.token1IsActive,
-            Errors.RESERVE_INACTIVE
-        );
+       require(vars.token0IsActive, Errors.RESERVE_INACTIVE); 
+       require(vars.token1IsActive, Errors.RESERVE_INACTIVE);
    }
    if (checkNotPaused) {
-        require(
-            !vars.token0IsPaused && !vars.token1IsPaused,
-            Errors.RESERVE_PAUSED
-        );
+        require(!vars.token0IsPaused, Errors.RESERVE_PAUSED); 
+        require(!vars.token1IsPaused, Errors.RESERVE_PAUSED);
    }
    if (checkNotFrozen) {
-        require(
-            !vars.token0IsFrozen && !vars.token1IsFrozen,
-            Errors.RESERVE_FROZEN
-        );
+        require(!vars.token0IsFrozen, Errors.RESERVE_FROZEN);
+        require(!vars.token1IsFrozen, Errors.RESERVE_FROZEN);
    }
```

```diff
// PoolParameters.setAuctionRecoveryHealthFactor
-   require(
-       value > MIN_AUCTION_HEALTH_FACTOR &&
-           value <= MAX_AUCTION_HEALTH_FACTOR,
-       Errors.INVALID_AMOUNT
-    );
+   require(value > MIN_AUCTION_HEALTH_FACTOR, Errors.INVALID_AMOUNT)
+   require(value <= MAX_AUCTION_HEALTH_FACTOR,Errors.INVALID_AMOUNT);
```

[POC](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-11-splitting-require-statements-that-use--saves-gas)

# Use storage pointer to save gas
```diff
// NFTFloorPriceOracle._checkValidity
-   PriceInformation memory assetPriceMapEntry = assetPriceMap[_asset];
PriceInformation storage assetPriceMapEntry = assetPriceMap[_asset];
```

```diff
// GenericLogic.getLtvAndLTForUniswapV3
-   DataTypes.ReserveConfigurationMap memory token0Configs = reservesData[
+   DataTypes.ReserveConfigurationMap storage token0Configs = reservesData[
        token0
    ].configuration;
-   DataTypes.ReserveConfigurationMap memory token1Configs = reservesData[
+   DataTypes.ReserveConfigurationMap storage token1Configs = reservesData[
        token1
    ].configuration;
```

# NFTFloorOracle: PriceInformation struct can use uint128 to save gas.
2^40 as a unix timestamp is ~35.000 yerar in the future, is more than enough to use it to save a timestamp and a block number (at least in ETH). So we can declear
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
In order to save one slot of memory whenever we use PriceInformation. [More information](https://twitter.com/PaulRBerg/status/1591832937179250693)

# NFTFloorOracle#setPrice dataValidity should move it declaration
This in order to save gas for a meaningless assignation. These lines should be changed for
```diff
-   bool dataValidity = false;
    if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) {
        _finalizePrice(_asset, _twap);
        return;
        }
-   dataValidity = _checkValidity(_asset, _twap);
+   bool dataValidity = _checkValidity(_asset, _twap);
```

# NFTFloorOracle#setMultiplePrices can cache _assets.length to avoid one access
```diff
+   uint256 _assetsLen = _assets.length;
    require(
-        _assets.length == _twaps.length,
+        _assetsLen == _twaps.length,
        "NFTOracle: Tokens and price length differ"
    );
-    for (uint256 i = 0; i < _assets.length; i++) {
+    for (uint256 i; i < _assetsLen; unchecked{++i}) {
        setPrice(_assets[i], _twaps[i]);
    }
```
Note: C4 audit indicate this to be considered just in the loop, this issue indicates that it should be used before the require


# NFTFloorOracle#_whenNotPaused can avoid assignation operation by just using storage value
```diff
    function _whenNotPaused(address _asset) internal view {
-       bool _paused = assetFeederMap[_asset].paused;
        require(!assetFeederMap[_asset].paused, "NFTOracle: nft price feed paused");
    }
```

# ValidationLogic#validateSupply can avoid if else check by just using else
Given that DataTypes.AssetType can currently have just 2 types, once the first type is discarted, the one left is the only option
```diff
    // ValidationLogic.validateSupply function
-   } else if (assetType == DataTypes.AssetType.ERC721) {
+   } else{
+       // assetType == DataTypes.AssetType.ERC721 is the only chance giving DataTypes.AssetType has just 2 values
        require(
            supplyCap == 0 ||
                (INToken(reserveCache.xTokenAddress).totalSupply() +
                    amount <=
                    supplyCap),
            Errors.SUPPLY_CAP_EXCEEDED
        );
    }
```
