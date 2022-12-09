# ADD UNCHECKED {} FOR SUBTRACTIONS WHERE THE OPERANDS CANNOT UNDERFLOW

```solidity
File: NFTFloorOracle.sol
403:         uint256 currentBlock = block.number;
404:         //first time just use the feeding value
405:         if (assetPriceMap[_asset].twap == 0) {
406:             return (true, _twap);
407:         }
408:         //use memory here so allocate with maximum length
409:         uint256 feederSize = feeders.length;
410:         uint256[] memory validPriceList = new uint256[](feederSize);
411:         uint256 validNum = 0;
412:         //aggeregate with price from all feeders
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

The `diffBlock` can not undeflow since `priceInfo.updatedAt` will always be lower than `block.number`, moreover this will save quite gas because it's inside loop and if `feederSize` is large.

# USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS

If needed, the values can be read from the verified contract source code

```solidity
File: LiquidationLogic.sol
57:     uint256 public constant MAX_LIQUIDATION_CLOSE_FACTOR = 1e4;
64:     uint256 public constant CLOSE_FACTOR_HF_THRESHOLD = 0.95e18;
```

```solidity
File: ValidationLogic.sol
45:     uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;
49:     uint256 public constant MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD =
50:         0.95e18;
56:     uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;
```

```solidity
File: PoolCore.sol
53:     uint256 public constant POOL_REVISION = 1;
```

```solidity
File: NToken.sol
32:     uint256 public constant NTOKEN_REVISION = 0x1;
```

# WRONG USE OF THE MEMORY KEYWORD FOR A STRUCT

When copying a state struct in memory, there are as many SLOADs and MSTOREs as there are slots. When reading the whole struct multiple times is not needed, it’s better to actually only read the relevant field(s). When only some of the fields are read several times, these particular values should be cached instead of the whole state struct.

Here, the storage keyword for `PriceInformation` should be used instead of memory:

```solidity
File: NFTFloorOracle.sol
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

# SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

If project compiled using the Optimizer at 200, instead of using the `&&` operator in a single require statement to check multiple conditions, Consider using multiple require statements with 1 condition per require statement.

Please note that this might not hold true at a higher number of runs for the Optimizer (10k). However, it indeed is true at 200.

```solidity
File: SeaportAdapter.sol
41:         require(
42:             // NOT criteria based and must be basic order
43:             resolvers.length == 0 && isBasicOrder(advancedOrder),
44:             Errors.INVALID_MARKETPLACE_ORDER
45:         );
```

```solidity
File: MarketplaceLogic.sol
388:             require(
389:                 item.itemType == ItemType.ERC20 ||
390:                     (vars.isETH && item.itemType == ItemType.NATIVE),
391:                 Errors.INVALID_ASSET_TYPE
392:             );
```

```solidity
File: ValidationLogic.sol
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
File: ValidationLogic.sol
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

# FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

```solidity
File: WPunkGateway.sol
202:     function emergencyERC721TokenTransfer(
203:         address token,
204:         uint256 tokenId,
205:         address to
206:     ) external onlyOwner {
207:         IERC721(token).safeTransferFrom(address(this), to, tokenId);
208:         emit EmergencyERC721TokenTransfer(token, tokenId, to);
209:     }
```

the scoped contract just only have one instance, but other contract out of scope contains much more this gas saving tips.

# AMOUNTS SHOULD BE CHECKED FOR 0 BEFORE CALLING A TRANSFER

Checking non-zero transfer values can avoid an expensive external call and save gas.

Consider adding a non-zero-value check here:

```solidity
File: BorrowLogic.sol
190:             IERC20(params.asset).safeTransferFrom(
191:                 msg.sender,
192:                 reserveCache.xTokenAddress,
193:                 paybackAmount
194:             );
```

```solidity
File: SupplyLogic.sol
104:         IERC20(params.asset).safeTransferFrom(
105:             params.payer,
106:             reserveCache.xTokenAddress,
107:             params.amount
108:         );
```

```solidity
File: LiquidationLogic.sol
531:         IERC20(params.liquidationAsset).safeTransferFrom(
532:             vars.payer,
533:             vars.liquidationAssetReserveCache.xTokenAddress,
534:             vars.actualLiquidationAmount
535:         );
```

```solidity
File: MarketplaceLogic.sol
402:             IERC20(vars.creditToken).safeTransferFrom(
403:                 params.orderInfo.taker,
404:                 address(this),
405:                 downpayment
406:             );
```
