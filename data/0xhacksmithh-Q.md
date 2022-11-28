


## QA Report


| |Issue|Instances|
|-|:-|:-:|
| [LOW-1] | Should use recent updated and stable version of solidity | 6 |
| [LOW-2] | return Should be a named value rather than a expression | 2 |
| [LOW-3] | Absence of zero address checks during assigning immutable state variables values | 6 |


### [LOW-01] Should use recent updated and stable version of solidity
Recent version of solidity are more bug free. Consider to use recent stable version of solidity

*Instances (6)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol
File:   paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```

### [LOW-02] return Should be a named value rather than a expression

*Instances (2)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

129-137:  return  
            // FULL_OPEN || FULL_RESTRICTED
            (uint256(advancedOrder.parameters.orderType) == 0 ||
                uint256(advancedOrder.parameters.orderType) == 2) &&
            // NOT PARTIAL_FULFILLABLE
            advancedOrder.numerator == 1 &&
            advancedOrder.denominator == 1 &&
            // NO ZONE
            advancedOrder.extraData.length == 0;
```

```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

176-180: return  
            (((liquidityAmount0 + feeAmount0) * oracleData.token0Price) /
                10**oracleData.token0Decimal) +
            (((liquidityAmount1 + feeAmount1) * oracleData.token1Price) /
                10**oracleData.token1Decimal);
```

### [LOW-03] Absence of zero address checks during assigning immutable state variables values

*Instances (6)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

28:   UNISWAP_V3_FACTORY = IUniswapV3Factory(_factory);
29:   UNISWAP_V3_POSITION_MANAGER = INonfungiblePositionManager(_manager);
30:   ADDRESSES_PROVIDER = IPoolAddressesProvider(_addressProvider);
```

```solidity
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol

57:   ADDRESSES_PROVIDER = provider;  
58:   BASE_CURRENCY = baseCurrency;
```

```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

98:   address _admin,
```