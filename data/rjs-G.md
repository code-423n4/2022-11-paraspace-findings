The ParaSpace codebase is necessarily complex and does well to follow Solidity best practice. That being said, many areas of the ParaSpace protocol involve regular user-initiated transactions and therefore these hot paths should be considered for gas optimization.

Often gas optimization recommendations can be at odds with Solidity best practice, and developers must be sure to carefully balance deployment and user transaction costs against the risk of potentially introducing future issues if the assumptions behind gas optimized functions were to change.

While I would normally endeavour to provide estimates of gas improvements, there appears to be an issue with the ParaSpace-configured `hardhat-gas-reporter` that I was not able to resolve in the limited time available.

## Gas Optimization Findings

| ID  | Finding                                                                    | Instances |
| --- | -------------------------------------------------------------------------- | --------- |
|G-01     | Use custom, user-friendly errors (NOT identified in C4udit report)        |   115        |
|G-02     | Use `unchecked` blocks where overflow and underflow is not a valid concern   |  1         |
|G-03     | Use primitive types or a user-defined `type` instead of a single member `struct`    |  2         |
|G-04     | `x = x + y` is more efficient than `x += y;` | 24          |
|G-05     | Use `transferFrom` instead of `safeTransferFrom` where previously vetted                                        |  12         |
|G-06     | Use named `returns` when possible                                         |     -      |

Total: 154 instances over 6 issues

# Gas Optimization Findings

## \[G-01\] Use custom, user-friendly errors (NOT identified in C4udit report) (115 instances)

Deployment Gas saved:  ✔ (10,000s)
Transaction Gas saved:  ❌

Note that while the accompanying [C4udit report](https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba#GAS-6) did recommend the use of custom errors where strings were directly used, C4udit missed scenarios where indirect string constants were used i.e. in the `Errors` library.

That being said, while the ParaSpace errors in the `Errors` library are all stringified numeric codes and therefore have minor gas costs, I would nevertheless recommend using custom errors with more descriptive user-friendly errors, perhaps prefixed with codes to retain the benefits of error codes for front-end development.

Finally, I would recommend not mixing direct strings in `revert()` vs. strings inside an `Errors` library -- rather stick to a single convention i.e. custom errors.

*There are 115 instances of this issue:*

* paraspace-core/contracts/protocol/libraries/helpers/Errors.sol
```solidity
10:     string public constant CALLER_NOT_POOL_ADMIN = "1"; // 'The caller of the function is not a pool admin'
11:     string public constant CALLER_NOT_EMERGENCY_ADMIN = "2"; // 'The caller of the function is not an emergency admin'
12:     string public constant CALLER_NOT_POOL_OR_EMERGENCY_ADMIN = "3"; // 'The caller of the function is not a pool or emergency admin'
13:     string public constant CALLER_NOT_RISK_OR_POOL_ADMIN = "4"; // 'The caller of the function is not a risk or pool admin'
14:     string public constant CALLER_NOT_ASSET_LISTING_OR_POOL_ADMIN = "5"; // 'The caller of the function is not an asset listing or pool admin'
15:     string public constant CALLER_NOT_BRIDGE = "6"; // 'The caller of the function is not a bridge'
...
125:    string public constant APE_STAKING_AMOUNT_NON_ZERO = "130"; //ape staking amount should be zero when supply bayc/mayc.
```

## \[G-02\] Use `unchecked` blocks where overflow and underflow is not a valid concern (1 instance)

Deployment Gas saved:  ✔ (1,000s)
Transaction Gas saved:  ❌

The complexity of the ParaSpace protocol involves a number of loops that are executed on every major transaction type in the various `*Logic` contracts. In many of these loops, overflow and underflow is not a valid concern. The careful use of `unchecked` in these core contracts can reduce transaction fees for customers.

* paraspace-core/contracts/misc/NFTFloorOracle.sol
```solidity
283:         assets.push(_asset);
284:         assetFeederMap[_asset].index = uint8(assets.length - 1);
```

## \[G-03\] Use primitive types or a user-defined `type` instead of a single member `struct` (2 instances)

Deployment Gas saved:  ✔ (10,000s)
Transaction Gas saved:  ✔ (1,000s)

* paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
```solidity
26:     struct APEStakingParameter {
27:         uint256 unstakeIncentive;
28:     }
```

There is another instance in `ParaReentrancyGuard`, but this structure is only ever referenced via `SLOAD` and therefore changing this will only affect deployment gas:

* paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol

```solidity
39:     struct RGStorage {
40:         uint256 _status;
41:     }
```

## \[G-04\] `x = x + y` is more efficient than `x += y;` (24 instances)

Deployment Gas saved:  ✔ (10,000s)
Transaction Gas saved:  ❌

* paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
```solidity
149:         token0Amount += positionData.tokensOwed0;
150:         token1Amount += positionData.tokensOwed1;
...
211:             sum += getTokenPrice(tokenIds[index]);
```

* paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol
```solidity
169:                     vars.payableDebtByERC20Assets += vars
170:                         .userBalanceInBaseCurrency
171:                         .percentDiv(vars.liquidationBonus);
...
176:                     vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
... 12 other instances
```

* paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol
```solidity
397:             price += item.startAmount;
```

* paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol
```solidity
252:             reserve.accruedToTreasury += vars
253:                 .amountToMint
254:                 .rayDiv(reserveCache.nextLiquidityIndex)
255:                 .toUint128();
```

* paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol
```solidity
158:             vars.currentVariableBorrowRate +=
159:                 _variableRateSlope1 +
160:                 _variableRateSlope2.rayMul(excessBorrowUsageRatio);
...
162:             vars.currentVariableBorrowRate += _variableRateSlope1
163:                 .rayMul(vars.borrowUsageRatio)
164:                 .rayDiv(OPTIMAL_USAGE_RATIO);
```

* paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
```solidity
77:             amountToWithdraw += _nfts[index].amount;
...
166:            amountToWithdraw += _nftPairs[index].amount;
```

* paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
```solidity
215:             totalAmount += getTokenIdStakingAmount(
216:                 poolId,
217:                 _apeCoinStaking,
218:                 tokenId
219:             );
...
257:             apeStakedAmount += bakcStakedAmount;
```

## \[G-05\] Use `transferFrom` instead of `safeTransferFrom` where previously vetted (12 instances)

ParaSpace makes extensive use of `safeTransferFrom()` on various hot paths.`

`safeTransferFrom` builds on top of `transferFrom` by performing additional safety checks e.g. if the token address is a contract , whether it implements `onERC721Received` etc.

Since ParaSpace currently explicitly approves and vets all tokens and NFTs available in the protocol due to collateral pricing volatility concerns, it is perfectly reasonable to use `transferFrom` and avoid these redundant checks in the hot paths of the protocol.

Some examples:

* paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol
```solidity
846:         apeCoin.safeTransferFrom(msg.sender, address(this), _amount);
```

* paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
```solidity
157:                 bakcContract.safeTransferFrom(
158:                     msg.sender,
159:                     xTokenAddress,
160:                     _nftPairs[index].bakcTokenId
161:                 );
```

* paraspace-core/contracts/ui/WPunkGateway.sol

```solidity
110:             nWPunk.safeTransferFrom(msg.sender, address(this), punkIndexes[i]);
```

## \[G-06\] Use named `returns` when possible

Deployment Gas saved:  ✔ (1,000s)
Transaction Gas saved:  ❌

Named returns not only save deployment gas and improve code clarity, but reduce the number of lines of code, which will result in a cheaper audit next time around.

While much of the code does already use named returns e.g. `getAuctionData(uint256 tokenId)@paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol`, there are still a number of instances of multi-line functions where this language feature is not being used, e.g.:

* paraspace-core/contracts/misc/ParaSpaceOracle.sol
```solidity
115:     function getAssetPrice(address asset)
116:         public
117:         view
118:         override
119:         returns (uint256) // use returns (uint256 price) instead
120:     {
121:         if (asset == BASE_CURRENCY) {
122:             return BASE_CURRENCY_UNIT;
123:         }
124: 
125:         uint256 price = 0;
126:         IEACAggregatorProxy source = IEACAggregatorProxy(assetsSources[asset]);
127:         if (address(source) != address(0)) {
128:             price = uint256(source.latestAnswer());
129:         }
130:         if (price == 0 && address(_fallbackOracle) != address(0)) {
131:             price = _fallbackOracle.getAssetPrice(asset);
132:         }
133: 
134:         require(price != 0, Errors.ORACLE_PRICE_NOT_READY);
135:         return price; // and this can be deleted
136:     }
``````
