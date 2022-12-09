# REQUIRE() THAT USE && CAN BE SPLIT TO SAVES GAS

Instead of using the `&&` operator in a single require statement to check multiple conditions, it is better to use multiple require statements with 1 condition per require statement.
By default / commonly we use 200 runs for the Optimizer in compilation, but this might not be true at a higher number of runs for the Optimizer (10k) in compilation.

MarketplaceLogic.sol

```solidity
388:             require(
389:                 item.itemType == ItemType.ERC20 ||
390:                     (vars.isETH && item.itemType == ItemType.NATIVE),
391:                 Errors.INVALID_ASSET_TYPE
392:             );
```

ValidationLogic.sol

```solidity
546:         require(
547:             vars.collateralReserveActive && vars.principalReserveActive,
548:             Errors.RESERVE_INACTIVE
549:         );
550:         require(
551:             !vars.collateralReservePaused && !vars.principalReservePaused,
552:             Errors.RESERVE_PAUSED
553:         );
...
636:         require(
637:             vars.collateralReserveActive && vars.principalReserveActive,
638:             Errors.RESERVE_INACTIVE
639:         );
640:         require(
641:             !vars.collateralReservePaused && !vars.principalReservePaused,
642:             Errors.RESERVE_PAUSED
643:         );
...
1195:         if (checkActive) {
1196:             require(
1197:                 vars.token0IsActive && vars.token1IsActive,
1198:                 Errors.RESERVE_INACTIVE
1199:             );
1200:         }
1201:         if (checkNotPaused) {
1202:             require(
1203:                 !vars.token0IsPaused && !vars.token1IsPaused,
1204:                 Errors.RESERVE_PAUSED
1205:             );
1206:         }
1207:         if (checkNotFrozen) {
1208:             require(
1209:                 !vars.token0IsFrozen && !vars.token1IsFrozen,
1210:                 Errors.RESERVE_FROZEN
1211:             );
1212:         }
```

# NON-ZERO transfer CHECK

Checking non-zero transfer values can avoid an expensive external call and save gas.

Suggesting to add a non-zero-value check

MarketplaceLogic.sol

```solidity
402:             IERC20(vars.creditToken).safeTransferFrom(
403:                 params.orderInfo.taker,
404:                 address(this),
405:                 downpayment
406:             );
```

LiquidationLogic.sol

```solidity
531:         IERC20(params.liquidationAsset).safeTransferFrom(
532:             vars.payer,
533:             vars.liquidationAssetReserveCache.xTokenAddress,
534:             vars.actualLiquidationAmount
535:         );
```

BorrowLogic.sol

```solidity
190:             IERC20(params.asset).safeTransferFrom(
191:                 msg.sender,
192:                 reserveCache.xTokenAddress,
193:                 paybackAmount
194:             );
```

SupplyLogic.sol

```solidity
104:         IERC20(params.asset).safeTransferFrom(
105:             params.payer,
106:             reserveCache.xTokenAddress,
107:             params.amount
108:         );
```

# SUBSTRACTION WITH VALUE NEVER BE UNDERFLOW CAN USE UNCHECKED {}

NFTFloorOracle.sol

```solidity
403:         uint256 currentBlock = block.number;
...
413:         for (uint256 i = 0; i < feederSize; i++) {
414:             PriceInformation memory priceInfo = feederRegistrar.feederPrice[
415:                 feeders[i]
416:             ];
417:             if (priceInfo.updatedAt > 0) {
418:                 uint256 diffBlock = currentBlock - priceInfo.updatedAt;
419:                 if (diffBlock <= config.expirationPeriod) {
420:                     validPriceList[validNum] = priceInfo.twap;
421:                     validNum++;
422:                 }
423:             }
424:         }
```

If we check `diffBlock` will never be underflow because `priceInfo.updatedAt` will always be lower than `block.number`
