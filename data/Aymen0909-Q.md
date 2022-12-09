# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1      | Immutable state variables lack zero address checks | Low | 15 |
| 2      | Remove redundant checks  |NC | 2 |
| 3      | Remove redundant state variables | NC | 3 |
| 4      | Use scientific notation | NC | 1 |
| 5      | `2**<N>` should be re-written as `type(uint<N>).max`  | NC | 3 |


## Findings

### 1- Immutable state variables lack zero address checks  :

Constructors should check the values written in an immutable state variables(address) is not the address(0)

#### Risk : Low

#### Proof of Concept
Instances include:

File: paraspace-core/contracts/misc/ParaSpaceOracle.sol [Line 57](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L57)
```
ADDRESSES_PROVIDER = provider;
```

File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol [Line 28-30](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L28-L30)
```
UNISWAP_V3_FACTORY = IUniswapV3Factory(_factory);
UNISWAP_V3_POSITION_MANAGER = INonfungiblePositionManager(_manager);
ADDRESSES_PROVIDER = IPoolAddressesProvider(_addressProvider);
```

File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol [Line 52](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L52)
```
ADDRESSES_PROVIDER = provider;
```

File: paraspace-core/contracts/protocol/pool/PoolCore.sol [Line 65](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L65)
```
ADDRESSES_PROVIDER = provider;
```

File: paraspace-core/contracts/protocol/pool/PoolMarketplace.sol [Line 63](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L63)
```
ADDRESSES_PROVIDER = provider;
```

File: paraspace-core/contracts/protocol/pool/PoolParameters.sol [Line 90](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L90)
```
ADDRESSES_PROVIDER = provider;
```

File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol [Line 33](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L33)
```
_apeCoinStaking = ApeCoinStaking(apeCoinStaking);
```

File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol [Line 48-54](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L48-L54)
```
punk = _punk;
wpunk = _wpunk;
pool = _pool;

Punk = IPunks(punk);
WPunk = IWrappedPunks(wpunk);
Pool = IPool(pool);
```

#### Mitigation
Add non-zero address checks in the constructors for the instances aforementioned.

### 2- Remove redundant checks in functions :

#### Risk : NON CRITICAL

When a `public/external` function calls an `internal` function it is redundant to include the same modifier in both functions, so it's recommended to use the modifier only on one of the functions (either the internal or the public).

There are 2 instances of this issue:

The public function `removeAsset` calls the internal function `_removeAsset` and both contain the same modifier `onlyWhenAssetExisted(_asset)`, so this modifier should be removed from one of them.

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 148-154](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L148-L154)
```
function removeAsset(address _asset)
    external
    onlyRole(DEFAULT_ADMIN_ROLE)
    onlyWhenAssetExisted(_asset)
{
    _removeAsset(_asset);
}
```

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 296-298](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296-L298)
```
function _removeAsset(address _asset) internal onlyWhenAssetExisted(_asset)
```

The public function `removeFeeder` calls the internal function `_removeFeeder` and both contain the same modifier `onlyWhenFeederExisted(_feeder)`, so this modifier should be removed from one of them.

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 167-172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167-L172)
```
function removeFeeder(address _feeder)
    external
    onlyWhenFeederExisted(_feeder)
{
    _removeFeeder(_feeder);
}
```

File: paraspace-core/contracts/misc/NFTFloorOracle.sol [Line 326-328](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L326-L328)
```
function _removeFeeder(address _feeder) internal onlyWhenFeederExisted(_feeder)
```

### 3- Remove redundant state variables :

In the `WPunkGateway` contract the following state variables : `punk`, `wpunk` and `pool` are redundant and can be removed because the value of each variable can be derived from its interface : `Punk`, `WPunk` and `Pool` respectively and this by using `address(<interface>)`.

#### Risk : NON CRITICAL

#### Proof of Concept

In the `WPunkGateway` contract the following state variables are redundant and can be removed :

File: paraspace-core/contracts/ui/WPunkGateway.sol [Line 33-35](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L33-L35)
```
address public immutable punk;
address public immutable wpunk;
address public immutable pool;
```

Those variables are unnecessary because for each one there exist another state variable that store the same value : 

```
IPunks internal immutable Punk;
IWrappedPunks internal immutable WPunk;
IPool internal immutable Pool;
```

And for example if we need to get the `pool` address we can obtain it by using `address(Pool)` directly (and the same apply for the other variables).

#### Mitigation

The aforementioned state variables should be removed.

### 4- Use scientific notation :

When using multiples of 10 you shouldn't use decimal literals or exponentiation (e.g. 1000000, 10**18) but use scientific notation for better readability.

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol [Line 245](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245)

```
oracleData.token0Price * (10**18)
```

#### Mitigation

Use scientific notation for the instances aforementioned.

### 5- `2**<N>` should be re-written as `type(uint<N>).max` :

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

[2**96](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L247)

[2**96](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L259)

[2**96](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L271)

#### Mitigation
Replace the aforementioned statements for better readability.