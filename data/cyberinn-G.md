## G-01 internal functions only called once can be inlined

### Impact

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

### Findings

[paraspace-core/contracts/misc/NFTFloorOracle.sol#L265](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/NFTFloorOracle.sol#L265)

[paraspace-core/contracts/misc/NFTFloorOracle.sol#L388](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/NFTFloorOracle.sol#L388)

```
265:    function _whenNotPaused(address _asset) internal view {
...
388:    function _addRawValue(address _asset, uint256 _twap) internal {
```

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69)

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L76](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L76)

```
69:    function _onlyPoolConfigurator() internal view virtual {
...
76:    function _onlyPoolAdmin() internal view virtual {
```

## G-02 Splitting require() statements that use &&

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-220](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-220)

```
216:    require(
217:                value > MIN_AUCTION_HEALTH_FACTOR &&
218:                    value <= MAX_AUCTION_HEALTH_FACTOR,
219:                Errors.INVALID_AMOUNT
220:    );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L1211](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L1211)

```
546:    require(
547:                vars.collateralReserveActive && vars.principalReserveActive,
548:                Errors.RESERVE_INACTIVE
549:    );
550:    require(
551:                !vars.collateralReservePaused && !vars.principalReservePaused,
552:                Errors.RESERVE_PAUSED
553:    );
...
636:    require(
637:                vars.collateralReserveActive && vars.principalReserveActive,
638:                Errors.RESERVE_INACTIVE
639:   );
640:    require(
641:                !vars.collateralReservePaused && !vars.principalReservePaused,
642:                Errors.RESERVE_PAUSED
643:    );
...
672:    require(
673:                params.maxLiquidationAmount >= params.actualLiquidationAmount &&
674:                    (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
675:                Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
676:    );
...
1196:    require(
1197:                    vars.token0IsActive && vars.token1IsActive,
1198:                    Errors.RESERVE_INACTIVE
1199:    );
...
1202:    require(
1203:                    !vars.token0IsPaused && !vars.token1IsPaused,
1204:                    Errors.RESERVE_PAUSED
1205:    );
...
1208:    require(
1209:                    !vars.token0IsFrozen && !vars.token1IsFrozen,
1210:                    Errors.RESERVE_FROZEN
1211:     );
```





