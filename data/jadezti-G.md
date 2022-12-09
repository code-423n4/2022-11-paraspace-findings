
|         | Issue                                                                                  | Instances |
| ------- |:-------------------------------------------------------------------------------------- |:---------:|
| [GAS-1] | Using `unchecked` math operations when no overflow/underflow           |    45     |
| [GAS-2] | Using Immutable instead of Constant for keccak expressions                                      |     2     |
| [GAS-3] | Using named variables in returns saves gas                                                  |     7     | 



### [GAS-1] Using `unchecked` math operations when no overflow/underflow 

*Instances (45)*:


```js
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

442:         while (arr[uint256(i)] < pivot) i++;

443:         while (pivot < arr[uint256(j)]) j--;

450:         j++;
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)


```js
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

638:   droppedReservesCount++;
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol)

In addition to for-loop optimizations reported in [C4Audit Report](https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba), such as caching array length, without initializing variable with default value, and replacing `i++` with `++i`, for-loop can be further optimized by applying `uncheck` math operation on the index increment (`unchecked {++i;}`) to save more gas. The final optimized foo-loop is like:
```js
for (uint i; i<length;){
  ...
  unchecked {++i;}
}
```

for-loop instances in the smart contracts:
```js
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38:         for (i = 0; i < params.nftTokenIds.length; i++) {

56:        for (i = 0; i < params.nftTokenIds.length; i++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol)

```js
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

229:         for (uint256 i = 0; i < _assets.length; i++) {

291:         for (uint256 i = 0; i < _assets.length; i++) {

321:         for (uint256 i = 0; i < _feeders.length; i++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```js
File:  paraspace-core/contracts/misc/ParaSpaceOracle.sol

95:        for (uint256 i = 0; i < assets.length; i++) {

197:      for (uint256 i = 0; i < assets.length; i++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

```js
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:         for (uint256 i = 0; i < marketplaceIds.length; i++) {

284:         for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:         for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:         for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol)

```js
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

104:       for (uint256 i = 0; i < assets.length; i++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol)

```js
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:         for (uint256 index = 0; index < params.tokenData.length; index++) {

195:         for (uint256 index = 0; index < params.tokenData.length; index++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol)

```js
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:      for (uint256 index = 0; index < tokenIds.length; index++) {

210:        for (uint256 index = 0; index < tokenIds.length; index++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

```js
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

212:             for (uint256 index = 0; index < tokenIds.length; index++) {

465:             for (uint256 index = 0; index < tokenIds.length; index++) {

910:          for (uint256 index = 0; index < tokenIds.length; index++) {

1034:         for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol)

```js
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72:         for (uint256 index = 0; index < _nfts.length; index++) {

103:         for (uint256 index = 0; index < _nfts.length; index++) {

138:        for (uint256 index = 0; index < _nftPairs.length; index++) {

199:        for (uint256 index = 0; index < _nftPairs.length; index++) {

215:         for (uint256 index = 0; index < _nftPairs.length; index++) {

279:         for (uint256 index = 0; index < _nfts.length; index++) {

289:        for (uint256 index = 0; index < _nftPairs.length; index++) {

305:         for (uint256 index = 0; index < _nftPairs.length; index++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol)

```js
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

106:           for (uint256 index = 0; index < tokenIds.length; index++) {

145:        for (uint256 i = 0; i < ids.length; i++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol)

```js
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

107:         for (uint256 index = 0; index < tokenIds.length; index++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol)

```js
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:            for (uint256 index = 0; index < tokenIds.length; index++) {

97:         for (uint256 index = 0; index < tokenIds.length; index++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol)

```js
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493:       for (uint256 index = 0; index < tokenIds.length; index++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol)

```js
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:        for (uint256 index = 0; index < tokenData.length; index++) {

280:        for (uint256 index = 0; index < tokenIds.length; index++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol)

```js
File: paraspace-core/contracts/ui/WPunkGateway.sol

82:         for (uint256 i = 0; i < punkIndexes.length; i++) {

109:         for (uint256 i = 0; i < punkIndexes.length; i++) {

113:         for (uint256 i = 0; i < punkIndexes.length; i++) {

136:       for (uint256 i = 0; i < punkIndexes.length; i++) {

174:       for (uint256 i = 0; i < punkIndexes.length; i++) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol)




### [GAS-2] Using Immutable instead of Constant for keccak expressions

As reported, changing `constant` to `immutable` with keccak256 expressions will save gas.

*Instances (2)*:

```js
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

23:    bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
24:          bytes32(
25:              uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
26:          );
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23-L26)

```js
File:  paraspace-core/contracts/protocol/pool/PoolStorage.sol

16:     bytes32 constant POOL_STORAGE_POSITION =
17:         bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16-L17)

### [GAS-3] Using named variables in returns saves gas

*Instances (7)*:

```js
File:    paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

189:    returns (uint256[] memory)

224:    returns (PairOracleData memory)
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

```js
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

379:     ) internal returns (uint256, uint256) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol)

```js
File: paraspace-core/contracts/misc/ProtocolDataProvider.sol

43:    returns (DataTypes.TokenData[] memory)


84:    returns (DataTypes.TokenData[] memory)

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ProtocolDataProvider.sol)

```js
File: paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol

316:    returns (DataTypes.ReserveCache memory)

```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol#L316)


```js
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

709:    ) internal view returns (uint256, bool) {
```
[Link to code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L709)
