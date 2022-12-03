## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|Estimated Gas Saved|
|-|:-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 1 | - |
| [GAS&#x2011;2](#GAS&#x2011;2) | Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function | 40 | 1120 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Empty Blocks Should Be Removed Or Emit Something | 2 | - |
| [GAS&#x2011;4](#GAS&#x2011;4) | `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables | 26 | - |
| [GAS&#x2011;5](#GAS&#x2011;5) | `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops | 49 | 1715 |
| [GAS&#x2011;6](#GAS&#x2011;6) | `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas | 18 | - |
| [GAS&#x2011;7](#GAS&#x2011;7) | `abi.encode()` is less efficient than `abi.encodepacked()` | 2 | 200 |
| [GAS&#x2011;8](#GAS&#x2011;8) | Using `10**X` for constants isn't gas efficient  | 1 | - |
| [GAS&#x2011;9](#GAS&#x2011;9) | Public Functions To External | 16 | - |
| [GAS&#x2011;10](#GAS&#x2011;10) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 26 | - |
| [GAS&#x2011;11](#GAS&#x2011;11) | Optimize names to save gas | 15 | 330 |
| [GAS&#x2011;12](#GAS&#x2011;12) | Use immutable `weth` variable value | 1 | - |
| [GAS&#x2011;13](#GAS&#x2011;13) | State variables only set in the `constructor` should be declared immutable | 1 | - |
| [GAS&#x2011;14](#GAS&#x2011;14) | `internal` functions only called once can be inlined to save gas | 23 | - |
| [GAS&#x2011;15](#GAS&#x2011;15) | Setting the `constructor` to `payable` | 19 | 247 |
| [GAS&#x2011;16](#GAS&#x2011;16) | Functions guaranteed to revert when called by normal users can be marked `payable` | 65 | 1365 |
| [GAS&#x2011;17](#GAS&#x2011;17) | Using `unchecked` blocks to save gas | 2 | 272 |
| [GAS&#x2011;18](#GAS&#x2011;18) | Structs can be packed into fewer storage slots | 1 | - |

Total: 307 contexts over 17 issues

## Gas Optimizations


### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```solidity
74: mapping(address => PriceInformation) public assetPriceMap;
81: mapping(address => FeederPosition) private feederPositionMap;
88: mapping(address => FeederRegistrar) public assetFeederMap;
```

Can be changed to:
```solidity
    struct priceFeederStruct {
        PriceInformation assetPriceMap;
        FeederPosition feederPositionMap;
        FeederRegistrar assetFeederMap;
    }
    
    mapping(address => priceFeederStruct) public priceFeederInfo;
```


https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L40

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L74

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L81

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L88


#### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function

Saves deployment costs

#### <ins>Proof Of Concept</ins>

```solidity
51: (orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
84: (orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L51

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84



```solidity
68: (amount != 0, Errors.INVALID_AMOUNT);
160: (amount != 0, Errors.INVALID_AMOUNT);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L68

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L160

```solidity
85: (isActive, Errors.RESERVE_INACTIVE);
185: (isActive, Errors.RESERVE_INACTIVE);
207: (isActive, Errors.RESERVE_INACTIVE);
390: (isActive, Errors.RESERVE_INACTIVE);
438: (isActive, Errors.RESERVE_INACTIVE);
460: (isActive, Errors.RESERVE_INACTIVE);
1029: (isActive, Errors.RESERVE_INACTIVE);
1057: (isActive, Errors.RESERVE_INACTIVE);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L85

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L185

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L207

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L390

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L438

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L460

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1029

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1057

```solidity
86: (!isPaused, Errors.RESERVE_PAUSED);
186: (!isPaused, Errors.RESERVE_PAUSED);
208: (!isPaused, Errors.RESERVE_PAUSED);
391: (!isPaused, Errors.RESERVE_PAUSED);
439: (!isPaused, Errors.RESERVE_PAUSED);
461: (!isPaused, Errors.RESERVE_PAUSED);
1030: (!isPaused, Errors.RESERVE_PAUSED);
1058: (!isPaused, Errors.RESERVE_PAUSED);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L86

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L186

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L208

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L391

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L439

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L461

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1030

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1058

```solidity
768: (vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
824: (vars.collateralReserveActive, Errors.RESERVE_INACTIVE);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L768

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L824

```solidity
769: (!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
825: (!vars.collateralReservePaused, Errors.RESERVE_PAUSED);
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L769

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L825

```solidity
937: (!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);
950: (!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L937

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L950

```solidity
1068: (!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);
1074: (!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1068

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1074

```solidity
157: (asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
172: (asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
187: (asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187





### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Empty Blocks Should Be Removed Or Emit Something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})

#### <ins>Proof Of Concept</ins>

```solidity
50: {}
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L50

```solidity
149: receive() external payable {}
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149






### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

#### <ins>Proof Of Concept</ins>


```solidity
149: token0Amount += positionData.tokensOwed0;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L149

```solidity
150: token1Amount += positionData.tokensOwed1;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L150

```solidity
211: sum += getTokenPrice(tokenIds[index]);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L211

```solidity
169: vars.payableDebtByERC20Assets += vars
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L169

```solidity
176: vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L176

```solidity
178: vars.totalCollateralInBaseCurrency += vars
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L178

```solidity
185: vars.avgLiquidationThreshold += vars.liquidationThreshold;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L185

```solidity
189: vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L189

```solidity
231: vars.avgERC721LiquidationThreshold += vars
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L231

```solidity
233: vars.totalERC721CollateralInBaseCurrency += vars
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L233

```solidity
237: vars.avgLtv += vars.ltv;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L237

```solidity
380: totalValue += _getTokenPrice(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L380

```solidity
479: totalValue += tokenPrice;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L479

```solidity
496: totalLTV += tmpLTV * tokenPrice;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L496

```solidity
497: totalLiquidationThreshold +=
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L497

```solidity
83: vars.ethLeft -= _buyWithCredit(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L83

```solidity
205: vars.ethLeft -= _buyWithCredit(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L205

```solidity
397: price += item.startAmount;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L397

```solidity
77: amountToWithdraw += _nfts[index].amount;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L77

```solidity
166: amountToWithdraw += _nftPairs[index].amount;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L166

```solidity
215: totalAmount += getTokenIdStakingAmount(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L215

```solidity
257: apeStakedAmount += bakcStakedAmount;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L257

```solidity
146: erc721Data.userState[from].collateralizedBalance -= 1;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L146

```solidity
318: erc721Data.userState[user].balance -= balanceToBurn;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L318





### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

#### <ins>Proof Of Concept</ins>


```solidity
229: for (uint256 i = 0; i < _assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229

```solidity
291: for (uint256 i = 0; i < _assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291

```solidity
321: for (uint256 i = 0; i < _feeders.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321

```solidity
413: for (uint256 i = 0; i < feederSize; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413

```solidity
95: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95

```solidity
197: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197

```solidity
193: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193

```solidity
210: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210

```solidity
38: for (i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38

```solidity
371: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371

```solidity
466: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466

```solidity
177: for (uint256 i = 0; i < marketplaceIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177

```solidity
284: for (uint256 i = 0; i < marketplaceIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284

```solidity
382: for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382

```solidity
470: for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470

```solidity
58: for (uint16 i = 0; i < params.reservesCount; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58

```solidity
104: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104

```solidity
184: for (uint256 index = 0; index < params.tokenData.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184

```solidity
131: for (uint256 index = 0; index < amount; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131

```solidity
212: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212

```solidity
465: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465

```solidity
910: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910

```solidity
1034: for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034

```solidity
72: for (uint256 index = 0; index < _nfts.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72

```solidity
103: for (uint256 index = 0; index < _nfts.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103

```solidity
138: for (uint256 index = 0; index < _nftPairs.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138

```solidity
172: for (uint256 index = 0; index < actualTransferAmount; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172

```solidity
199: for (uint256 index = 0; index < _nftPairs.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199

```solidity
279: for (uint256 index = 0; index < _nfts.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279

```solidity
289: for (uint256 index = 0; index < _nftPairs.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289

```solidity
634: for (uint256 i = 0; i < reservesListCount; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634

```solidity
106: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106

```solidity
145: for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145

```solidity
107: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107

```solidity
51: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51

```solidity
97: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97

```solidity
493: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493

```solidity
213: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213

```solidity
207: for (uint256 index = 0; index < tokenData.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207

```solidity
280: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280

```solidity
82: for (uint256 i = 0; i < punkIndexes.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82

```solidity
109: for (uint256 i = 0; i < punkIndexes.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109

```solidity
136: for (uint256 i = 0; i < punkIndexes.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136

```solidity
174: for (uint256 i = 0; i < punkIndexes.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174





### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas

#### <ins>Proof Of Concept</ins>


```solidity
118: require(_isAssetExisted(_asset), "NFTOracle: asset not existed");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L118

```solidity
128: require(_isFeederExisted(_feeder), "NFTOracle: feeder not existed");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L128

```solidity
207: require(dataValidity, "NFTOracle: invalid price data");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L207

```solidity
267: require(!_paused, "NFTOracle: nft price feed paused");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L267

```solidity
356: require(_twap > 0, "NFTOracle: price should be more than 0");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L356

```solidity
51: require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L51

```solidity
84: require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84

```solidity
38: require(runInput.details.length == 1, Errors.INVALID_MARKETPLACE_ORDER);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L38

```solidity
51: require(nfts.length == 1, Errors.INVALID_MARKETPLACE_ORDER);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L51

```solidity
86: require(id != POOL, Errors.INVALID_ADDRESSES_PROVIDER_ID);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L86

```solidity
406: require((variableDebt != 0), Errors.NO_DEBT_OF_SELECTED_TYPE);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L406

```solidity
418: require(userBalance != 0, Errors.UNDERLYING_BALANCE_ZERO);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L418

```solidity
60: require(initializingPool == POOL, Errors.POOL_ADDRESSES_DO_NOT_MATCH);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L60

```solidity
176: require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L176

```solidity
70: require(msg.sender == _underlyingAsset, Errors.OPERATION_NOT_SUPPORTED);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L70

```solidity
193: require(to != owner, "ERC721: approval to old owner");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L193

```solidity
92: require(to != address(0), "ERC721: transfer to the zero address");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92

```solidity
199: require(to != address(0), "ERC721: mint to the zero address");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L199







### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> `abi.encode()` is less efficient than `abi.encodepacked()`

See for more information: https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison 

#### <ins>Proof Of Concept</ins>


```solidity
1124: abi.encode(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1124

```solidity
1136: abi.encode(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1136




### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> Using `10**X` for constants isn't gas efficient 

In Solidity, a constant expression in a variable will compute the expression everytime the variable is called. It's not the result of the expression that is stored, but the expression itself.

As Solidity supports the scientific notation, constants of form 10**X can be rewritten as 1eX to save the gas cost from the calculation with the exponentiation operator **.

#### <ins>Proof Of Concept</ins>


```solidity
245: ((oracleData.token0Price * (10**18)) /
```

Should be change to:
```solidity
245: ((oracleData.token0Price * (1e18)) /
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245



#### <ins>Recommended Mitigation Steps</ins>

Replace 10**X with 1eX



### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```solidity
function initialize(
        address _admin,
        address[] memory _feeders,
        address[] memory _assets
    ) public initializer {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97

```solidity
function getFeederSize() public view returns (uint256) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L261

```solidity
function getLiquidityAmountFromPositionData(
        UinswapV3PositionData memory positionData
    ) public pure returns (uint256 token0Amount, uint256 token1Amount) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L113

```solidity
function getLpFeeAmountFromPositionData(
        UinswapV3PositionData memory positionData
    ) public view returns (uint256 token0Amount, uint256 token1Amount) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L144

```solidity
function getTokenPrice(uint256 tokenId) public view returns (uint256) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L156

```solidity
function getAddress(bytes32 id) public view override returns (address) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L65

```solidity
function executeBorrow(
        mapping(address => DataTypes.ReserveData) storage reservesData,
        mapping(uint256 => address) storage reservesList,
        DataTypes.UserConfigurationMap storage userConfig,
        DataTypes.ExecuteBorrowParams memory params
    ) public {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L52

```solidity
function initialize(
        IPool initializingPool,
        address underlyingAsset,
        IRewardController incentivesController,
        string calldata nTokenName,
        string calldata nTokenSymbol,
        bytes calldata params
    ) public virtual override initializer {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L52

```solidity
function initialize(
        IPool initializingPool,
        address underlyingAsset,
        IRewardController incentivesController,
        string calldata nTokenName,
        string calldata nTokenSymbol,
        bytes calldata params
    ) public virtual override initializer {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L36

```solidity
function getBAKC() public view returns (IERC721) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L64

```solidity
function name() public view override returns (string memory) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L96

```solidity
function totalSupply() public view virtual override returns (uint256) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L604

```solidity
function getTokenIdStakingAmount(
        uint256 poolId,
        ApeCoinStaking _apeCoinStaking,
        uint256 tokenId
    ) public view returns (uint256) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L231

```solidity
function executeTransfer(
        MintableERC721Data storage erc721Data,
        IPool POOL,
        bool ATOMIC_PRICING,
        address from,
        address to,
        uint256 tokenId
    ) public {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L80

```solidity
function isAuctioned(
        MintableERC721Data storage erc721Data,
        IPool POOL,
        uint256 tokenId
    ) public view returns (bool) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L424

```solidity
function onERC721Received(
        address,
        address,
        uint256,
        bytes memory
    ) public virtual override returns (bytes4) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L232






### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```solidity
18: uint128 expirationPeriod;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L18

```solidity
20: uint128 maxPriceDeviation;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L20

```solidity
47: uint8 index;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L47

```solidity
300: uint8 assetIndex = assetFeederMap[_asset].index;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L300

```solidity
330: uint8 feederIndex = feederPositionMap[_feeder].index;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L330

```solidity
43: uint8 token0Decimal;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L43

```solidity
44: uint8 token1Decimal;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L44

```solidity
45: uint160 sqrtPriceX96;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L45

```solidity
125: uint16 liquidationAssetReserveId;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L125

```solidity
581: interfaceId == type(IERC721Metadata).interfaceId;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L581

```solidity
13: uint64 balance;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L13

```solidity
14: uint64 collateralizedBalance;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L14

```solidity
15: uint128 additionalData;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L15

```solidity
42: uint64 balanceLimit;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L42

```solidity
103: uint64 oldSenderBalance = erc721Data.userState[from].balance;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L103

```solidity
105: uint64 oldRecipientBalance = erc721Data.userState[to].balance;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L105

```solidity
106: uint64 newRecipientBalance = oldRecipientBalance + 1;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L106

```solidity
200: uint64 oldBalance = erc721Data.userState[to].balance;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L200

```solidity
205: uint64 collateralizedTokens = 0;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L205

```solidity
247: uint64 newBalance = oldBalance + uint64(tokenData.length);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L247

```solidity
272: uint64 burntCollateralizedTokens = 0;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272

```solidity
273: uint64 balanceToBurn;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L273

```solidity
408: uint64 balanceLimit = erc721Data.balanceLimit;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L408




### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

See more <a href="https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92">here</a>


#### <ins>Proof Of Concept</ins>

Relevant to all in-scope contracts, read more about it <a href="https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92">here</a>


#### <ins>Recommended Mitigation Steps</ins>
Find a lower method ID name for the most called functions for example Call() vs. Call1() is cheaper by 22 gas
For example, the function IDs in the Gauge.sol contract will be the most used; A lower method ID may be given.





### <a href="#Summary">[GAS&#x2011;12]</a><a name="GAS&#x2011;12"> Use immutable `weth` variable value

It is cheaper to use `weth` variable as immutable and store the value of WETH address. It saves one storage read and make the code clearer.

#### <ins>Proof Of Concept</ins>


```solidity
92: address weth = _addressesProvider.getWETH();
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L92






### <a href="#Summary">[GAS&#x2011;13]</a><a name="GAS&#x2011;13"> State variables only set in the `constructor` should be declared immutable

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

#### <ins>Proof Of Concept</ins>


```solidity
65: _underlyingAsset = underlyingAsset
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L65





### <a href="#Summary">[GAS&#x2011;14]</a><a name="GAS&#x2011;14"> `internal` functions only called once can be inlined to save gas

#### <ins>Proof Of Concept</ins>

```solidity
265: function _whenNotPaused

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L265

```solidity
388: function _addRawValue

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L388

```solidity
218: function _onlyAssetListingOrPoolAdmins

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L218

```solidity
302: function _updateParaProxyImpl

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L302




```solidity
435: function _burnCollateralPTokens

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L435

```solidity
465: function _burnCollateralNTokens

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L465

```solidity
488: function _liquidatePTokens

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L488

```solidity
523: function _burnDebtTokens

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L523

```solidity
564: function _supplyNewCollateral

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L564

```solidity
605: function _calculateDebt

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L605

```solidity
376: function _delegateToPool

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L376

```solidity
552: function _checkAllowance

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L552

```solidity
593: function _transferAndCollateralize

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L593


```solidity
118: function validateSupplyFromNToken

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L118


```solidity
740: function validateStartAuction

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L740

```solidity
801: function validateEndAuction

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L801

```solidity
944: function validateTransferERC721

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L944

```solidity
1133: function getDomainSeparator

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1133

```solidity
413: function setSApeUseAsCollateral

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413

```solidity
69: function _onlyPoolConfigurator

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69

```solidity
76: function _onlyPoolAdmin

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L76

```solidity
130: function initializeStakingData

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L130

```solidity
51: function _decreaseLiquidity

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L51




### <a href="#Summary">[GAS&#x2011;15]</a><a name="GAS&#x2011;15"> Setting the `constructor` to `payable`

Saves ~13 gas per instance

#### <ins>Proof Of Concept</ins>

```solidity
49: constructor(
        IPoolAddressesProvider provider,
        address[] memory assets,
        address[] memory sources,
        address fallbackOracle,
        address baseCurrency,
        uint256 baseCurrencyUnit
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L49

```solidity
23: constructor(
        address _factory,
        address _manager,
        address _addressProvider
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23

```solidity
45: constructor(string memory marketId, address owner)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L45

```solidity
50: constructor(
        uint256 maxPriceMultiplier,
        uint256 minExpPriceMultiplier,
        uint256 minPriceMultiplier,
        uint256 stepLinear,
        uint256 stepExp,
        uint256 tickLength
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L50

```solidity
51: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L51

```solidity
64: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L64

```solidity
62: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L62

```solidity
89: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L89

```solidity
43: constructor(IPool pool, bool atomic_pricing)
        MintableIncentivizedERC721(
            pool,
            "NTOKEN_IMPL",
            "NTOKEN_IMPL",
            atomic_pricing
        )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L43

```solidity
32: constructor(IPool pool, address apeCoinStaking) NToken(pool, false)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L32

```solidity
16: constructor(IPool pool, address apeCoinStaking)
        NTokenApeStaking(pool, apeCoinStaking)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L16

```solidity
16: constructor(IPool pool, address apeCoinStaking)
        NTokenApeStaking(pool, apeCoinStaking)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L16

```solidity
32: constructor(IPool pool) NToken(pool, false)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L32

```solidity
33: constructor(IPool pool) NToken(pool, true)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L33

```solidity
83: constructor(
        IPool pool,
        string memory name_,
        string memory symbol_,
        bool atomic_pricing
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L83

```solidity
43: constructor(
        address _punk,
        address _wpunk,
        address _pool
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L43





### <a href="#Summary">[GAS&#x2011;16]</a><a name="GAS&#x2011;16"> Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier or require such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

#### <ins>Proof Of Concept</ins>

```solidity
139: function addAssets(address[] calldata _assets)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L139

```solidity
148: function removeAsset(address _asset)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
        onlyWhenAssetExisted(_asset)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L148

```solidity
158: function addFeeders(address[] calldata _feeders)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L158

```solidity
167: function removeFeeder(address _feeder)
        external
        onlyWhenFeederExisted(_feeder)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167

```solidity
175: function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L175

```solidity
183: function setPause(address _asset, bool _flag)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L183

```solidity
195: function setPrice(address _asset, uint256 _twap)
        public
        onlyRole(UPDATER_ROLE)
        onlyWhenAssetExisted(_asset)
        whenNotPaused(_asset)
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L195

```solidity
221: function setMultiplePrices(
        address[] calldata _assets,
        uint256[] calldata _twaps
    ) external onlyRole(UPDATER_ROLE) {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L221

```solidity
66: function setAssetSources(
        address[] calldata assets,
        address[] calldata sources
    ) external override onlyAssetListingOrPoolAdmins {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L66

```solidity
74: function setFallbackOracle(address fallbackOracle)
        external
        override
        onlyAssetListingOrPoolAdmins
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L74

```solidity
56: function setMarketId(string memory newMarketId)
        external
        override
        onlyOwner
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56

```solidity
70: function setAddress(bytes32 id, address newAddress)
        external
        override
        onlyOwner
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70


```solidity
81: function setAddressAsProxy(bytes32 id, address newImplementationAddress)
        external
        override
        onlyOwner
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L81

```solidity
105: function updatePoolImpl(
        IParaProxy.ProxyImplementation[] calldata implementationParams,
        address _init,
        bytes calldata _calldata
    ) external override onlyOwner {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L105

```solidity
121: function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
        external
        override
        onlyOwner
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L121

```solidity
142: function setPriceOracle(address newPriceOracle)
        external
        override
        onlyOwner
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142


```solidity
158: function setACLManager(address newAclManager) external override onlyOwner {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158

```solidity
170: function setACLAdmin(address newAclAdmin) external override onlyOwner {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170

```solidity
182: function setPriceOracleSentinel(address newPriceOracleSentinel)
        external
        override
        onlyOwner
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182

```solidity
224: function setProtocolDataProvider(address newDataProvider)
        external
        override
        onlyOwner
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224

```solidity
235: function setWETH(address newWETH) external override onlyOwner {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235

```solidity
242: function setMarketplace(
        bytes32 id,
        address marketplace,
        address adapter,
        address operator,
        bool paused
    ) external override onlyOwner {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242

```solidity
110: function initReserve(
        address asset,
        address xTokenAddress,
        address variableDebtAddress,
        address interestRateStrategyAddress,
        address auctionStrategyAddress
    ) external virtual override onlyPoolConfigurator {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L110

```solidity
139: function dropReserve(address asset)
        external
        virtual
        override
        onlyPoolConfigurator
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139

```solidity
151: function setReserveInterestRateStrategyAddress(
        address asset,
        address rateStrategyAddress
    ) external virtual override onlyPoolConfigurator {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151

```solidity
166: function setReserveAuctionStrategyAddress(
        address asset,
        address auctionStrategyAddress
    ) external virtual override onlyPoolConfigurator {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166

```solidity
181: function setConfiguration(
        address asset,
        DataTypes.ReserveConfigurationMap calldata configuration
    ) external virtual override onlyPoolConfigurator {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181

```solidity
196: function rescueTokens(
        DataTypes.AssetType assetType,
        address token,
        address to,
        uint256 amountOrTokenId
    ) external virtual override onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L196

```solidity
206: function setAuctionRecoveryHealthFactor(uint64 value)
        external
        virtual
        override
        onlyPoolConfigurator
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206

```solidity
79: function mint(
        address onBehalfOf,
        DataTypes.ERC721SupplyParams[] calldata tokenData
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L79

```solidity
87: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L87

```solidity
119: function transferOnLiquidation(
        address from,
        address to,
        uint256 value
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L119

```solidity
127: function rescueERC20(
        address token,
        address to,
        uint256 amount
    ) external override onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L127

```solidity
136: function rescueERC721(
        address token,
        address to,
        uint256[] calldata ids
    ) external override onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L136

```solidity
151: function rescueERC1155(
        address token,
        address to,
        uint256[] calldata ids,
        uint256[] calldata amounts,
        bytes calldata data
    ) external override onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L151

```solidity
168: function executeAirdrop(
        address airdropContract,
        bytes calldata airdropParams
    ) external override onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L168

```solidity
199: function transferUnderlyingTo(address target, uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L199

```solidity
214: function handleRepayment(address user, uint256 amount)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L214

```solidity
102: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102

```solidity
136: function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L136

```solidity
159: function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L159

```solidity
26: function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L26

```solidity
39: function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L39

```solidity
52: function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L52

```solidity
66: function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L66

```solidity
82: function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L82

```solidity
97: function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97

```solidity
26: function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26

```solidity
39: function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L39

```solidity
52: function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L52

```solidity
66: function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L66

```solidity
82: function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L82

```solidity
97: function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97

```solidity
40: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40

```solidity
123: function decreaseUniswapV3Liquidity(
        address user,
        uint256 tokenId,
        uint128 liquidityDecrease,
        uint256 amount0Min,
        uint256 amount1Min,
        bool receiveEthAsWeth
    ) external onlyPool nonReentrant {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123

```solidity
131: function setIncentivesController(IRewardController controller)
        external
        onlyPoolAdmin
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131

```solidity
142: function setBalanceLimit(uint64 limit) external onlyPoolAdmin {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142

```solidity
458: function setIsUsedAsCollateral(
        uint256 tokenId,
        bool useAsCollateral,
        address sender
    ) external virtual override onlyPool nonReentrant returns (bool) {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458

```solidity
474: function batchSetIsUsedAsCollateral(
        uint256[] calldata tokenIds,
        bool useAsCollateral,
        address sender
    )
        external
        virtual
        override
        onlyPool
        nonReentrant
        returns (
            uint256 oldCollateralizedBalance,
            uint256 newCollateralizedBalance
        )
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L474

```solidity
529: function startAuction(uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L529

```solidity
540: function endAuction(uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L540

```solidity
202: function emergencyERC721TokenTransfer(
        address token,
        uint256 tokenId,
        address to
    ) external onlyOwner {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L202

```solidity
217: function emergencyPunkTransfer(address to, uint256 punkIndex)
        external
        onlyOwner
    {

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L217



#### <ins>Recommended Mitigation Steps</ins>
Functions guaranteed to revert when called by normal users can be marked payable.



### <a href="#Summary">[GAS&#x2011;17]</a><a name="GAS&#x2011;17"> Using `unchecked` blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an `unchecked` block

#### <ins>Proof Of Concept</ins>

```solidity
243: require(
244:     (block.number - updatedAt) <= config.expirationPeriod,
245:     "NFTOracle: asset price expired"
246: );

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L243-L246

```solidity
400: uint256 downpayment = price - vars.creditAmount;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L400



### <a href="#Summary">[GAS&#x2011;18]</a><a name="GAS&#x2011;18"> Structs can be packed into fewer storage slots

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

#### <ins>Proof Of Concept</ins>

There is no need to use `uint256` for timestamp as it is unlikely to ever reach the max `uint256` value. `uint64` is enough for timestamps

```solidity
struct PriceInformation {
    // last reported floor price(offchain twap)
    uint256 twap;
    // last updated blocknumber
    uint256 updatedAt;
    // last updated timestamp
    uint256 updatedTimestamp;
}
```

Saving 2 slots by changing to:

```solidity
struct PriceInformation {
    // last reported floor price(offchain twap)
    uint64 twap;
    // last updated blocknumber
    uint64 updatedAt;
    // last updated timestamp
    uint64 updatedTimestamp;
}
```