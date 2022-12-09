**Overview**
Risk Rating | Number of issues
--- | ---
Low Risk | 9
Non-Critical Risk | 8

**Table of Contents**

- [1. Low Risk Issues](#1-low-risk-issues)
  - [1.1. `WETHGateway.sol`: Use upgradeable dependencies](#11-wethgatewaysol-use-upgradeable-dependencies)
  - [1.2. `assets[]` should be deleted with a swap and pop like `feeders[]` or `erc721Data.allTokens[]`](#12-assets-should-be-deleted-with-a-swap-and-pop-like-feeders-or-erc721dataalltokens)
  - [1.3. Known vulnerabilities exist in currently used Solidity version](#13-known-vulnerabilities-exist-in-currently-used-solidity-version)
  - [1.4. Known vulnerabilities exist in currently used `@openzeppelin/contracts` version](#14-known-vulnerabilities-exist-in-currently-used-openzeppelincontracts-version)
  - [1.5. Missing `ProxyUpdated` event](#15-missing-proxyupdated-event)
  - [1.6. Missing address(0) checks for immutable addresses](#16-missing-address0-checks-for-immutable-addresses)
  - [1.7. Prevent accidentally burning tokens](#17-prevent-accidentally-burning-tokens)
  - [1.8. All `initialize()` functions are front-runnable in the solution](#18-all-initialize-functions-are-front-runnable-in-the-solution)
  - [1.9. Use a 2-step ownership transfer pattern](#19-use-a-2-step-ownership-transfer-pattern)
- [2. Non-Critical Issues](#2-non-critical-issues)
  - [2.1. Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe](#21-replace-abiencodewithsignature-and-abiencodewithselector-with-abiencodecall-which-keeps-the-code-typotype-safe)
  - [2.2. The `nonReentrant` `modifier` should occur before all other modifiers](#22-the-nonreentrant-modifier-should-occur-before-all-other-modifiers)
  - [2.3. Typos](#23-typos)
  - [2.4. Open TODOS](#24-open-todos)
  - [2.5. Default Visibility](#25-default-visibility)
  - [2.6. Using `bytes.concat()` would be more idiomatic than `abi.encodePacked`](#26-using-bytesconcat-would-be-more-idiomatic-than-abiencodepacked)
  - [2.7. Adding a `return` statement when the function defines a named return variable, is redundant](#27-adding-a-return-statement-when-the-function-defines-a-named-return-variable-is-redundant)
  - [2.8. `public` functions not called by the contract should be declared `external` instead](#28-public-functions-not-called-by-the-contract-should-be-declared-external-instead)

# 1. Low Risk Issues

## 1.1. `WETHGateway.sol`: Use upgradeable dependencies

The wrong dependencies are imported and used in `WETHGateway.sol`:

```diff
File: WETHGateway.sol
4: import {OwnableUpgradeable} from "../dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol";
- 5: import {IERC20} from "../dependencies/openzeppelin/contracts/IERC20.sol";
...
- 14: import {ReentrancyGuard} from "../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";
+ 14: import {ReentrancyGuardUpgradeable} from "../dependencies/openzeppelin/upgradeability/ReentrancyGuardUpgradeable.sol";
- 15: import {SafeERC20} from "../dependencies/openzeppelin/contracts/SafeERC20.sol";
+ 15: import {SafeERC20} from "../dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol";
...
- 17: contract WETHGateway is ReentrancyGuard, IWETHGateway, OwnableUpgradeable {
+ 17: contract WETHGateway is ReentrancyGuardUpgradeable, IWETHGateway, OwnableUpgradeable {
...
- 20:     using SafeERC20 for IERC20;
+ 20:     using SafeERC20Upgradeable for IERC20Upgradeable;
...
39:     function initialize() external initializer {
40:         __Ownable_init();
+ 41:         __ReentrancyGuard_init_unchained(); 
41: 
42:         WETH.approve(pool, type(uint256).max);
43:     }
```

## 1.2. `assets[]` should be deleted with a swap and pop like `feeders[]` or `erc721Data.allTokens[]`

The best practice for deleting elements in arrays is using a "swap + pop" pattern. This is applied inconsistently:

- OK for `feeders[]`:

```solidity
File: NFTFloorOracle.sol
332:             feeders[feederIndex] = feeders[feeders.length - 1];
333:             feeders.pop();
```

- OK for `erc721Data.allTokens[]`:

```solidity
File: MintableERC721Logic.sol
560:         erc721Data.allTokens[tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
561:         erc721Data.allTokensIndex[lastTokenId] = tokenIndex; // Update the moved token's index
562: 
563:         // This also deletes the contents at the last position of the array
564:         delete erc721Data.allTokensIndex[tokenId];
565:         erc721Data.allTokens.pop();
```

- KO for `assets[]` (and there doesn't seem to be a good reason for keeping it this way, like index conservation or another reason):

```solidity
File: NFTFloorOracle.sol
296:     function _removeAsset(address _asset)
297:         internal
298:         onlyWhenAssetExisted(_asset)
299:     {
300:         uint8 assetIndex = assetFeederMap[_asset].index;
301:         delete assets[assetIndex];
302:         delete assetPriceMap[_asset];
303:         delete assetFeederMap[_asset];
304:         emit AssetRemoved(_asset);
305:     }
```

## 1.3. Known vulnerabilities exist in currently used Solidity version

The currently used version of Solidity (`0.8.10`) could be affected with the medium severity bug: [Head Overflow Bug in Calldata Tuple ABI-Reencoding](https://github.com/ethereum/solidity-blog/blob/master/_posts/2022-08-08-calldata-tuple-reencoding-head-overflow-bug.md) bug.
It would manifest its effects under the following 3 conditions:

> - The last component of the tuple is a (potentially nested) statically-sized calldata array with the most base type being either uint or bytes32. E.g. bytes32[10] or uint[2][2][2].
> - The tuple contains at least one dynamic component. E.g. bytes or a struct containing a dynamic array.
> - The code is using ABI coder v2, which is the default since Solidity 0.8.0.

Consider upgrading to Solidity `0.8.16`

## 1.4. Known vulnerabilities exist in currently used `@openzeppelin/contracts` version

As some [known vulnerabilities](https://snyk.io/test/npm/@openzeppelin/contracts/4.2.0) exist in the current `@openzeppelin/contracts` version, consider updating `package.json` with at least `@openzeppelin/contracts@4.7.3` here:

- <https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/package.json#L31-L32>:

```json
File: package.json
31:     "@openzeppel/contracts": "4.2.0",
32:     "@openzeppelin/contracts-upgradeable": "4.2.0",
```

The following vulnerabilities are present:

- [Improper Verification of Cryptographic Signature](https://security.snyk.io/vuln/SNYK-JS-OPENZEPPELINCONTRACTSUPGRADEABLE-2980280)

```solidity
paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol (out of scope):
  134:         address signer = ECDSA.recover(hash, v, r, s);
  480:         address signer = ECDSA.recover(hash, input.v, input.r, input.s);
  502:             orderSigner = ECDSA.recover(
```

- [Deserialization of Untrusted Data](https://security.snyk.io/vuln/SNYK-JS-OPENZEPPELINCONTRACTSUPGRADEABLE-2320177) (needs an external call from a user-provided address in `initialize()` with `initializer`)

```solidity
File: PoolConfigurator.sol
76:     function initialize(IPoolAddressesProvider provider) external initializer {
77:         _addressesProvider = provider;
78:         _pool = IPool(_addressesProvider.getPool());
79:     }
```

## 1.5. Missing `ProxyUpdated` event

It should be expected that the following function emits a `ProxyUpdated` event in the `else` clause:

```solidity
File: PoolAddressesProvider.sol
275: 
276:         if (proxyAddress == address(0)) {
277:             proxy = new InitializableImmutableAdminUpgradeabilityProxy(
278:                 address(this)
279:             );
280:             proxy.initialize(newAddress, params);
281:             _addresses[id] = proxyAddress = address(proxy);
282:             emit ProxyCreated(id, proxyAddress, newAddress);
283:         } else {
284:             proxy = InitializableImmutableAdminUpgradeabilityProxy(
285:                 payable(proxyAddress)
286:             );
287:             proxy.upgradeToAndCall(newAddress, params); //@audit-issue no event emitted
288:         }
````

Notice that such an event (`ParaProxyUpdated`) exists for ParaProxy:

```solidity
File: PoolAddressesProvider.sol
312:         if (proxyAddress == address(0)) {
313:             proxy = IParaProxy(address(new ParaProxy(address(this))));
314:             proxy.updateImplementation(implementationParams, _init, _calldata);
315:             _addresses[id] = proxyAddress = address(proxy);
316:             emit ParaProxyCreated(id, proxyAddress, implementationParams);
317:         } else {
318:             proxy = IParaProxy(payable(proxyAddress));
319: 
320:             proxy.updateImplementation(implementationParams, _init, _calldata);
321:             emit ParaProxyUpdated(id, proxyAddress, implementationParams);
322:         }
```

## 1.6. Missing address(0) checks for immutable addresses

It's a best practice to add an `address(0)` check for immutable addresses in the constructor. However, in the whole solution, this best practice is never applied.

List of the impacted immutable addresses :

```solidity
contracts/misc/ParaSpaceOracle.sol:22:    IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
contracts/misc/ParaSpaceOracle.sol:28:    address public immutable override BASE_CURRENCY;
contracts/misc/ParaSpaceOracle.sol:29:    uint256 public immutable override BASE_CURRENCY_UNIT;
contracts/misc/UniswapV3OracleWrapper.sol:18:    IUniswapV3Factory immutable UNISWAP_V3_FACTORY;
contracts/misc/UniswapV3OracleWrapper.sol:19:    INonfungiblePositionManager immutable UNISWAP_V3_POSITION_MANAGER;
contracts/misc/UniswapV3OracleWrapper.sol:20:    IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
contracts/ui/WPunkGateway.sol:28:    IPunks internal immutable Punk;
contracts/ui/WPunkGateway.sol:29:    IWrappedPunks internal immutable WPunk;
contracts/ui/WPunkGateway.sol:30:    IPool internal immutable Pool;
contracts/ui/WPunkGateway.sol:33:    address public immutable punk;
contracts/ui/WPunkGateway.sol:34:    address public immutable wpunk;
contracts/ui/WPunkGateway.sol:35:    address public immutable pool;
contracts/protocol/tokenization/NTokenApeStaking.sol:21:    ApeCoinStaking immutable _apeCoinStaking;
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:71:    IPoolAddressesProvider internal immutable _addressesProvider;
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:72:    IPool public immutable POOL;
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:73:    bool public immutable ATOMIC_PRICING;
contracts/protocol/pool/PoolApeStaking.sol:34:    IPoolAddressesProvider internal immutable ADDRESSES_PROVIDER;
contracts/protocol/pool/PoolCore.sol:54:    IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;
contracts/protocol/pool/PoolMarketplace.sol:55:    IPoolAddressesProvider internal immutable ADDRESSES_PROVIDER;
contracts/protocol/pool/PoolParameters.sol:48:    IPoolAddressesProvider internal immutable ADDRESSES_PROVIDER;
```

## 1.7. Prevent accidentally burning tokens

Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.

Consider adding a check to prevent accidentally burning tokens here:

```solidity
contracts/protocol/libraries/logic/PoolLogic.sol:89:            IERC20(token).safeTransfer(to, amountOrTokenId);
contracts/protocol/tokenization/NToken.sol:132:        IERC20(token).safeTransfer(to, amount);
```

## 1.8. All `initialize()` functions are front-runnable in the solution

Consider adding some access control to them or deploying atomically (couldn't find any instance of this):

```solidity
contracts/misc/NFTFloorOracle.sol:97:    function initialize(
contracts/ui/WPunkGateway.sol:57:    function initialize() external initializer {
contracts/protocol/tokenization/NTokenApeStaking.sol:36:    function initialize(
contracts/protocol/pool/PoolCore.sol:75:    function initialize(IPoolAddressesProvider provider)
contracts/protocol/tokenization/NToken.sol:52:    function initialize(
```

## 1.9. Use a 2-step ownership transfer pattern

Contracts inheriting from OpenZeppelin's libraries have the default `transferOwnership()` function (a one-step process). It's possible that the `onlyOwner` role mistakenly transfers ownership to a wrong address, resulting in a loss of the `onlyOwner` role.
Consider overriding the default `transferOwnership()` function to first nominate an address as the `pendingOwner` and implementing an `acceptOwnership()` function which is called by the `pendingOwner` to confirm the transfer.

```solidity
contracts/ui/WPunkGateway.sol:4:import {OwnableUpgradeable} from "../dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol";
contracts/protocol/configuration/PoolAddressesProvider.sol:4:import {Ownable} from "../../dependencies/openzeppelin/contracts/Ownable.sol";
```

# 2. Non-Critical Issues

## 2.1. Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe

When using `abi.encodeWithSignature`, it is possible to include a typo for the correct function signature.
When using `abi.encodeWithSignature` or `abi.encodeWithSelector`, it is also possible to provide parameters that are not of the correct type for the function.

To avoid these pitfalls, it would be best to use [`abi.encodeCall`](https://solidity-by-example.org/abi-encode/) instead.

- `abi.encodeWithSignature`'s occurrence:

```solidity
File: PoolAddressesProvider.sol
271:         bytes memory params = abi.encodeWithSignature(
272:             "initialize(address)",
273:             address(this)
274:         );
```

- `abi.encodeWithSelector`'s occurrences:

```solidity
File: MarketplaceLogic.sol
134:             abi.encodeWithSelector(
135:                 IMarketplace.matchAskWithTakerBid.selector,
136:                 params.marketplace.marketplace,
137:                 params.payload,
138:                 priceEth
139:             )
...
346:             abi.encodeWithSelector(
347:                 IMarketplace.matchBidWithTakerAsk.selector,
348:                 params.marketplace.marketplace,
349:                 params.payload
350:             )
```

## 2.2. The `nonReentrant` `modifier` should occur before all other modifiers

This is a best-practice to protect against re-entrancy in other modifiers.

At these places, the `nonReentrant` modifier always comes after the `onlyPool` modifier:

```solidity
contracts/protocol/tokenization/NToken.sol:82:    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
contracts/protocol/tokenization/NToken.sol:91:    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
contracts/protocol/tokenization/NToken.sol:123:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NToken.sol:204:        nonReentrant
contracts/protocol/tokenization/NToken.sol:219:        nonReentrant
contracts/protocol/tokenization/NTokenApeStaking.sol:106:    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
contracts/protocol/tokenization/NTokenApeStaking.sol:162:        nonReentrant
contracts/protocol/tokenization/NTokenBAYC.sol:29:        nonReentrant
contracts/protocol/tokenization/NTokenBAYC.sol:42:        nonReentrant
contracts/protocol/tokenization/NTokenBAYC.sol:55:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NTokenBAYC.sol:69:        nonReentrant
contracts/protocol/tokenization/NTokenBAYC.sol:85:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NTokenBAYC.sol:100:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NTokenMAYC.sol:29:        nonReentrant
contracts/protocol/tokenization/NTokenMAYC.sol:42:        nonReentrant
contracts/protocol/tokenization/NTokenMAYC.sol:55:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NTokenMAYC.sol:69:        nonReentrant
contracts/protocol/tokenization/NTokenMAYC.sol:85:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NTokenMAYC.sol:100:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/NTokenMoonBirds.sol:44:    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
contracts/protocol/tokenization/NTokenUniswapV3.sol:130:    ) external onlyPool nonReentrant {
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:462:    ) external virtual override onlyPool nonReentrant returns (bool) {
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:483:        nonReentrant
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:534:        nonReentrant
contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:545:        nonReentrant
```

## 2.3. Typos

```diff
- contracts/misc/NFTFloorOracle.sol:53:/// aggeregate prices which are not expired from different feeders, if number of valid/unexpired prices
+ contracts/misc/NFTFloorOracle.sol:53:/// aggregate prices which are not expired from different feeders, if number of valid/unexpired prices
- contracts/misc/NFTFloorOracle.sol:54:/// not enough, we do not aggeregate and just use previous price
+ contracts/misc/NFTFloorOracle.sol:54:/// not enough, we do not aggregate and just use previous price
- contracts/protocol/libraries/logic/AuctionLogic.sol:34:     * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
+ contracts/protocol/libraries/logic/AuctionLogic.sol:34:     * @notice Function to start auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
- contracts/protocol/libraries/logic/LiquidationLogic.sol:104:        //userDebt from liquadationReserve
+ contracts/protocol/libraries/logic/LiquidationLogic.sol:104:        //userDebt from liquidationReserve
- contracts/protocol/libraries/logic/MarketplaceLogic.sol:195:            // eg. The following example imagee that the `taker` owns only ETH and wants to
+ contracts/protocol/libraries/logic/MarketplaceLogic.sol:195:            // eg. The following example image that the `taker` owns only ETH and wants to
```

## 2.4. Open TODOS

The codebase still contains `TODO` statements. Consider resolving and removing the TODOs before deploying.

```solidity
contracts/misc/UniswapV3OracleWrapper.sol:238:        // TODO using bit shifting for the 2^96
contracts/misc/marketplaces/LooksRareAdapter.sol:59:            makerAsk.price, // TODO: take minPercentageToAsk into account
contracts/protocol/libraries/logic/MarketplaceLogic.sol:442:        // TODO: support PToken
```

## 2.5. Default Visibility

The following constants are using the default visibility:

```solidity
contracts/protocol/tokenization/NTokenApeStaking.sol:23:    bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:22:    uint256 constant BAYC_POOL_ID = 1;
contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:23:    uint256 constant MAYC_POOL_ID = 2;
contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:24:    uint256 constant BAKC_POOL_ID = 3;
contracts/protocol/pool/PoolStorage.sol:16:    bytes32 constant POOL_STORAGE_POSITION =
```

For readability, consider explicitly declaring them as `internal`. The full test-suite was run with success by adding that visibility to those constants

## 2.6. Using `bytes.concat()` would be more idiomatic than `abi.encodePacked`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

```solidity
contracts/misc/marketplaces/LooksRareAdapter.sol:2:pragma solidity 0.8.10;
contracts/misc/marketplaces/LooksRareAdapter.sol:63:        orderInfo.id = abi.encodePacked(makerAsk.r, makerAsk.s, makerAsk.v);
contracts/misc/marketplaces/LooksRareAdapter.sol:86:        bytes memory data = abi.encodePacked(selector, params);
contracts/misc/marketplaces/SeaportAdapter.sol:2:pragma solidity 0.8.10;
contracts/misc/marketplaces/SeaportAdapter.sol:99:        bytes memory data = abi.encodePacked(selector, params);
contracts/misc/marketplaces/SeaportAdapter.sol:115:        bytes memory data = abi.encodePacked(selector, params);
contracts/misc/marketplaces/X2Y2Adapter.sol:2:pragma solidity 0.8.10;
contracts/misc/marketplaces/X2Y2Adapter.sol:75:        orderInfo.id = abi.encodePacked(order.r, order.s, order.v);
contracts/misc/marketplaces/X2Y2Adapter.sol:94:        bytes memory data = abi.encodePacked(selector, params);
```

Note: There's an interesting discussion about why its usage is wished over `abi.encodePacked` here: [Introduce concat or bytes.concat solidity#10903](https://github.com/ethereum/solidity/issues/10903)

## 2.7. Adding a `return` statement when the function defines a named return variable, is redundant

While not consuming more gas: using both named returns and a return statement isn't necessary. Removing one of those can improve code clarity.

Affected code:

- File: NFTFloorOracle.sol

```solidity
240:         returns (uint256 price)
...
247:         return assetPriceMap[_asset].twap;
```

```solidity
256:         returns (uint256 timestamp)
257:     {
258:         return assetPriceMap[_asset].updatedTimestamp;
```

- File: PoolLogic.sol

```solidity
218:     ) external view returns (uint256 ltv, uint256 lt) {
...
226:         if (tokenType == XTokenType.NTokenUniswapV3) {
227:             return
228:                 GenericLogic.getLtvAndLTForUniswapV3(
229:                     ps._reserves,
230:                     asset,
231:                     tokenId,
232:                     collectionLtv,
233:                     collectionLT
234:                 );
235:         } else {
236:             return (collectionLtv, collectionLT);
237:         }
```

- File: PoolParameters.sol

```solidity
256:         returns (uint256 ltv, uint256 lt)
257:     {
258:         DataTypes.PoolStorage storage ps = poolStorage();
259:         return PoolLogic.executeGetAssetLtvAndLT(ps, asset, tokenId);
```

## 2.8. `public` functions not called by the contract should be declared `external` instead

```solidity
contracts/misc/NFTFloorOracle.sol:261:    function getFeederSize() public view returns (uint256) {
```
