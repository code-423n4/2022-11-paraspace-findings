
### [LOW-01] return value Should be a named value rather than a expression

*Instances (2)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L129-L137
```

```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L176-L180
```

### [LOW-02] Absence of zero address checks during assigning immutable state variables values

*Instances (12)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L28
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L29
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L30
```

```solidity
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L57
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L58
```

```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L58
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L51-L53

```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolCore.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L64-L66
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L62-L63
```
```solidity
File:   paraspace-core/contracts/ui/WPunkGateway.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L52
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L53
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L54
```

### [LOW-03] Transfer of ownership should be a 2 step process 

*Instances (1)*:
```solidity
File:   paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L47
```

### [LOW-04]  Left the default message hash
For the in-scope code MESSAGE is left to the default value

    ```bytes32 constant POOL_STORAGE_POSITION =
        bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);```

This could cause the signature to be replayable in other applications that use the same message.

Mitigation Steps
Add the proper message, most likely a TOS acknowledgement or a ipfs hash to a document.

*Instances (3)*:
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolStorage.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16-L17
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NToken.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L32
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L24-L27

```

### [LOW-05] Instead of magic number try to use constant state variables
  
*Instances (1)*:
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L30
```


### [LOW-06] Should use recent updated and stable version of solidity
Recent version of solidity are more bug free. Consider to use recent stable version of solidity

*Instances (32)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol
File:   paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

....
....
all contracts should be updated

```
