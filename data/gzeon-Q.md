## Low

### MAX_VALID_LTV is set above 100%

It does not make sense to allow LTV above 100%, although it is currently not possible as a consequence of other invariant, it is recommended to set MAX_VALID_LTV to PERCENTAGE_FACTOR.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol#L48

```solidity
    uint256 internal constant MAX_VALID_LTV = 65535;
```

Currently, it is not possible to set ltv > PERCENTAGE_FACTOR because the following code make sure ltv <= liquidationThreshold <= PERCENTAGE_FACTOR

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol#L141-L160

```solidity
        require(ltv <= liquidationThreshold, Errors.INVALID_RESERVE_PARAMS);

        DataTypes.ReserveConfigurationMap memory currentConfig = _pool
            .getConfiguration(asset);

        if (liquidationThreshold != 0) {
            //liquidation bonus must be bigger than 100.00%, otherwise the liquidator would receive less
            //collateral than needed to cover the debt
            require(
                liquidationBonus >= PercentageMath.PERCENTAGE_FACTOR,
                Errors.INVALID_RESERVE_PARAMS
            );

            //if threshold * bonus is less than PERCENTAGE_FACTOR, it's guaranteed that at the moment
            //a loan is taken there is enough collateral available to cover the liquidation bonus
            require(
                liquidationThreshold.percentMul(liquidationBonus) <=
                    PercentageMath.PERCENTAGE_FACTOR,
                Errors.INVALID_RESERVE_PARAMS
            );
```

### Price can be equal to 0 while being valid

ParaSpaceOracle revert if both the primary and fallback oracle return price == 0. This might have undesired consequence, for instance, if an asset price literally drop to 0, the protocol will not be able to liquidate the said asset.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L134

```solidity
        require(price != 0, Errors.ORACLE_PRICE_NOT_READY);
```


## Non-Critical

### Confusing variable naming

For example, `liquidationProtocolFee` and `liquidationProtocolFeePercentage` are used interchangeably where it is unclear it represent the amount or %. 

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L694-L696

```solidity
        vars.liquidationProtocolFeePercentage = collateralReserve
            .configuration
            .getLiquidationProtocolFee();
```

### Unused error code 

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol#L15

```solidity
    string public constant CALLER_NOT_BRIDGE = "6"; // 'The caller of the function is not a bridge'
```

### Dangerous ReserveConfiguration design

ReserveConfiguration used a bit masking appoach to pack storage. While no issue is found at the moment it is a risky design. Consider using a packed struct instead.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol#L12-L13

### Unresolved TODOs

```
contracts/mocks/tokens/Moonbirds.sol:483:OpenSea's example code. TODO: it's likely that the above mapping can be changed
contracts/misc/UniswapV3OracleWrapper.sol:238:        // TODO using bit shifting for the 2^96
contracts/misc/marketplaces/LooksRareAdapter.sol:59:            makerAsk.price, // TODO: take minPercentageToAsk into account
contracts/ui/UiIncentiveDataProvider.sol:68:            // TODO: check that this is deployed correctly on contract and remove casting
contracts/protocol/libraries/logic/MarketplaceLogic.sol:442:        // TODO: support PToken
contracts/interfaces/INToken.sol:97:    // TODO are we using the Treasury at all? Can we remove?
```