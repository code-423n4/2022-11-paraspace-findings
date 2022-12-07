## G-01 - Avoid using >= or <= where possible to save gas.  Extra opcode used for combination comparators.

Use of >=

```
paraspace-core/contracts/misc/NFTFloorOracle.sol:331:        if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {

paraspace-core/contracts/misc/NFTFloorOracle.sol:370:        if (priceDeviation >= config.maxPriceDeviation) {

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:324:            if (positionData.currentTick >= positionData.tickLower) {

paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol:273:            vars.payableDebtByERC20Assets >= vars.totalDebtInBaseCurrency)

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:412:            require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:526:            msg.value == 0 || msg.value >= params.actualLiquidationAmount,

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:673:            params.maxLiquidationAmount >= params.actualLiquidationAmount &&

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:674:                (msg.value == 0 || msg.value >= params.maxLiquidationAmount),

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:733:            healthFactor >= HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

paraspace-core/contracts/protocol/tokenization/NToken.sol:176:        require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);

```

Use of <=

```
paraspace-core/contracts/misc/NFTFloorOracle.sol:244:            (block.number - updatedAt) <= config.expirationPeriod,

paraspace-core/contracts/misc/NFTFloorOracle.sol:419:                if (diffBlock <= config.expirationPeriod) {

paraspace-core/contracts/misc/NFTFloorOracle.sol:441:        while (i <= j) {

paraspace-core/contracts/misc/NFTFloorOracle.sol:444:            if (i <= j) {

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:96:                        .rayMul(reserveCache.nextLiquidityIndex) + amount) <=

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:105:                        amount <=

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:163:            amount <= userBalance,

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:307:                    vars.totalDebt <= vars.borrowCap * vars.assetUnit,

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:358:            vars.collateralNeededInBaseCurrency <=

paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol:115:        if (ticks <= ticksMinExp) {

paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol:130:        if (ticks <= ticksMin) {

paraspace-core/contracts/protocol/pool/PoolParameters.sol:218:                value <= MAX_AUCTION_HEALTH_FACTOR,

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:410:                balanceLimit == 0 || balance <= balanceLimit,
```