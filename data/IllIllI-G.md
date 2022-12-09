## Summary

### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|-|:-|:-:|:-:|
| [G&#x2011;01] | Save gas by checking against default WETH address | 1 | 2200 |
| [G&#x2011;02] | Save gas by batching `NToken` operations | 2 | 200 |
| [G&#x2011;03] | Using `storage` instead of `memory` for structs/arrays saves gas | 5 | 21000 |
| [G&#x2011;04] | Multiple accesses of a mapping/array should use a local variable cache | 4 | 168 |
| [G&#x2011;05] | The result of function calls should be cached rather than re-calling the function | 2 | - |
| [G&#x2011;06] | `internal` functions only called once can be inlined to save gas | 15 | 300 |
| [G&#x2011;07] | Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if`-statement | 1 | 85 |
| [G&#x2011;08] | `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops | 49 | 2940 |
| [G&#x2011;09] | `require()`/`revert()` strings longer than 32 bytes cost extra gas | 14 | - |
| [G&#x2011;10] | Optimize names to save gas | 23 | 506 |
| [G&#x2011;11] | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 1 | 5 |
| [G&#x2011;12] | Splitting `require()` statements that use `&&` saves gas | 13 | 39 |
| [G&#x2011;13] | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 5 | - |
| [G&#x2011;14] | Using `private` rather than `public` for constants, saves gas | 1 | - |
| [G&#x2011;15] | Inverting the condition of an `if`-`else`-statement wastes gas | 2 | - |
| [G&#x2011;16] | `require()` or `revert()` statements that check input arguments should be at the top of the function | 1 | - |
| [G&#x2011;17] | Use custom errors rather than `revert()`/`require()` strings to save gas | 202 | - |
| [G&#x2011;18] | Functions guaranteed to revert when called by normal users can be marked `payable` | 66 | 1386 |
| [G&#x2011;19] | Don't use `_msgSender()` if not supporting EIP-2771 | 7 | 112 |

Total: 414 instances over 19 issues with **28941 gas** saved

Gas totals use lower bounds of ranges and count two iterations of each `for`-loop. All values above are runtime, not deployment, values; deployment values are listed in the individual issue descriptions. The table above as well as its gas numbers do not include any of the excluded findings.


## Gas Optimizations

### [G&#x2011;01]  Save gas by checking against default WETH address
You can save a Gcoldsload (**2100 gas**) in the address provider, plus the **100 gas** overhead of the external call, for every `receive()`, by creating an immutable `DEFAULT_WETH` variable which will store the initial WETH address, and change the require statement to be: `require(msg.ender == DEFAULT_WETH || msg.sender == <etc>)`.

*There is 1 instance of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolCore.sol

802      receive() external payable {
803          require(
804              msg.sender ==
805                  address(IPoolAddressesProvider(ADDRESSES_PROVIDER).getWETH()),
806              "Receive not allowed"
807          );
808:     }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L802-L808

### [G&#x2011;02]  Save gas by batching `NToken` operations
_Every_ external call made to a contract incurs at least **100 gas** of overhead. Since all of the IDs belong to the same NToken, you can prevent this overhead by having a `safeTransferFromBatch()` function, or by implementing EIP-1155 support, which natively supports batching.

*There are 2 instances of this issue:*
```solidity
File: /paraspace-core/contracts/ui/WPunkGateway.sol

102      function withdrawPunk(uint256[] calldata punkIndexes, address to)
103          external
104          nonReentrant
105      {
106          INToken nWPunk = INToken(
107              Pool.getReserveData(address(WPunk)).xTokenAddress
108          );
109          for (uint256 i = 0; i < punkIndexes.length; i++) {
110              nWPunk.safeTransferFrom(msg.sender, address(this), punkIndexes[i]);
111:         }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L102-L111

```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

28       function executeFlashClaim(
29           DataTypes.PoolStorage storage ps,
30           DataTypes.ExecuteFlashClaimParams memory params
31       ) external {
32           DataTypes.ReserveData storage reserve = ps._reserves[params.nftAsset];
33           address nTokenAddress = reserve.xTokenAddress;
34           ValidationLogic.validateFlashClaim(ps, params);
35   
36           uint256 i;
37           // step 1: moving underlying asset forward to receiver contract
38           for (i = 0; i < params.nftTokenIds.length; i++) {
39               INToken(nTokenAddress).transferUnderlyingTo(
40                   params.receiverAddress,
41                   params.nftTokenIds[i]
42               );
43:          }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L28-L43

### [G&#x2011;03]  Using `storage` instead of `memory` for structs/arrays saves gas
When fetching data from a storage location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (**2100 gas**) for *each* field of the struct/array. If the fields are read from the new memory variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct

*There are 5 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

40:           IX2Y2.SettleDetail memory detail = runInput.details[0];

41:           IX2Y2.Order memory order = runInput.orders[detail.orderIdx];

42:           IX2Y2.OrderItem memory item = order.items[detail.itemIdx];

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L40

```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

414               PriceInformation memory priceInfo = feederRegistrar.feederPrice[
415                   feeders[i]
416:              ];

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L414-L416

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

109               DataTypes.ReserveConfigurationMap
110:                  memory reserveConfiguration = reserve.configuration;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L109-L110

### [G&#x2011;04]  Multiple accesses of a mapping/array should use a local variable cache
The instances below point to the second+ access of a value inside a mapping/array, within a function. Caching a mapping's value in a local `storage` or `calldata` variable when the value is accessed [multiple times](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0), saves **~42 gas per access** due to not having to recalculate the key's keccak256 hash (Gkeccak256 - **30 gas**) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into memory/calldata

*There are 4 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit assetPriceMap[_asset] on line 242
247:          return assetPriceMap[_asset].twap;

/// @audit assetFeederMap[_asset] on line 282
284:          assetFeederMap[_asset].index = uint8(assets.length - 1);

/// @audit feederPositionMap[_feeder] on line 312
313:          feederPositionMap[_feeder].registered = true;

/// @audit assetPriceMap[_asset] on line 405
426:              return (false, assetPriceMap[_asset].twap);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L247

### [G&#x2011;05]  The result of function calls should be cached rather than re-calling the function
The instances below point to the second+ call of the function within a single function

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

/// @audit ADDRESSES_PROVIDER.getWETH() on line 475
477:                  liquidationAsset: ADDRESSES_PROVIDER.getWETH(),

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L477

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit _apeCoinStaking.apeCoin() on line 53
55:           _apeCoinStaking.apeCoin().safeTransfer(_apeRecipient, balance);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L55

### [G&#x2011;06]  `internal` functions only called once can be inlined to save gas
Not inlining costs **20 to 40 gas** because of two extra `JUMP` instructions and additional stack operations needed for function calls.

*There are 15 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

265:      function _whenNotPaused(address _asset) internal view {

278       function _addAsset(address _asset)
279           internal
280:          onlyWhenAssetNotExisted(_asset)

296       function _removeAsset(address _asset)
297           internal
298:          onlyWhenAssetExisted(_asset)

307       function _addFeeder(address _feeder)
308           internal
309:          onlyWhenFeederNotExisted(_feeder)

326       function _removeFeeder(address _feeder)
327           internal
328:          onlyWhenFeederExisted(_feeder)

351       function _checkValidity(address _asset, uint256 _twap)
352           internal
353           view
354:          returns (bool)

388:      function _addRawValue(address _asset, uint256 _twap) internal {

397       function _combine(address _asset, uint256 _twap)
398           internal
399           view
400:          returns (bool, uint256)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L265

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

221       function _getOracleData(UinswapV3PositionData memory positionData)
222           internal
223           view
224:          returns (PairOracleData memory)

282       function _getPendingFeeAmounts(UinswapV3PositionData memory positionData)
283           internal
284           view
285:          returns (uint256 token0Amount, uint256 token1Amount)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L221-L224

```solidity
File: paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

302       function _updateParaProxyImpl(
303           bytes32 id,
304           IParaProxy.ProxyImplementation[] calldata implementationParams,
305           address _init,
306:          bytes calldata _calldata

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L302-L306

```solidity
File: paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

101       function _calculateAuctionPriceMultiplierByTicks(uint256 ticks)
102           internal
103           view
104:          returns (uint256)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L101-L104

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

413:      function setSApeUseAsCollateral(address user) internal {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

51        function _decreaseLiquidity(
52            address user,
53            uint256 tokenId,
54            uint128 liquidityDecrease,
55            uint256 amount0Min,
56            uint256 amount1Min,
57            bool receiveEthAsWeth
58:       ) internal returns (uint256 amount0, uint256 amount1) {

144:      function _safeTransferETH(address to, uint256 value) internal {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L51-L58

### [G&#x2011;07]  Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if`-statement
`require(a <= b); x = b - a` => `require(a <= b); unchecked { x = b - a }`

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

/// @audit if-condition on line 874
877:                      msg.value - vars.actualLiquidationAmount

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L877

### [G&#x2011;08]  `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops
The `unchecked` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves **30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)**

*There are 49 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

229:          for (uint256 i = 0; i < _assets.length; i++) {

291:          for (uint256 i = 0; i < _assets.length; i++) {

321:          for (uint256 i = 0; i < _feeders.length; i++) {

413:          for (uint256 i = 0; i < feederSize; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

95:           for (uint256 i = 0; i < assets.length; i++) {

197:          for (uint256 i = 0; i < assets.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:          for (uint256 index = 0; index < tokenIds.length; index++) {

210:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38:           for (i = 0; i < params.nftTokenIds.length; i++) {

56:           for (i = 0; i < params.nftTokenIds.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

371:              for (uint256 index = 0; index < totalBalance; index++) {

466:          for (uint256 index = 0; index < totalBalance; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:          for (uint256 i = 0; i < marketplaceIds.length; i++) {

284:          for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:          for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:          for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58:           for (uint16 i = 0; i < params.reservesCount; i++) {

104:          for (uint256 i = 0; i < assets.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:              for (uint256 index = 0; index < params.tokenData.length; index++) {

195:          for (uint256 index = 0; index < params.tokenData.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

131:          for (uint256 index = 0; index < amount; index++) {

212:              for (uint256 index = 0; index < tokenIds.length; index++) {

465:              for (uint256 index = 0; index < tokenIds.length; index++) {

910:                  for (uint256 index = 0; index < tokenIds.length; index++) {

1034:         for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72:           for (uint256 index = 0; index < _nfts.length; index++) {

103:          for (uint256 index = 0; index < _nfts.length; index++) {

138:          for (uint256 index = 0; index < _nftPairs.length; index++) {

172:          for (uint256 index = 0; index < actualTransferAmount; index++) {

199:          for (uint256 index = 0; index < _nftPairs.length; index++) {

215:          for (uint256 index = 0; index < _nftPairs.length; index++) {

279:          for (uint256 index = 0; index < _nfts.length; index++) {

289:          for (uint256 index = 0; index < _nftPairs.length; index++) {

305:          for (uint256 index = 0; index < _nftPairs.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

634:          for (uint256 i = 0; i < reservesListCount; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213:          for (uint256 index = 0; index < totalBalance; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:          for (uint256 index = 0; index < tokenData.length; index++) {

280:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

107:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:               for (uint256 index = 0; index < tokenIds.length; index++) {

97:           for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

106:              for (uint256 index = 0; index < tokenIds.length; index++) {

145:          for (uint256 i = 0; i < ids.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

82:           for (uint256 i = 0; i < punkIndexes.length; i++) {

109:          for (uint256 i = 0; i < punkIndexes.length; i++) {

113:          for (uint256 i = 0; i < punkIndexes.length; i++) {

136:          for (uint256 i = 0; i < punkIndexes.length; i++) {

174:          for (uint256 i = 0; i < punkIndexes.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82

### [G&#x2011;09]  `require()`/`revert()` strings longer than 32 bytes cost extra gas
Each extra memory word of bytes past the original 32 [incurs an MSTORE](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs **3 gas**

*There are 14 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

225           require(
226               _assets.length == _twaps.length,
227               "NFTOracle: Tokens and price length differ"
228:          );

356:          require(_twap > 0, "NFTOracle: price should be more than 0");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L225-L228

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

195           require(
196               _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
197               "ERC721: approve caller is not owner nor approved for all"
198:          );

213           require(
214               _exists(tokenId),
215               "ERC721: approved query for nonexistent token"
216:          );

259           require(
260               _isApprovedOrOwner(_msgSender(), tokenId),
261               "ERC721: transfer caller is not owner nor approved"
262:          );

296           require(
297               _isApprovedOrOwner(_msgSender(), tokenId),
298               "ERC721: transfer caller is not owner nor approved"
299:          );

354           require(
355               _exists(tokenId),
356               "ERC721: operator query for nonexistent token"
357:          );

594           require(
595               index < balanceOf(owner),
596               "ERC721Enumerable: owner index out of bounds"
597:          );

618           require(
619               index < totalSupply(),
620               "ERC721Enumerable: global index out of bounds"
621:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L195-L198

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

88            require(
89                erc721Data.owners[tokenId] == from,
90                "ERC721: transfer from incorrect owner"
91:           );

92:           require(to != address(0), "ERC721: transfer to the zero address");

377           require(
378               _exists(erc721Data, tokenId),
379               "ERC721: startAuction for nonexistent token"
380:          );

395           require(
396               _exists(erc721Data, tokenId),
397               "ERC721: endAuction for nonexistent token"
398:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L88-L91

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

98                require(
99                    _isApprovedOrOwner(_msgSender(), tokenIds[index]),
100                   "ERC721: transfer caller is not owner nor approved"
101:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L98-L101

### [G&#x2011;10]  Optimize names to save gas
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

*There are 23 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit initialize(), addAssets(), removeAsset(), addFeeders(), removeFeeder(), setConfig(), setPause(), setPrice(), setMultiplePrices(), getFeederSize()
55:   contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L55

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

/// @audit getFallbackOracle()
21:   contract ParaSpaceOracle is IParaSpaceOracle {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L21

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

/// @audit getOnchainPositionData(), getLiquidityAmount(), getLiquidityAmountFromPositionData(), getLpFeeAmount(), getLpFeeAmountFromPositionData(), getTokenPrice(), getTokensPrices(), getTokensPricesSum()
17:   contract UniswapV3OracleWrapper is IUniswapV3OracleWrapper {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L17

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol

/// @audit executeStartAuction(), executeEndAuction()
15:   library AuctionLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L15

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol

/// @audit executeBorrow(), executeRepay()
21:   library BorrowLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L21

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

/// @audit executeFlashClaim()
14:   library FlashClaimLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L14

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

/// @audit executeLiquidateERC20(), executeLiquidateERC721()
36:   library LiquidationLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L36

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

/// @audit executeBuyWithCredit(), executeBatchBuyWithCredit(), executeAcceptBidWithCredit(), executeBatchAcceptBidWithCredit()
33:   library MarketplaceLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L33

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

/// @audit executeInitReserve(), executeRescueTokens(), executeMintToTreasury(), executeDropReserve(), executeGetUserAccountData(), executeGetAssetLtvAndLT()
23:   library PoolLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L23

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

/// @audit executeSupply(), executeSupplyERC721(), executeSupplyERC721FromNToken(), executeWithdraw(), executeWithdrawERC721(), executeDecreaseUniswapV3Liquidity(), executeFinalizeTransferERC20(), executeFinalizeTransferERC721(), executeUseERC20AsCollateral(), executeCollateralizeERC721(), executeUncollateralizeERC721()
28:   library SupplyLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L28

```solidity
File: paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

/// @audit getMaxPriceMultiplier(), getMinExpPriceMultiplier(), getMinPriceMultiplier(), getStepLinear(), getStepExp(), getTickLength()
20:   contract DefaultReserveAuctionStrategy is IReserveAuctionStrategy {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L20

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

/// @audit withdrawApeCoin(), claimApeCoin(), withdrawBAKC(), claimBAKC(), borrowApeAndStake(), unstakeApePositionAndRepay(), repayAndSupply()
23:   contract PoolApeStaking is

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L23

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

/// @audit initialize(), getReserveAddressById()
45:   contract PoolCore is

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L45

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit setIncentivesController(), setBalanceLimit()
29:   abstract contract MintableIncentivizedERC721 is

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit withdrawBAKC(), executeSetUnstakeApeIncentive(), executeUnstakePositionAndRepay(), getUserTotalStakingAmount(), getTokenIdStakingAmount()
18:   library ApeStakingLogic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L18

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit executeTransfer(), executeTransferCollateralizable(), executeMintMultiple(), executeBurnMultiple(), executeApprove(), executeApprovalForAll(), executeStartAuction(), executeEndAuction(), isAuctioned()
52:   library MintableERC721Logic {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L52

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit getBAKC(), getApeStaking(), setUnstakeApeIncentive(), unstakePositionAndRepay(), getUserApeStakingAmount()
20:   abstract contract NTokenApeStaking is NToken, INTokenApeStaking {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L20

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

/// @audit depositApeCoin(), claimApeCoin(), withdrawApeCoin(), depositBAKC(), claimBAKC(), withdrawBAKC()
15:   contract NTokenBAYC is NTokenApeStaking {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L15

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

/// @audit depositApeCoin(), claimApeCoin(), withdrawApeCoin(), depositBAKC(), claimBAKC(), withdrawBAKC()
15:   contract NTokenMAYC is NTokenApeStaking {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L15

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

/// @audit toggleNesting(), nestingPeriod(), nestingOpen()
27:   contract NTokenMoonBirds is NToken, IMoonBirdBase {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L27

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit getAtomicPricingConfig()
29:   contract NToken is VersionedInitializable, MintableIncentivizedERC721, INToken {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

/// @audit decreaseUniswapV3Liquidity()
26:   contract NTokenUniswapV3 is NToken, INTokenUniswapV3 {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L26

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

/// @audit initialize(), supplyPunk(), withdrawPunk(), acceptBidWithCredit(), batchAcceptBidWithCredit(), emergencyERC721TokenTransfer(), emergencyPunkTransfer(), getWPunkAddress()
19:   contract WPunkGateway is

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L19

### [G&#x2011;11]  `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
Saves **5 gas per loop**

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

450:                  j--;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L450

### [G&#x2011;12]  Splitting `require()` statements that use `&&` saves gas
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by **3 gas**

*There are 13 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

41            require(
42                // NOT criteria based and must be basic order
43                resolvers.length == 0 && isBasicOrder(advancedOrder),
44                Errors.INVALID_MARKETPLACE_ORDER
45:           );

71            require(
72                // NOT criteria based and must be basic order
73                advancedOrders.length == 2 &&
74                    isBasicOrder(advancedOrders[0]) &&
75                    isBasicOrder(advancedOrders[1]),
76                Errors.INVALID_MARKETPLACE_ORDER
77:           );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41-L45

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

171           require(
172               marketplaceIds.length == payloads.length &&
173                   payloads.length == credits.length,
174               Errors.INCONSISTENT_PARAMS_LENGTH
175:          );

279           require(
280               marketplaceIds.length == payloads.length &&
281                   payloads.length == credits.length,
282               Errors.INCONSISTENT_PARAMS_LENGTH
283:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-L175

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

546           require(
547               vars.collateralReserveActive && vars.principalReserveActive,
548               Errors.RESERVE_INACTIVE
549:          );

550           require(
551               !vars.collateralReservePaused && !vars.principalReservePaused,
552               Errors.RESERVE_PAUSED
553:          );

636           require(
637               vars.collateralReserveActive && vars.principalReserveActive,
638               Errors.RESERVE_INACTIVE
639:          );

640           require(
641               !vars.collateralReservePaused && !vars.principalReservePaused,
642               Errors.RESERVE_PAUSED
643:          );

672           require(
673               params.maxLiquidationAmount >= params.actualLiquidationAmount &&
674                   (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
675               Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
676:          );

1196              require(
1197                  vars.token0IsActive && vars.token1IsActive,
1198                  Errors.RESERVE_INACTIVE
1199:             );

1202              require(
1203                  !vars.token0IsPaused && !vars.token1IsPaused,
1204                  Errors.RESERVE_PAUSED
1205:             );

1208              require(
1209                  !vars.token0IsFrozen && !vars.token1IsFrozen,
1210                  Errors.RESERVE_FROZEN
1211:             );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L549

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

216           require(
217               value > MIN_AUCTION_HEALTH_FACTOR &&
218                   value <= MAX_AUCTION_HEALTH_FACTOR,
219               Errors.INVALID_AMOUNT
220:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-L220

### [G&#x2011;13]  Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead
> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra [**22-28 gas**](https://gist.github.com/IllIllI000/9388d20c70f9a4632eb3ca7836f54977) (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

*There are 5 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit uint64 collateralizedBalance
177           collateralizedBalance = useAsCollateral
178               ? collateralizedBalance + 1
179:              : collateralizedBalance - 1;

/// @audit uint64 oldCollateralizedBalance
201           oldCollateralizedBalance = erc721Data
202               .userState[to]
203:              .collateralizedBalance;

/// @audit uint64 newCollateralizedBalance
240           newCollateralizedBalance =
241               oldCollateralizedBalance +
242:              collateralizedTokens;

/// @audit uint64 oldCollateralizedBalance
276           oldCollateralizedBalance = erc721Data
277               .userState[user]
278:              .collateralizedBalance;

/// @audit uint64 newCollateralizedBalance
319           newCollateralizedBalance =
320               oldCollateralizedBalance -
321:              burntCollateralizedTokens;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L177-L179

### [G&#x2011;14]  Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

73:       bool public immutable ATOMIC_PRICING;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L73

### [G&#x2011;15]  Inverting the condition of an `if`-`else`-statement wastes gas
Flipping the `true` and `false` blocks instead saves ***[3 gas](https://gist.github.com/IllIllI000/44da6fbe9d12b9ab21af82f14add56b9)***

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

401           if (!vars.isETH) {
402               IERC20(vars.creditToken).safeTransferFrom(
403                   params.orderInfo.taker,
404                   address(this),
405                   downpayment
406               );
407               _checkAllowance(vars.creditToken, params.marketplace.operator);
408               // convert to (priceEth, downpaymentEth)
409               price = 0;
410               downpayment = 0;
411           } else {
412               require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);
413:          }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L401-L413

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

562           if (!_isAuctioned) {
563               auction = DataTypes.Auction({startTime: 0});
564           } else {
565               auction = _ERC721Data.auctions[tokenId];
566:          }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L562-L566

### [G&#x2011;16]  `require()` or `revert()` statements that check input arguments should be at the top of the function
Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (**2100 gas***) in a function that may ultimately revert in the unhappy case.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

/// @audit expensive op on line 212
214:          require(value != 0, Errors.INVALID_AMOUNT);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L214

### [G&#x2011;17]  Use custom errors rather than `revert()`/`require()` strings to save gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

*There are 202 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

41            require(
42                // NOT criteria based and must be basic order
43                resolvers.length == 0 && isBasicOrder(advancedOrder),
44                Errors.INVALID_MARKETPLACE_ORDER
45:           );

51:           require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

54            require(
55                orderInfo.consideration.length > 0,
56                Errors.INVALID_MARKETPLACE_ORDER
57:           );

71            require(
72                // NOT criteria based and must be basic order
73                advancedOrders.length == 2 &&
74                    isBasicOrder(advancedOrders[0]) &&
75                    isBasicOrder(advancedOrders[1]),
76                Errors.INVALID_MARKETPLACE_ORDER
77:           );

84:           require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

87            require(
88                orderInfo.consideration.length > 0,
89                Errors.INVALID_MARKETPLACE_ORDER
90:           );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41-L45

```solidity
File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

38:           require(runInput.details.length == 1, Errors.INVALID_MARKETPLACE_ORDER);

44            require(
45                IX2Y2.Op.COMPLETE_SELL_OFFER == detail.op,
46                Errors.INVALID_MARKETPLACE_ORDER
47:           );

51:           require(nfts.length == 1, Errors.INVALID_MARKETPLACE_ORDER);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L38

```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

225           require(
226               _assets.length == _twaps.length,
227               "NFTOracle: Tokens and price length differ"
228:          );

243           require(
244               (block.number - updatedAt) <= config.expirationPeriod,
245               "NFTOracle: asset price expired"
246:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L225-L228

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

91            require(
92                assets.length == sources.length,
93                Errors.INCONSISTENT_PARAMS_LENGTH
94:           );

96                require(
97                    assets[i] != BASE_CURRENCY,
98                    Errors.SET_ORACLE_SOURCE_NOT_ALLOWED
99:               );

134:          require(price != 0, Errors.ORACLE_PRICE_NOT_READY);

222           require(
223               aclManager.isAssetListingAdmin(msg.sender) ||
224                   aclManager.isPoolAdmin(msg.sender),
225               Errors.CALLER_NOT_ASSET_LISTING_OR_POOL_ADMIN
226:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L91-L94

```solidity
File: paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

86:           require(id != POOL, Errors.INVALID_ADDRESSES_PROVIDER_ID);

268:          require(newAddress != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L86

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

46            require(
47                IFlashClaimReceiver(params.receiverAddress).executeOperation(
48                    params.nftAsset,
49                    params.nftTokenIds,
50                    params.params
51                ),
52                Errors.INVALID_FLASH_CLAIM_RECEIVER
53:           );

86            require(
87                healthFactor > DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
88                Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
89:           );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L46-L53

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

171           require(
172               marketplaceIds.length == payloads.length &&
173                   payloads.length == credits.length,
174               Errors.INCONSISTENT_PARAMS_LENGTH
175:          );

245:          require(vars.orderInfo.taker == onBehalfOf, Errors.INVALID_ORDER_TAKER);

279           require(
280               marketplaceIds.length == payloads.length &&
281                   payloads.length == credits.length,
282               Errors.INCONSISTENT_PARAMS_LENGTH
283:          );

294               require(
295                   vars.orderInfo.taker == onBehalfOf,
296                   Errors.INVALID_ORDER_TAKER
297:              );

384               require(
385                   item.startAmount == item.endAmount,
386                   Errors.INVALID_MARKETPLACE_ORDER
387:              );

388               require(
389                   item.itemType == ItemType.ERC20 ||
390                       (vars.isETH && item.itemType == ItemType.NATIVE),
391                   Errors.INVALID_ASSET_TYPE
392:              );

393               require(
394                   item.token == params.credit.token,
395                   Errors.CREDIT_DOES_NOT_MATCH_ORDER
396:              );

412:              require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);

440:          require(vars.xTokenAddress != address(0), Errors.ASSET_NOT_LISTED);

472               require(
473                   item.itemType == ItemType.ERC721,
474                   Errors.INVALID_ASSET_TYPE
475:              );

489:                  require(isNToken, Errors.ASSET_NOT_LISTED);

494               require(
495                   INToken(vars.xTokenAddress).getXTokenType() !=
496                       XTokenType.NTokenUniswapV3,
497                   Errors.UNIV3_NOT_ALLOWED
498:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-L175

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

45:               require(Address.isContract(params.asset), Errors.NOT_CONTRACT);

56:           require(!reserveAlreadyAdded, Errors.RESERVE_ALREADY_ADDED);

66            require(
67                params.reservesCount < params.maxNumberReserves,
68                Errors.NO_MORE_RESERVES_ALLOWED
69:           );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L45

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

409           require(
410               nToken.getXTokenType() == XTokenType.NTokenUniswapV3,
411               Errors.ONLY_UNIV3_ALLOWED
412:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L409-L412

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

68:           require(amount != 0, Errors.INVALID_AMOUNT);

71            require(
72                xToken.getXTokenType() != XTokenType.PTokenSApe,
73                Errors.SAPE_NOT_ALLOWED
74:           );

84:           require(reserveAssetType == assetType, Errors.INVALID_ASSET_TYPE);

85:           require(isActive, Errors.RESERVE_INACTIVE);

86:           require(!isPaused, Errors.RESERVE_PAUSED);

87:           require(!isFrozen, Errors.RESERVE_FROZEN);

92                require(
93                    supplyCap == 0 ||
94                        (IPToken(reserveCache.xTokenAddress)
95                            .scaledTotalSupply()
96                            .rayMul(reserveCache.nextLiquidityIndex) + amount) <=
97                        supplyCap *
98                            (10**reserveCache.reserveConfiguration.getDecimals()),
99                    Errors.SUPPLY_CAP_EXCEEDED
100:              );

102               require(
103                   supplyCap == 0 ||
104                       (INToken(reserveCache.xTokenAddress).totalSupply() +
105                           amount <=
106                           supplyCap),
107                   Errors.SUPPLY_CAP_EXCEEDED
108:              );

123           require(
124               msg.sender == reserveCache.xTokenAddress,
125               Errors.CALLER_NOT_XTOKEN
126:          );

133               require(
134                   IERC721(params.asset).ownerOf(
135                       params.tokenData[index].tokenId
136                   ) == reserveCache.xTokenAddress,
137                   Errors.NOT_THE_OWNER
138:              );

140               require(
141                   IERC721(reserveCache.xTokenAddress).ownerOf(
142                       params.tokenData[index].tokenId
143                   ) == address(0x0),
144                   Errors.NOT_THE_OWNER
145:              );

160:          require(amount != 0, Errors.INVALID_AMOUNT);

162           require(
163               amount <= userBalance,
164               Errors.NOT_ENOUGH_AVAILABLE_USER_BALANCE
165:          );

168           require(
169               xToken.getXTokenType() != XTokenType.PTokenSApe,
170               Errors.SAPE_NOT_ALLOWED
171:          );

181           require(
182               reserveAssetType == DataTypes.AssetType.ERC20,
183               Errors.INVALID_ASSET_TYPE
184:          );

185:          require(isActive, Errors.RESERVE_INACTIVE);

186:          require(!isPaused, Errors.RESERVE_PAUSED);

203           require(
204               reserveAssetType == DataTypes.AssetType.ERC721,
205               Errors.INVALID_ASSET_TYPE
206:          );

207:          require(isActive, Errors.RESERVE_INACTIVE);

208:          require(!isPaused, Errors.RESERVE_PAUSED);

258:          require(params.amount != 0, Errors.INVALID_AMOUNT);

269           require(
270               vars.assetType == DataTypes.AssetType.ERC20,
271               Errors.INVALID_ASSET_TYPE
272:          );

273:          require(vars.isActive, Errors.RESERVE_INACTIVE);

274:          require(!vars.isPaused, Errors.RESERVE_PAUSED);

275:          require(!vars.isFrozen, Errors.RESERVE_FROZEN);

276:          require(vars.borrowingEnabled, Errors.BORROWING_NOT_ENABLED);

278           require(
279               params.priceOracleSentinel == address(0) ||
280                   IPriceOracleSentinel(params.priceOracleSentinel)
281                       .isBorrowAllowed(),
282               Errors.PRICE_ORACLE_SENTINEL_CHECK_FAILED
283:          );

306                   require(
307                       vars.totalDebt <= vars.borrowCap * vars.assetUnit,
308                       Errors.BORROW_CAP_EXCEEDED
309:                  );

335           require(
336               vars.userCollateralInBaseCurrency != 0,
337               Errors.COLLATERAL_BALANCE_IS_ZERO
338:          );

339:          require(vars.currentLtv != 0, Errors.LTV_VALIDATION_FAILED);

341           require(
342               vars.healthFactor > HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
343               Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
344:          );

357           require(
358               vars.collateralNeededInBaseCurrency <=
359                   vars.userCollateralInBaseCurrency,
360               Errors.COLLATERAL_CANNOT_COVER_NEW_BORROW
361:          );

377:          require(amountSent != 0, Errors.INVALID_AMOUNT);

378           require(
379               amountSent != type(uint256).max || msg.sender == onBehalfOf,
380               Errors.NO_EXPLICIT_AMOUNT_TO_REPAY_ON_BEHALF
381:          );

390:          require(isActive, Errors.RESERVE_INACTIVE);

391:          require(!isPaused, Errors.RESERVE_PAUSED);

392           require(
393               assetType == DataTypes.AssetType.ERC20,
394               Errors.INVALID_ASSET_TYPE
395:          );

401           require(
402               (variableDebtPreviousIndex < reserveCache.nextVariableBorrowIndex),
403               Errors.SAME_BLOCK_BORROW_REPAY
404:          );

406:          require((variableDebt != 0), Errors.NO_DEBT_OF_SELECTED_TYPE);

418:          require(userBalance != 0, Errors.UNDERLYING_BALANCE_ZERO);

421           require(
422               xToken.getXTokenType() != XTokenType.PTokenSApe,
423               Errors.SAPE_NOT_ALLOWED
424:          );

434           require(
435               reserveAssetType == DataTypes.AssetType.ERC20,
436               Errors.INVALID_ASSET_TYPE
437:          );

438:          require(isActive, Errors.RESERVE_INACTIVE);

439:          require(!isPaused, Errors.RESERVE_PAUSED);

456           require(
457               reserveAssetType == DataTypes.AssetType.ERC721,
458               Errors.INVALID_ASSET_TYPE
459:          );

460:          require(isActive, Errors.RESERVE_INACTIVE);

461:          require(!isPaused, Errors.RESERVE_PAUSED);

515           require(
516               vars.collateralReserveAssetType == DataTypes.AssetType.ERC20,
517               Errors.INVALID_ASSET_TYPE
518:          );

520           require(
521               msg.value == 0 || params.liquidationAsset == params.weth,
522               Errors.INVALID_LIQUIDATION_ASSET
523:          );

525           require(
526               msg.value == 0 || msg.value >= params.actualLiquidationAmount,
527               Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
528:          );

533           require(
534               xToken.getXTokenType() != XTokenType.PTokenSApe,
535               Errors.SAPE_NOT_ALLOWED
536:          );

546           require(
547               vars.collateralReserveActive && vars.principalReserveActive,
548               Errors.RESERVE_INACTIVE
549:          );

550           require(
551               !vars.collateralReservePaused && !vars.principalReservePaused,
552               Errors.RESERVE_PAUSED
553:          );

555           require(
556               params.priceOracleSentinel == address(0) ||
557                   params.healthFactor <
558                   MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD ||
559                   IPriceOracleSentinel(params.priceOracleSentinel)
560                       .isLiquidationAllowed(),
561               Errors.PRICE_ORACLE_SENTINEL_CHECK_FAILED
562:          );

564           require(
565               params.healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
566               Errors.HEALTH_FACTOR_NOT_BELOW_THRESHOLD
567:          );

574           require(
575               vars.isCollateralEnabled,
576               Errors.COLLATERAL_CANNOT_BE_AUCTIONED_OR_LIQUIDATED
577:          );

578           require(
579               params.totalDebt != 0,
580               Errors.SPECIFIED_CURRENCY_NOT_BORROWED_BY_USER
581:          );

596           require(
597               params.liquidator != params.borrower,
598               Errors.LIQUIDATOR_CAN_NOT_BE_SELF
599:          );

611           require(
612               vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,
613               Errors.INVALID_ASSET_TYPE
614:          );

636           require(
637               vars.collateralReserveActive && vars.principalReserveActive,
638               Errors.RESERVE_INACTIVE
639:          );

640           require(
641               !vars.collateralReservePaused && !vars.principalReservePaused,
642               Errors.RESERVE_PAUSED
643:          );

645           require(
646               params.priceOracleSentinel == address(0) ||
647                   params.healthFactor <
648                   MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD ||
649                   IPriceOracleSentinel(params.priceOracleSentinel)
650                       .isLiquidationAllowed(),
651               Errors.PRICE_ORACLE_SENTINEL_CHECK_FAILED
652:          );

655               require(
656                   params.healthFactor < params.auctionRecoveryHealthFactor,
657                   Errors.ERC721_HEALTH_FACTOR_NOT_BELOW_THRESHOLD
658:              );

659               require(
660                   IAuctionableERC721(params.xTokenAddress).isAuctioned(
661                       params.tokenId
662                   ),
663                   Errors.AUCTION_NOT_STARTED
664:              );

666               require(
667                   params.healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
668                   Errors.ERC721_HEALTH_FACTOR_NOT_BELOW_THRESHOLD
669:              );

672           require(
673               params.maxLiquidationAmount >= params.actualLiquidationAmount &&
674                   (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
675               Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
676:          );

686           require(
687               vars.isCollateralEnabled,
688               Errors.COLLATERAL_CANNOT_BE_AUCTIONED_OR_LIQUIDATED
689:          );

690:          require(params.globalDebt != 0, Errors.GLOBAL_DEBT_IS_ZERO);

732           require(
733               healthFactor >= HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
734               Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
735:          );

757           require(
758               vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,
759               Errors.INVALID_ASSET_TYPE
760:          );

762           require(
763               IERC721(params.xTokenAddress).ownerOf(params.tokenId) ==
764                   params.user,
765               Errors.NOT_THE_OWNER
766:          );

768:          require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

769:          require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

771           require(
772               collateralReserve.auctionStrategyAddress != address(0),
773               Errors.AUCTION_NOT_ENABLED
774:          );

775           require(
776               !IAuctionableERC721(params.xTokenAddress).isAuctioned(
777                   params.tokenId
778               ),
779               Errors.AUCTION_ALREADY_STARTED
780:          );

782           require(
783               params.erc721HealthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
784               Errors.ERC721_HEALTH_FACTOR_NOT_BELOW_THRESHOLD
785:          );

795           require(
796               vars.isCollateralEnabled,
797               Errors.COLLATERAL_CANNOT_BE_AUCTIONED_OR_LIQUIDATED
798:          );

815           require(
816               vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,
817               Errors.INVALID_ASSET_TYPE
818:          );

819           require(
820               IERC721(params.xTokenAddress).ownerOf(params.tokenId) ==
821                   params.user,
822               Errors.NOT_THE_OWNER
823:          );

824:          require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

825:          require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

826           require(
827               IAuctionableERC721(params.xTokenAddress).isAuctioned(
828                   params.tokenId
829               ),
830               Errors.AUCTION_NOT_STARTED
831:          );

833           require(
834               params.erc721HealthFactor > params.auctionRecoveryHealthFactor,
835               Errors.ERC721_HEALTH_FACTOR_NOT_ABOVE_THRESHOLD
836:          );

869           require(
870               !hasZeroLtvCollateral || reserve.configuration.getLtv() == 0,
871               Errors.LTV_VALIDATION_FAILED
872:          );

918:                      require(assetLTV == 0, Errors.LTV_VALIDATION_FAILED);

921                   require(
922                       reserve.configuration.getLtv() == 0,
923                       Errors.LTV_VALIDATION_FAILED
924:                  );

937:          require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

950:          require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

975:          require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

976           require(
977               reserve.id != 0 || reservesList[0] == asset,
978               Errors.ASSET_NOT_LISTED
979:          );

980           require(
981               IToken(reserve.variableDebtTokenAddress).totalSupply() == 0,
982               Errors.VARIABLE_DEBT_SUPPLY_NOT_ZERO
983:          );

984           require(
985               IToken(reserve.xTokenAddress).totalSupply() == 0,
986               Errors.XTOKEN_SUPPLY_NOT_ZERO
987:          );

1000          require(
1001              reserve.configuration.getAssetType() == DataTypes.AssetType.ERC721,
1002              Errors.INVALID_ASSET_TYPE
1003:         );

1004          require(
1005              params.receiverAddress != address(0),
1006              Errors.ZERO_ADDRESS_NOT_VALID
1007:         );

1011          require(
1012              tokenType != XTokenType.NTokenUniswapV3,
1013              Errors.UNIV3_NOT_ALLOWED
1014:         );

1029:             require(isActive, Errors.RESERVE_INACTIVE);

1030:             require(!isPaused, Errors.RESERVE_PAUSED);

1035              require(
1036                  nToken.ownerOf(params.nftTokenIds[i]) == msg.sender,
1037                  Errors.NOT_THE_OWNER
1038:             );

1057:         require(isActive, Errors.RESERVE_INACTIVE);

1058:         require(!isPaused, Errors.RESERVE_PAUSED);

1059          require(
1060              assetType == DataTypes.AssetType.ERC20,
1061              Errors.INVALID_ASSET_TYPE
1062:         );

1068:         require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

1074:         require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

1075          require(
1076              keccak256(abi.encodePacked(params.orderInfo.id)) ==
1077                  keccak256(abi.encodePacked(params.credit.orderId)),
1078              Errors.CREDIT_DOES_NOT_MATCH_ORDER
1079:         );

1080          require(
1081              verifyCreditSignature(
1082                  params.credit,
1083                  params.orderInfo.maker,
1084                  params.credit.v,
1085                  params.credit.r,
1086                  params.credit.s
1087              ),
1088              Errors.INVALID_CREDIT_SIGNATURE
1089:         );

1196              require(
1197                  vars.token0IsActive && vars.token1IsActive,
1198                  Errors.RESERVE_INACTIVE
1199:             );

1202              require(
1203                  !vars.token0IsPaused && !vars.token1IsPaused,
1204                  Errors.RESERVE_PAUSED
1205:             );

1208              require(
1209                  !vars.token0IsFrozen && !vars.token1IsFrozen,
1210                  Errors.RESERVE_FROZEN
1211:             );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L68

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

73                require(
74                    nToken.ownerOf(_nfts[index].tokenId) == msg.sender,
75                    Errors.NOT_THE_OWNER
76:               );

85            require(
86                getUserHf(msg.sender) >
87                    DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
88                Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
89:           );

104               require(
105                   nToken.ownerOf(_nfts[index]) == msg.sender,
106                   Errors.NOT_THE_OWNER
107:              );

112           require(
113               getUserHf(msg.sender) >
114                   DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
115               Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
116:          );

139               require(
140                   INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
141                       msg.sender,
142                   Errors.NOT_THE_OWNER
143:              );

180           require(
181               getUserHf(msg.sender) >
182                   DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
183               Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
184:          );

200               require(
201                   INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
202                       msg.sender,
203                   Errors.NOT_THE_OWNER
204:              );

280               require(
281                   INToken(localVar.nTokenAddress).ownerOf(_nfts[index].tokenId) ==
282                       msg.sender,
283                   Errors.NOT_THE_OWNER
284:              );

290               require(
291                   INToken(localVar.nTokenAddress).ownerOf(
292                       _nftPairs[index].mainTokenId
293                   ) == msg.sender,
294                   Errors.NOT_THE_OWNER
295:              );

338           require(
339               localVar.apeCoin.balanceOf(localVar.nTokenAddress) ==
340                   localVar.beforeBalance,
341               Errors.TOTAL_STAKING_AMOUNT_WRONG
342:          );

356               require(
357                   getUserHf(positionOwner) <
358                       DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
359                   Errors.HEALTH_FACTOR_NOT_BELOW_THRESHOLD
360:              );

380           require(
381               msg.sender == ps._reserves[underlyingAsset].xTokenAddress,
382               Errors.CALLER_NOT_XTOKEN
383:          );

453:          require(isActive, Errors.RESERVE_INACTIVE);

454:          require(!isPaused, Errors.RESERVE_PAUSED);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L73-L76

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

80            require(
81                provider == ADDRESSES_PROVIDER,
82                Errors.INVALID_ADDRESSES_PROVIDER
83:           );

692           require(
693               msg.sender == ps._reserves[asset].xTokenAddress,
694               Errors.CALLER_NOT_XTOKEN
695:          );

726           require(
727               msg.sender == ps._reserves[asset].xTokenAddress,
728               Errors.CALLER_NOT_XTOKEN
729:          );

761           require(
762               reserve.id != 0 || ps._reservesList[0] == underlyingAsset,
763               Errors.ASSET_NOT_LISTED
764:          );

803           require(
804               msg.sender ==
805                   address(IPoolAddressesProvider(ADDRESSES_PROVIDER).getWETH()),
806               "Receive not allowed"
807:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L80-L83

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

70            require(
71                ADDRESSES_PROVIDER.getPoolConfigurator() == msg.sender,
72                Errors.CALLER_NOT_POOL_CONFIGURATOR
73:           );

77            require(
78                IACLManager(ADDRESSES_PROVIDER.getACLManager()).isPoolAdmin(
79                    msg.sender
80                ),
81                Errors.CALLER_NOT_POOL_ADMIN
82:           );

157:          require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

158           require(
159               ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
160               Errors.ASSET_NOT_LISTED
161:          );

172:          require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

173           require(
174               ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
175               Errors.ASSET_NOT_LISTED
176:          );

187:          require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

188           require(
189               ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
190               Errors.ASSET_NOT_LISTED
191:          );

214:          require(value != 0, Errors.INVALID_AMOUNT);

216           require(
217               value > MIN_AUCTION_HEALTH_FACTOR &&
218                   value <= MAX_AUCTION_HEALTH_FACTOR,
219               Errors.INVALID_AMOUNT
220:          );

271:          require(user != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

281           require(
282               erc721HealthFactor > ps._auctionRecoveryHealthFactor,
283               Errors.ERC721_HEALTH_FACTOR_NOT_ABOVE_THRESHOLD
284:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L70-L73

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

49            require(
50                aclManager.isPoolAdmin(msg.sender),
51                Errors.CALLER_NOT_POOL_ADMIN
52:           );

60:           require(_msgSender() == address(POOL), Errors.CALLER_MUST_BE_POOL);

195           require(
196               _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
197               "ERC721: approve caller is not owner nor approved for all"
198:          );

213           require(
214               _exists(tokenId),
215               "ERC721: approved query for nonexistent token"
216:          );

259           require(
260               _isApprovedOrOwner(_msgSender(), tokenId),
261               "ERC721: transfer caller is not owner nor approved"
262:          );

296           require(
297               _isApprovedOrOwner(_msgSender(), tokenId),
298               "ERC721: transfer caller is not owner nor approved"
299:          );

354           require(
355               _exists(tokenId),
356               "ERC721: operator query for nonexistent token"
357:          );

594           require(
595               index < balanceOf(owner),
596               "ERC721Enumerable: owner index out of bounds"
597:          );

618           require(
619               index < totalSupply(),
620               "ERC721Enumerable: global index out of bounds"
621:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L49-L52

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

67            require(
68                incentive < PercentageMath.HALF_PERCENTAGE_FACTOR,
69                "Value Too High"
70:           );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L67-L70

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

88            require(
89                erc721Data.owners[tokenId] == from,
90                "ERC721: transfer from incorrect owner"
91:           );

93            require(
94                !isAuctioned(erc721Data, POOL, tokenId),
95                Errors.TOKEN_IN_AUCTION
96:           );

167               require(
168                   !isAuctioned(erc721Data, POOL, tokenId),
169                   Errors.TOKEN_IN_AUCTION
170:              );

210               require(
211                   !_exists(erc721Data, tokenId),
212                   "ERC721: token already minted"
213:              );

284               require(
285                   !isAuctioned(erc721Data, POOL, tokenId),
286                   Errors.TOKEN_IN_AUCTION
287:              );

373           require(
374               !isAuctioned(erc721Data, POOL, tokenId),
375               Errors.AUCTION_ALREADY_STARTED
376:          );

377           require(
378               _exists(erc721Data, tokenId),
379               "ERC721: startAuction for nonexistent token"
380:          );

391           require(
392               isAuctioned(erc721Data, POOL, tokenId),
393               Errors.AUCTION_NOT_STARTED
394:          );

395           require(
396               _exists(erc721Data, tokenId),
397               "ERC721: endAuction for nonexistent token"
398:          );

409               require(
410                   balanceLimit == 0 || balance <= balanceLimit,
411                   Errors.NTOKEN_BALANCE_EXCEEDED
412:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L88-L91

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

70:           require(msg.sender == _underlyingAsset, Errors.OPERATION_NOT_SUPPORTED);

98                require(
99                    _isApprovedOrOwner(_msgSender(), tokenIds[index]),
100                   "ERC721: transfer caller is not owner nor approved"
101:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L70

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

60:           require(initializingPool == POOL, Errors.POOL_ADDRESSES_DO_NOT_MATCH);

64:           require(underlyingAsset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

141           require(
142               token != _underlyingAsset,
143               Errors.UNDERLYING_ASSET_CAN_NOT_BE_TRANSFERRED
144:          );

172           require(
173               airdropContract != address(0),
174               Errors.INVALID_AIRDROP_CONTRACT_ADDRESS
175:          );

176:          require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L60

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

131:          require(user == ownerOf(tokenId), Errors.NOT_THE_OWNER);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L131

### [G&#x2011;18]  Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are 
`CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about **21 gas per call** to the function, in addition to the extra deployment cost

*There are 66 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

139       function addAssets(address[] calldata _assets)
140           external
141:          onlyRole(DEFAULT_ADMIN_ROLE)

148       function removeAsset(address _asset)
149           external
150           onlyRole(DEFAULT_ADMIN_ROLE)
151:          onlyWhenAssetExisted(_asset)

158       function addFeeders(address[] calldata _feeders)
159           external
160:          onlyRole(DEFAULT_ADMIN_ROLE)

175       function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
176           external
177:          onlyRole(DEFAULT_ADMIN_ROLE)

183       function setPause(address _asset, bool _flag)
184           external
185:          onlyRole(DEFAULT_ADMIN_ROLE)

195       function setPrice(address _asset, uint256 _twap)
196           public
197           onlyRole(UPDATER_ROLE)
198           onlyWhenAssetExisted(_asset)
199:          whenNotPaused(_asset)

221       function setMultiplePrices(
222           address[] calldata _assets,
223           uint256[] calldata _twaps
224:      ) external onlyRole(UPDATER_ROLE) {

278       function _addAsset(address _asset)
279           internal
280:          onlyWhenAssetNotExisted(_asset)

296       function _removeAsset(address _asset)
297           internal
298:          onlyWhenAssetExisted(_asset)

307       function _addFeeder(address _feeder)
308           internal
309:          onlyWhenFeederNotExisted(_feeder)

326       function _removeFeeder(address _feeder)
327           internal
328:          onlyWhenFeederExisted(_feeder)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L139-L141

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

66        function setAssetSources(
67            address[] calldata assets,
68            address[] calldata sources
69:       ) external override onlyAssetListingOrPoolAdmins {

74        function setFallbackOracle(address fallbackOracle)
75            external
76            override
77:           onlyAssetListingOrPoolAdmins

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L66-L69

```solidity
File: paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

56        function setMarketId(string memory newMarketId)
57            external
58            override
59:           onlyOwner

70        function setAddress(bytes32 id, address newAddress)
71            external
72            override
73:           onlyOwner

81        function setAddressAsProxy(bytes32 id, address newImplementationAddress)
82            external
83            override
84:           onlyOwner

105       function updatePoolImpl(
106           IParaProxy.ProxyImplementation[] calldata implementationParams,
107           address _init,
108           bytes calldata _calldata
109:      ) external override onlyOwner {

121       function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
122           external
123           override
124:          onlyOwner

142       function setPriceOracle(address newPriceOracle)
143           external
144           override
145:          onlyOwner

158:      function setACLManager(address newAclManager) external override onlyOwner {

170:      function setACLAdmin(address newAclAdmin) external override onlyOwner {

182       function setPriceOracleSentinel(address newPriceOracleSentinel)
183           external
184           override
185:          onlyOwner

224       function setProtocolDataProvider(address newDataProvider)
225           external
226           override
227:          onlyOwner

235:      function setWETH(address newWETH) external override onlyOwner {

242       function setMarketplace(
243           bytes32 id,
244           address marketplace,
245           address adapter,
246           address operator,
247           bool paused
248:      ) external override onlyOwner {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56-L59

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

110       function initReserve(
111           address asset,
112           address xTokenAddress,
113           address variableDebtAddress,
114           address interestRateStrategyAddress,
115           address auctionStrategyAddress
116:      ) external virtual override onlyPoolConfigurator {

139       function dropReserve(address asset)
140           external
141           virtual
142           override
143:          onlyPoolConfigurator

151       function setReserveInterestRateStrategyAddress(
152           address asset,
153           address rateStrategyAddress
154:      ) external virtual override onlyPoolConfigurator {

166       function setReserveAuctionStrategyAddress(
167           address asset,
168           address auctionStrategyAddress
169:      ) external virtual override onlyPoolConfigurator {

181       function setConfiguration(
182           address asset,
183           DataTypes.ReserveConfigurationMap calldata configuration
184:      ) external virtual override onlyPoolConfigurator {

196       function rescueTokens(
197           DataTypes.AssetType assetType,
198           address token,
199           address to,
200           uint256 amountOrTokenId
201:      ) external virtual override onlyPoolAdmin {

206       function setAuctionRecoveryHealthFactor(uint64 value)
207           external
208           virtual
209           override
210:          onlyPoolConfigurator

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L110-L116

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

131       function setIncentivesController(IRewardController controller)
132           external
133:          onlyPoolAdmin

142:      function setBalanceLimit(uint64 limit) external onlyPoolAdmin {

458       function setIsUsedAsCollateral(
459           uint256 tokenId,
460           bool useAsCollateral,
461           address sender
462:      ) external virtual override onlyPool nonReentrant returns (bool) {

474       function batchSetIsUsedAsCollateral(
475           uint256[] calldata tokenIds,
476           bool useAsCollateral,
477           address sender
478       )
479           external
480           virtual
481           override
482           onlyPool
483           nonReentrant
484           returns (
485               uint256 oldCollateralizedBalance,
486:              uint256 newCollateralizedBalance

529       function startAuction(uint256 tokenId)
530           external
531           virtual
532           override
533           onlyPool
534:          nonReentrant

540       function endAuction(uint256 tokenId)
541           external
542           virtual
543           override
544           onlyPool
545:          nonReentrant

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131-L133

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

102       function burn(
103           address from,
104           address receiverOfUnderlying,
105           uint256[] calldata tokenIds
106:      ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

136:      function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {

159       function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)
160           external
161           onlyPool
162:          nonReentrant

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102-L106

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

26        function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
27            external
28            onlyPool
29:           nonReentrant

39        function claimApeCoin(uint256[] calldata _nfts, address _recipient)
40            external
41            onlyPool
42:           nonReentrant

52        function withdrawApeCoin(
53            ApeCoinStaking.SingleNft[] calldata _nfts,
54            address _recipient
55:       ) external onlyPool nonReentrant {

66        function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
67            external
68            onlyPool
69:           nonReentrant

82        function claimBAKC(
83            ApeCoinStaking.PairNft[] calldata _nftPairs,
84            address _recipient
85:       ) external onlyPool nonReentrant {

97        function withdrawBAKC(
98            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99            address _apeRecipient
100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L26-L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

26        function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
27            external
28            onlyPool
29:           nonReentrant

39        function claimApeCoin(uint256[] calldata _nfts, address _recipient)
40            external
41            onlyPool
42:           nonReentrant

52        function withdrawApeCoin(
53            ApeCoinStaking.SingleNft[] calldata _nfts,
54            address _recipient
55:       ) external onlyPool nonReentrant {

66        function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
67            external
68            onlyPool
69:           nonReentrant

82        function claimBAKC(
83            ApeCoinStaking.PairNft[] calldata _nftPairs,
84            address _recipient
85:       ) external onlyPool nonReentrant {

97        function withdrawBAKC(
98            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99            address _apeRecipient
100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26-L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

40        function burn(
41            address from,
42            address receiverOfUnderlying,
43            uint256[] calldata tokenIds
44:       ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40-L44

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

79        function mint(
80            address onBehalfOf,
81            DataTypes.ERC721SupplyParams[] calldata tokenData
82:       ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

87        function burn(
88            address from,
89            address receiverOfUnderlying,
90            uint256[] calldata tokenIds
91:       ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

119       function transferOnLiquidation(
120           address from,
121           address to,
122           uint256 value
123:      ) external onlyPool nonReentrant {

127       function rescueERC20(
128           address token,
129           address to,
130           uint256 amount
131:      ) external override onlyPoolAdmin {

136       function rescueERC721(
137           address token,
138           address to,
139           uint256[] calldata ids
140:      ) external override onlyPoolAdmin {

151       function rescueERC1155(
152           address token,
153           address to,
154           uint256[] calldata ids,
155           uint256[] calldata amounts,
156           bytes calldata data
157:      ) external override onlyPoolAdmin {

168       function executeAirdrop(
169           address airdropContract,
170           bytes calldata airdropParams
171:      ) external override onlyPoolAdmin {

199       function transferUnderlyingTo(address target, uint256 tokenId)
200           external
201           virtual
202           override
203           onlyPool
204:          nonReentrant

214       function handleRepayment(address user, uint256 amount)
215           external
216           virtual
217           override
218           onlyPool
219:          nonReentrant

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L79-L82

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

123       function decreaseUniswapV3Liquidity(
124           address user,
125           uint256 tokenId,
126           uint128 liquidityDecrease,
127           uint256 amount0Min,
128           uint256 amount1Min,
129           bool receiveEthAsWeth
130:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123-L130

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

202       function emergencyERC721TokenTransfer(
203           address token,
204           uint256 tokenId,
205           address to
206:      ) external onlyOwner {

217       function emergencyPunkTransfer(address to, uint256 punkIndex)
218           external
219:          onlyOwner

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L202-L206

### [G&#x2011;19]  Don't use `_msgSender()` if not supporting EIP-2771
Use `msg.sender` if the code does not implement [EIP-2771 trusted forwarder](https://eips.ethereum.org/EIPS/eip-2771) support

*There are 7 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

60:           require(_msgSender() == address(POOL), Errors.CALLER_MUST_BE_POOL);

196:              _msgSender() == owner || isApprovedForAll(owner, _msgSender()),

231:              _msgSender(),

260:              _isApprovedOrOwner(_msgSender(), tokenId),

297:              _isApprovedOrOwner(_msgSender(), tokenId),

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L60

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

99:                   _isApprovedOrOwner(_msgSender(), tokenIds[index]),

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L99







___

## Excluded findings
These findings are excluded from awards calculations because there are publicly-available automated tools that find them. The valid ones appear here for completeness

## Summary

### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|-|:-|:-:|:-:|
| [G&#x2011;01] | Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas | 34 | 4080 |
| [G&#x2011;02] | State variables should be cached in stack variables rather than re-reading them from storage | 1 | 97 |
| [G&#x2011;03] | `<array>.length` should not be looked up in every loop of a `for`-loop | 41 | 123 |
| [G&#x2011;04] | Using `bool`s for storage incurs overhead | 1 | 17100 |
| [G&#x2011;05] | Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement | 1 | 6 |
| [G&#x2011;06] | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 57 | 285 |
| [G&#x2011;07] | Using `private` rather than `public` for constants, saves gas | 8 | - |
| [G&#x2011;08] | Division by two should use bit shifting | 2 | 40 |
| [G&#x2011;09] | Use custom errors rather than `revert()`/`require()` strings to save gas | 15 | - |

Total: 160 instances over 9 issues with **21731 gas** saved

Gas totals use lower bounds of ranges and count two iterations of each `for`-loop. All values above are runtime, not deployment, values; deployment values are listed in the individual issue descriptions. The table above as well as its gas numbers do not include any of the excluded findings.





## Gas Optimizations

### [G&#x2011;01]  Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. **Each iteration of this for-loop costs at least 60 gas** (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having `memory` arguments, it's still valid for implementation contracs to use `calldata` arguments instead. 

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I've also flagged instances where the function is `public` but can be marked as `external` since it's not called by the contract, and cases where a constructor is involved

*There are 34 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

/// @audit params - (valid but excluded finding)
25        function getAskOrderInfo(bytes memory params, address weth)
26            external
27            pure
28            override
29:           returns (DataTypes.OrderInfo memory orderInfo)

/// @audit (valid but excluded finding)
67        function getBidOrderInfo(
68            bytes memory /*params*/
69:       ) external pure override returns (DataTypes.OrderInfo memory) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L25-L29

```solidity
File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

/// @audit params - (valid but excluded finding)
25        function getAskOrderInfo(bytes memory params, address)
26            external
27            pure
28            override
29:           returns (DataTypes.OrderInfo memory orderInfo)

/// @audit params - (valid but excluded finding)
60        function getBidOrderInfo(bytes memory params)
61            external
62            pure
63            override
64:           returns (DataTypes.OrderInfo memory orderInfo)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L25-L29

```solidity
File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

/// @audit params - (valid but excluded finding)
31        function getAskOrderInfo(bytes memory params, address)
32            external
33            pure
34            override
35:           returns (DataTypes.OrderInfo memory orderInfo)

/// @audit (valid but excluded finding)
79        function getBidOrderInfo(bytes memory)
80            external
81            pure
82            override
83:           returns (DataTypes.OrderInfo memory)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L31-L35

```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit _feeders - (valid but excluded finding)
/// @audit _assets - (valid but excluded finding)
97        function initialize(
98            address _admin,
99            address[] memory _feeders,
100           address[] memory _assets
101:      ) public initializer {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97-L101

```solidity
File: paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

/// @audit newMarketId - (valid but excluded finding)
56        function setMarketId(string memory newMarketId)
57            external
58            override
59:           onlyOwner

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56-L59

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol

/// @audit params - (valid but excluded finding)
41        function executeStartAuction(
42            mapping(address => DataTypes.ReserveData) storage reservesData,
43            mapping(uint256 => address) storage reservesList,
44            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
45:           DataTypes.ExecuteAuctionParams memory params

/// @audit params - (valid but excluded finding)
101       function executeEndAuction(
102           mapping(address => DataTypes.ReserveData) storage reservesData,
103           mapping(uint256 => address) storage reservesList,
104           mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
105:          DataTypes.ExecuteAuctionParams memory params

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L41-L45

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol

/// @audit params - (valid but excluded finding)
52        function executeBorrow(
53            mapping(address => DataTypes.ReserveData) storage reservesData,
54            mapping(uint256 => address) storage reservesList,
55            DataTypes.UserConfigurationMap storage userConfig,
56:           DataTypes.ExecuteBorrowParams memory params

/// @audit params - (valid but excluded finding)
127       function executeRepay(
128           mapping(address => DataTypes.ReserveData) storage reservesData,
129           DataTypes.UserConfigurationMap storage userConfig,
130           DataTypes.ExecuteRepayParams memory params
131:      ) external returns (uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L52-L56

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

/// @audit params - (valid but excluded finding)
28        function executeFlashClaim(
29            DataTypes.PoolStorage storage ps,
30:           DataTypes.ExecuteFlashClaimParams memory params

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L28-L30

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

/// @audit params - (valid but excluded finding)
161       function executeLiquidateERC20(
162           mapping(address => DataTypes.ReserveData) storage reservesData,
163           mapping(uint256 => address) storage reservesList,
164           mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
165           DataTypes.ExecuteLiquidateParams memory params
166:      ) external returns (uint256) {

/// @audit params - (valid but excluded finding)
286       function executeLiquidateERC721(
287           mapping(address => DataTypes.ReserveData) storage reservesData,
288           mapping(uint256 => address) storage reservesList,
289           mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
290           DataTypes.ExecuteLiquidateParams memory params
291:      ) external returns (uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L161-L166

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

/// @audit params - (valid but excluded finding)
39        function executeInitReserve(
40            mapping(address => DataTypes.ReserveData) storage reservesData,
41            mapping(uint256 => address) storage reservesList,
42            DataTypes.InitReserveParams memory params
43:       ) external returns (bool) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L39-L43

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

/// @audit params - (valid but excluded finding)
81        function executeSupply(
82            mapping(address => DataTypes.ReserveData) storage reservesData,
83            DataTypes.UserConfigurationMap storage userConfig,
84:           DataTypes.ExecuteSupplyParams memory params

/// @audit params - (valid but excluded finding)
167       function executeSupplyERC721(
168           mapping(address => DataTypes.ReserveData) storage reservesData,
169           DataTypes.UserConfigurationMap storage userConfig,
170:          DataTypes.ExecuteSupplyERC721Params memory params

/// @audit params - (valid but excluded finding)
228       function executeSupplyERC721FromNToken(
229           mapping(address => DataTypes.ReserveData) storage reservesData,
230           DataTypes.UserConfigurationMap storage userConfig,
231:          DataTypes.ExecuteSupplyERC721Params memory params

/// @audit params - (valid but excluded finding)
270       function executeWithdraw(
271           mapping(address => DataTypes.ReserveData) storage reservesData,
272           mapping(uint256 => address) storage reservesList,
273           DataTypes.UserConfigurationMap storage userConfig,
274           DataTypes.ExecuteWithdrawParams memory params
275:      ) external returns (uint256) {

/// @audit params - (valid but excluded finding)
335       function executeWithdrawERC721(
336           mapping(address => DataTypes.ReserveData) storage reservesData,
337           mapping(uint256 => address) storage reservesList,
338           DataTypes.UserConfigurationMap storage userConfig,
339           DataTypes.ExecuteWithdrawERC721Params memory params
340:      ) external returns (uint256) {

/// @audit params - (valid but excluded finding)
396       function executeDecreaseUniswapV3Liquidity(
397           mapping(address => DataTypes.ReserveData) storage reservesData,
398           mapping(uint256 => address) storage reservesList,
399           DataTypes.UserConfigurationMap storage userConfig,
400:          DataTypes.ExecuteDecreaseUniswapV3LiquidityParams memory params

/// @audit params - (valid but excluded finding)
462       function executeFinalizeTransferERC20(
463           mapping(address => DataTypes.ReserveData) storage reservesData,
464           mapping(uint256 => address) storage reservesList,
465           mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
466:          DataTypes.FinalizeTransferParams memory params

/// @audit params - (valid but excluded finding)
523       function executeFinalizeTransferERC721(
524           mapping(address => DataTypes.ReserveData) storage reservesData,
525           mapping(uint256 => address) storage reservesList,
526           mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
527:          DataTypes.FinalizeTransferERC721Params memory params

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L81-L84

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

/// @audit _nftPairs - (valid but excluded finding)
120       function withdrawBAKC(
121           address nftAsset,
122           ApeCoinStaking.PairNftWithAmount[] memory _nftPairs
123:      ) external nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L120-L123

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

/// @audit (valid but excluded finding)
793       function onERC721Received(
794           address,
795           address,
796           uint256,
797           bytes memory
798:      ) external virtual returns (bytes4) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L793-L798

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit _data - (valid but excluded finding)
281       function safeTransferFrom(
282           address from,
283           address to,
284           uint256 tokenId,
285           bytes memory _data
286:      ) external virtual override nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L281-L286

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit _nftPairs - (valid but excluded finding)
38        function withdrawBAKC(
39            ApeCoinStaking _apeCoinStaking,
40            uint256 poolId,
41            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
42:           address _apeRecipient

/// @audit params - (valid but excluded finding)
92        function executeUnstakePositionAndRepay(
93            mapping(uint256 => address) storage _owners,
94            APEStakingParameter storage stakingParameter,
95:           UnstakeAndRepayParams memory params

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L38-L42

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

/// @audit _nftPairs - (valid but excluded finding)
97        function withdrawBAKC(
98            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99            address _apeRecipient
100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97-L100

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

/// @audit _nftPairs - (valid but excluded finding)
97        function withdrawBAKC(
98            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99            address _apeRecipient
100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-L100

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

/// @audit (valid but excluded finding)
63        function onERC721Received(
64            address operator,
65            address from,
66            uint256 id,
67            bytes memory
68:       ) external virtual override returns (bytes4) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L63-L68

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit (valid but excluded finding)
271       function onERC721Received(
272           address,
273           address,
274           uint256,
275           bytes memory
276:      ) external virtual override returns (bytes4) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L271-L276

### [G&#x2011;02]  State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (**100 gas**) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit assetPriceMap[_asset].twap on line 405 - (valid but excluded finding)
426:              return (false, assetPriceMap[_asset].twap);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L426

### [G&#x2011;03]  `<array>.length` should not be looked up in every loop of a `for`-loop
The overheads outlined below are _PER LOOP_, excluding the first loop
* storage arrays incur a Gwarmaccess (**100 gas**)
* memory arrays use `MLOAD` (**3 gas**)
* calldata arrays use `CALLDATALOAD` (**3 gas**)

Caching the length changes each of these to a `DUP<N>` (**3 gas**), and gets rid of the extra `DUP<N>` needed to store the stack offset

*There are 41 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
229:          for (uint256 i = 0; i < _assets.length; i++) {

/// @audit (valid but excluded finding)
291:          for (uint256 i = 0; i < _assets.length; i++) {

/// @audit (valid but excluded finding)
321:          for (uint256 i = 0; i < _feeders.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

/// @audit (valid but excluded finding)
95:           for (uint256 i = 0; i < assets.length; i++) {

/// @audit (valid but excluded finding)
197:          for (uint256 i = 0; i < assets.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

/// @audit (valid but excluded finding)
193:          for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
210:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

/// @audit (valid but excluded finding)
38:           for (i = 0; i < params.nftTokenIds.length; i++) {

/// @audit (valid but excluded finding)
56:           for (i = 0; i < params.nftTokenIds.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

/// @audit (valid but excluded finding)
177:          for (uint256 i = 0; i < marketplaceIds.length; i++) {

/// @audit (valid but excluded finding)
284:          for (uint256 i = 0; i < marketplaceIds.length; i++) {

/// @audit (valid but excluded finding)
382:          for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

/// @audit (valid but excluded finding)
470:          for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

/// @audit (valid but excluded finding)
104:          for (uint256 i = 0; i < assets.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

/// @audit (valid but excluded finding)
184:              for (uint256 index = 0; index < params.tokenData.length; index++) {

/// @audit (valid but excluded finding)
195:          for (uint256 index = 0; index < params.tokenData.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

/// @audit (valid but excluded finding)
212:              for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
465:              for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
910:                  for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
1034:         for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

/// @audit (valid but excluded finding)
72:           for (uint256 index = 0; index < _nfts.length; index++) {

/// @audit (valid but excluded finding)
103:          for (uint256 index = 0; index < _nfts.length; index++) {

/// @audit (valid but excluded finding)
138:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
199:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
215:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
279:          for (uint256 index = 0; index < _nfts.length; index++) {

/// @audit (valid but excluded finding)
289:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
305:          for (uint256 index = 0; index < _nftPairs.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit (valid but excluded finding)
493:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit (valid but excluded finding)
207:          for (uint256 index = 0; index < tokenData.length; index++) {

/// @audit (valid but excluded finding)
280:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit (valid but excluded finding)
107:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

/// @audit (valid but excluded finding)
51:               for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
97:           for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit (valid but excluded finding)
106:              for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
145:          for (uint256 i = 0; i < ids.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

/// @audit (valid but excluded finding)
82:           for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
109:          for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
113:          for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
136:          for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
174:          for (uint256 i = 0; i < punkIndexes.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82

### [G&#x2011;04]  Using `bool`s for storage incurs overhead
```solidity
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27
Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (**[100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from `false` to `true`, after having been `true` in the past

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit (valid but excluded finding)
73:       bool public immutable ATOMIC_PRICING;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L73

### [G&#x2011;05]  Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement
This change saves **[6 gas](https://aws1.discourse-cdn.com/business6/uploads/zeppelin/original/2X/3/363a367d6d68851f27d2679d10706cd16d788b96.png)** per instance. The optimization works until solidity version [0.8.13](https://gist.github.com/IllIllI000/bf2c3120f24a69e489f12b3213c06c94) where there is a regression in gas costs.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
356:          require(_twap > 0, "NFTOracle: price should be more than 0");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L356

### [G&#x2011;06]  `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
Saves **5 gas per loop**

*There are 57 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
229:          for (uint256 i = 0; i < _assets.length; i++) {

/// @audit (valid but excluded finding)
291:          for (uint256 i = 0; i < _assets.length; i++) {

/// @audit (valid but excluded finding)
321:          for (uint256 i = 0; i < _feeders.length; i++) {

/// @audit (valid but excluded finding)
413:          for (uint256 i = 0; i < feederSize; i++) {

/// @audit (valid but excluded finding)
421:                      validNum++;

/// @audit (valid but excluded finding)
449:                  i++;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

/// @audit (valid but excluded finding)
95:           for (uint256 i = 0; i < assets.length; i++) {

/// @audit (valid but excluded finding)
197:          for (uint256 i = 0; i < assets.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

/// @audit (valid but excluded finding)
193:          for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
210:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

/// @audit (valid but excluded finding)
38:           for (i = 0; i < params.nftTokenIds.length; i++) {

/// @audit (valid but excluded finding)
56:           for (i = 0; i < params.nftTokenIds.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

/// @audit (valid but excluded finding)
371:              for (uint256 index = 0; index < totalBalance; index++) {

/// @audit (valid but excluded finding)
466:          for (uint256 index = 0; index < totalBalance; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

/// @audit (valid but excluded finding)
177:          for (uint256 i = 0; i < marketplaceIds.length; i++) {

/// @audit (valid but excluded finding)
284:          for (uint256 i = 0; i < marketplaceIds.length; i++) {

/// @audit (valid but excluded finding)
382:          for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

/// @audit (valid but excluded finding)
470:          for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

/// @audit (valid but excluded finding)
58:           for (uint16 i = 0; i < params.reservesCount; i++) {

/// @audit (valid but excluded finding)
104:          for (uint256 i = 0; i < assets.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

/// @audit (valid but excluded finding)
184:              for (uint256 index = 0; index < params.tokenData.length; index++) {

/// @audit (valid but excluded finding)
195:          for (uint256 index = 0; index < params.tokenData.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

/// @audit (valid but excluded finding)
131:          for (uint256 index = 0; index < amount; index++) {

/// @audit (valid but excluded finding)
212:              for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
465:              for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
910:                  for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
1034:         for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

/// @audit (valid but excluded finding)
72:           for (uint256 index = 0; index < _nfts.length; index++) {

/// @audit (valid but excluded finding)
103:          for (uint256 index = 0; index < _nfts.length; index++) {

/// @audit (valid but excluded finding)
138:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
164:                  actualTransferAmount++;

/// @audit (valid but excluded finding)
172:          for (uint256 index = 0; index < actualTransferAmount; index++) {

/// @audit (valid but excluded finding)
199:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
215:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
279:          for (uint256 index = 0; index < _nfts.length; index++) {

/// @audit (valid but excluded finding)
289:          for (uint256 index = 0; index < _nftPairs.length; index++) {

/// @audit (valid but excluded finding)
305:          for (uint256 index = 0; index < _nftPairs.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

/// @audit (valid but excluded finding)
634:          for (uint256 i = 0; i < reservesListCount; i++) {

/// @audit (valid but excluded finding)
638:                  droppedReservesCount++;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

/// @audit (valid but excluded finding)
134:              ps._reservesCount++;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L134

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit (valid but excluded finding)
493:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit (valid but excluded finding)
213:          for (uint256 index = 0; index < totalBalance; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit (valid but excluded finding)
207:          for (uint256 index = 0; index < tokenData.length; index++) {

/// @audit (valid but excluded finding)
234:                  collateralizedTokens++;

/// @audit (valid but excluded finding)
280:          for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
304:              balanceToBurn++;

/// @audit (valid but excluded finding)
313:                  burntCollateralizedTokens++;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit (valid but excluded finding)
107:          for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

/// @audit (valid but excluded finding)
51:               for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
97:           for (uint256 index = 0; index < tokenIds.length; index++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit (valid but excluded finding)
106:              for (uint256 index = 0; index < tokenIds.length; index++) {

/// @audit (valid but excluded finding)
145:          for (uint256 i = 0; i < ids.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

/// @audit (valid but excluded finding)
82:           for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
109:          for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
113:          for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
136:          for (uint256 i = 0; i < punkIndexes.length; i++) {

/// @audit (valid but excluded finding)
174:          for (uint256 i = 0; i < punkIndexes.length; i++) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82

### [G&#x2011;07]  Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*There are 8 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
70:       bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L70

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

/// @audit (valid but excluded finding)
57:       uint256 public constant MAX_LIQUIDATION_CLOSE_FACTOR = 1e4;

/// @audit (valid but excluded finding)
64:       uint256 public constant CLOSE_FACTOR_HF_THRESHOLD = 0.95e18;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L57

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

/// @audit (valid but excluded finding)
45:       uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;

/// @audit (valid but excluded finding)
49        uint256 public constant MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD =
50:           0.95e18;

/// @audit (valid but excluded finding)
56:       uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

/// @audit (valid but excluded finding)
53:       uint256 public constant POOL_REVISION = 1;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L53

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit (valid but excluded finding)
32:       uint256 public constant NTOKEN_REVISION = 0x1;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L32

### [G&#x2011;08]  Division by two should use bit shifting
`<x> / 2` is the same as `<x> >> 1`. While the compiler uses the `SHR` opcode to accomplish both, the version that uses division incurs an overhead of [**20 gas**](https://gist.github.com/IllIllI000/ec0e4e6c4f52a6bca158f137a3afd4ff) due to `JUMP`s to and from a compiler utility function that introduces checks which can be avoided by using `unchecked {}` around the division by two

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
429:          return (true, validPriceList[validNum / 2]);

/// @audit (valid but excluded finding)
440:          uint256 pivot = arr[uint256(left + (right - left) / 2)];

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L429

### [G&#x2011;09]  Use custom errors rather than `revert()`/`require()` strings to save gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

*There are 15 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
118:          require(_isAssetExisted(_asset), "NFTOracle: asset not existed");

/// @audit (valid but excluded finding)
123:          require(!_isAssetExisted(_asset), "NFTOracle: asset existed");

/// @audit (valid but excluded finding)
128:          require(_isFeederExisted(_feeder), "NFTOracle: feeder not existed");

/// @audit (valid but excluded finding)
133:          require(!_isFeederExisted(_feeder), "NFTOracle: feeder existed");

/// @audit (valid but excluded finding)
207:          require(dataValidity, "NFTOracle: invalid price data");

/// @audit (valid but excluded finding)
267:          require(!_paused, "NFTOracle: nft price feed paused");

/// @audit (valid but excluded finding)
356:          require(_twap > 0, "NFTOracle: price should be more than 0");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L118

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit (valid but excluded finding)
193:          require(to != owner, "ERC721: approval to old owner");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L193

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit (valid but excluded finding)
72:           require(oldValue != incentive, "Same Value");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L72

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit (valid but excluded finding)
92:           require(to != address(0), "ERC721: transfer to the zero address");

/// @audit (valid but excluded finding)
164:          require(owner == sender, "not owner");

/// @audit (valid but excluded finding)
199:          require(to != address(0), "ERC721: mint to the zero address");

/// @audit (valid but excluded finding)
283:              require(owner == user, "not the owner of Ntoken");

/// @audit (valid but excluded finding)
363:          require(owner != operator, "ERC721: approve to caller");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

/// @audit (valid but excluded finding)
146:          require(success, "ETH_TRANSFER_FAILED");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L146


