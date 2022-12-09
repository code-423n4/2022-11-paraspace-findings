# [NC] NO SAME VALUE INPUT CONTROL

It's better to have a same value input control so if no changes it will just revert

```solidity
File: NFTFloorOracle.sol
343:     function _setConfig(uint128 _expirationPeriod, uint128 _maxPriceDeviation)
344:         internal
345:     {
346:         config.expirationPeriod = _expirationPeriod;
347:         config.maxPriceDeviation = _maxPriceDeviation;
348:         emit OracleConfigSet(_expirationPeriod, _maxPriceDeviation);
349:     }
```

```solidity
File: NFTFloorOracle.sol
183:     function setPause(address _asset, bool _flag)
184:         external
185:         onlyRole(DEFAULT_ADMIN_ROLE)
186:     {
187:         assetFeederMap[_asset].paused = _flag;
188:         emit AssetPaused(_asset, _flag);
189:     }
```

```solidity
File: ParaSpaceOracle.sol
109:     function _setFallbackOracle(address fallbackOracle) internal {
110:         _fallbackOracle = IPriceOracleGetter(fallbackOracle);
111:         emit FallbackOracleUpdated(fallbackOracle);
112:     }
```

```solidity
File: PoolAddressesProvider.sol
329:     function _setMarketId(string memory newMarketId) internal {
330:         string memory oldMarketId = _marketId;
331:         _marketId = newMarketId;
332:         emit MarketIdSet(oldMarketId, newMarketId);
333:     }
```

```solidity
File: PoolAddressesProvider.sol
70:     function setAddress(bytes32 id, address newAddress)
71:         external
72:         override
73:         onlyOwner
74:     {
75:         address oldAddress = _addresses[id];
76:         _addresses[id] = newAddress;
77:         emit AddressSet(id, oldAddress, newAddress);
78:     }
```

and other similar issue, on function `setPriceOracle()`, `setACLManager()`, `setACLAdmin()`, `setPriceOracleSentinel()`, `setProtocolDataProvider()`, `setWETH()`, `setMarketplace()`

## Recommended Mitigation Steps

For example, add code like this

```solidity
if(config.expirationPeriod == _expirationPeriod && config.maxPriceDeviation == _maxPriceDeviation) revert NO_CHANGES();

if(assetFeederMap[_asset].paused == _flag) revert NO_CHANGES();

if(_fallbackOracle == IPriceOracleGetter(fallbackOracle)) revert NO_CHANGES();

if(_marketId == newMarketId) revert NO_CHANGES();

if(_addresses[id] == newAddress) revert NO_CHANGES();

```

# [NC] EVENT SHOULD BE EMITTED IN SETTERS

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:

```solidity
File: PoolParameters.sol
151:     function setReserveInterestRateStrategyAddress(
152:         address asset,
153:         address rateStrategyAddress
154:     ) external virtual override onlyPoolConfigurator {
155:         DataTypes.PoolStorage storage ps = poolStorage();
156:
157:         require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
158:         require(
159:             ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
160:             Errors.ASSET_NOT_LISTED
161:         );
162:         ps._reserves[asset].interestRateStrategyAddress = rateStrategyAddress;
163:     }
```

```solidity
File: PoolParameters.sol
166:     function setReserveAuctionStrategyAddress(
167:         address asset,
168:         address auctionStrategyAddress
169:     ) external virtual override onlyPoolConfigurator {
170:         DataTypes.PoolStorage storage ps = poolStorage();
171:
172:         require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
173:         require(
174:             ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
175:             Errors.ASSET_NOT_LISTED
176:         );
177:         ps._reserves[asset].auctionStrategyAddress = auctionStrategyAddress;
178:     }
```

inside `PoolParameters.sol` other function with missing event emit `setConfiguration()`, `setAuctionRecoveryHealthFactor()`, `setAuctionValidityTime()`

# [NC] USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’

Most of files in scope already using named imports, but there are still contract using plain import file.

```solidity
File: NFTFloorOracle.sol
4: import "../dependencies/openzeppelin/contracts/AccessControl.sol";
5: import "../dependencies/openzeppelin/upgradeability/Initializable.sol";
6: import "./interfaces/INFTFloorOracle.sol";
```

```solidity
File: FlashClaimLogic.sol
10: import "../../../interfaces/INTokenApeStaking.sol";
```

```solidity
File: ValidationLogic.sol
30: import "../../../interfaces/INTokenApeStaking.sol";
```

```solidity
File: PoolApeStaking.sol
04: import "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
05: import "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
...
07: import "../../interfaces/IPoolApeStaking.sol";
08: import "../../interfaces/IPToken.sol";
09: import "../../dependencies/yoga-labs/ApeCoinStaking.sol";
10: import "../../interfaces/IXTokenType.sol";
11: import "../../interfaces/INTokenApeStaking.sol";
...
20: import "../libraries/logic/BorrowLogic.sol";
21: import "../libraries/logic/SupplyLogic.sol";
```

```solidity
File: NTokenApeStaking.sol
12: import "../../interfaces/INTokenApeStaking.sol";
```

```solidity
File: MintableERC721Logic.sol
7: import "../../../interfaces/IRewardController.sol";
8: import "../../libraries/types/DataTypes.sol";
9: import "../../../interfaces/IPool.sol";
```

```solidity
File: ApeStakingLogic.sol
07: import "../../../interfaces/IPool.sol";
11: import "./MintableERC721Logic.sol";
```

# [NC] USE LATEST OPEN ZEPPELIN CONTRACTS

Project current version of @openzeppelin/contracts (and @openzeppelin/contracts-upgradeable) is 4.2.0 and latest version is 4.8.0

# [L] ADD TWO STEP TRANSFER OWNERSHIP PATTERN

The project using Openzeppelin `Ownable` contract for ownership. The implementation of openzeppelin ownable contract doesn't handle two step verification owner transfer.

All contracts are inherited from OpenZeppelin’s Ownable and OwnableUpgradable contract which enables the onlyOwner role to transfer ownership to another address. It’s possible that the onlyOwner role mistakenly transfers ownership to the wrong address, resulting in a loss of the onlyOwner role.

If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier. Lack of two-step procedure for critical operations leaves them error-prone if the address is incorrect, the new address will take on the functionality of the new role immediately

## Recommended Mitigation Steps

Implement zero address check and Consider implementing a two step process where the owner nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.

# [NC] USE A MORE RECENT VERSION OF SOLIDITY

All scoped contracts using `pragma solidity 0.8.10;` released on 2021-11-09 (more than 1 year ago), and current latest version is `v0.8.17`
https://github.com/ethereum/solidity/blob/develop/Changelog.md

[https://github.com/ethereum/solidity/blob/develop/Changelog.md#0813-2022-03-16]
Use a solidity version of at least `0.8.13` to get the ability to use `using for` with a list of free functions

- `using M for Type;` is allowed at file level and M can now also be a brace-enclosed list of free functions or library functions.
- `using ... for T global;` is allowed at file level where the user-defined type T has been defined, resulting in the effect of the statement being available everywhere T is available.

# [N] CONSTANTS SHOULD BE DEFINED RATHER THAN USING MAGIC NUMBERS

```solidity
File: NTokenUniswapV3.sol
34:         _ERC721Data.balanceLimit = 30;
```

```solidity
File: NToken.sol
176:         require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);
```
