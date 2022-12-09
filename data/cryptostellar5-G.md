### G-01 ++I or I++ SHOULD BE UNCHECKED{++I} or UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

*Number of Instances Identified: 48*

the `unchecked` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. 

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
229: for (uint256 i = 0; i < _assets.length; i++) {
291: for (uint256 i = 0; i < _assets.length; i++) {
321: for (uint256 i = 0; i < _feeders.length; i++) {
413: for (uint256 i = 0; i < feederSize; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol

```
95: for (uint256 i = 0; i < assets.length; i++) {
197: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
193: for (uint256 index = 0; index < tokenIds.length; index++) {
210: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

```
38: for (i = 0; i < params.nftTokenIds.length; i++) {
56: for (i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

```
371: for (uint256 index = 0; index < totalBalance; index++) {
466: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
177: for (uint256 i = 0; i < marketplaceIds.length; i++) {
284: for (uint256 i = 0; i < marketplaceIds.length; i++) {
382: for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
470: for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

```
58: for (uint16 i = 0; i < params.reservesCount; i++) {
104: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

```
184: for (uint256 index = 0; index < params.tokenData.length; index++) {
195: for (uint256 index = 0; index < params.tokenData.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```
131: for (uint256 index = 0; index < amount; index++) {
212: for (uint256 index = 0; index < tokenIds.length; index++) {
465: for (uint256 index = 0; index < tokenIds.length; index++) {
910: for (uint256 index = 0; index < tokenIds.length; index++) {
1034: for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```
72:  for (uint256 index = 0; index < _nfts.length; index++) {
103: for (uint256 index = 0; index < _nfts.length; index++) {
138: for (uint256 index = 0; index < _nftPairs.length; index++) {
172: for (uint256 index = 0; index < actualTransferAmount; index++) {
199: for (uint256 index = 0; index < _nftPairs.length; index++) {
215: for (uint256 index = 0; index < _nftPairs.length; index++) {
279: for (uint256 index = 0; index < _nfts.length; index++) {
289: for (uint256 index = 0; index < _nftPairs.length; index++) {
305: for (uint256 index = 0; index < _nftPairs.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol

```
634: for (uint256 i = 0; i < reservesListCount; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol

```
106: for (uint256 index = 0; index < tokenIds.length; index++) {
145: for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```
107: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

```
51: for (uint256 index = 0; index < tokenIds.length; index++) {
97: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

```
493: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

```
213: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol

```
82: for (uint256 i = 0; i < punkIndexes.length; i++) {
109: for (uint256 i = 0; i < punkIndexes.length; i++) {
113: for (uint256 i = 0; i < punkIndexes.length; i++) {
136: for (uint256 i = 0; i < punkIndexes.length; i++) {
174: for (uint256 i = 0; i < punkIndexes.length; i++) {
```


### G-02 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Number of Instances Identified: 24*

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
149: token0Amount += positionData.tokensOwed0;
150: token1Amount += positionData.tokensOwed1;
211: sum += getTokenPrice(tokenIds[index]);
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

```
169: vars.payableDebtByERC20Assets += vars
176: vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
178: vars.totalCollateralInBaseCurrency += vars
185: vars.avgLiquidationThreshold += vars.liquidationThreshold;
189: vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency
231: vars.avgERC721LiquidationThreshold += vars
233: vars.totalERC721CollateralInBaseCurrency += vars
235: vars.totalCollateralInBaseCurrency += vars
237: vars.avgLtv += vars.ltv;
238: vars.avgLiquidationThreshold += vars.liquidationThreshold;
380: totalValue += _getTokenPrice
479: totalValue += tokenPrice;
496:  totalLTV += tmpLTV * tokenPrice;
497: totalLiquidationThreshold += tmpLiquidationThreshold * tokenPrice;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
83:  vars.ethLeft -= _buyWithCredit
205:  vars.ethLeft -= _buyWithCredit
397: price += item.startAmount;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```
77: amountToWithdraw += _nfts[index].amount;
166: amountToWithdraw += _nftPairs[index].amount;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

```
215: totalAmount += getTokenIdStakingAmount
257: apeStakedAmount += bakcStakedAmount;
```


### G-03 SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

*Number of Instances Identified: 11*

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
171-173: require(
            marketplaceIds.length == payloads.length && 
                payloads.length == credits.length,
279-283: require(
            marketplaceIds.length == payloads.length && 
                payloads.length == credits.length,
            Errors.INCONSISTENT_PARAMS_LENGTH
        );
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```
546-549:  require(
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
        );
550-553: require( 
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
        );
636-639: require( 
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
        );
640-663: require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
        );
672-676:  require( 
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&
                (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
            Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
        );
1196-1199: require(
                vars.token0IsActive && vars.token1IsActive,
                Errors.RESERVE_INACTIVE
            );   
1202-1205: require(
                !vars.token0IsPaused && !vars.token1IsPaused,
                Errors.RESERVE_PAUSED
            );
1208-1211: require(
                !vars.token0IsFrozen && !vars.token1IsFrozen,
                Errors.RESERVE_FROZEN
            );
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol

```
216-220:  require(
            value > MIN_AUCTION_HEALTH_FACTOR &&
                value <= MAX_AUCTION_HEALTH_FACTOR,
            Errors.INVALID_AMOUNT
        );
```



### G-04 USAGE OF UINTS or INTS SMALLER THAN 32 BYTES - 256 BITS INCURS OVERHEAD

*Number of Instances Identified: 23*

> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed


https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
300: uint8 assetIndex
330: uint8 feederIndex
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
69: uint16 referralCode
166: uint16 referralCode
236: uint16 referralCode
274: uint16 referralCode
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```
1095: uint8 v
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol

```
95: uint16 referralCode
117: uint16 referralCode
160: uint16 referralCode
162: uint8 permitV
240: uint128 liquidityDecrease
270: uint16 referralCode
343: uint8 permitV
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

```
75: uint16 referralCode
94: uint16 referralCode
114: uint16 referralCode
135: uint16 referralCode
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

```
54: uint128 liquidityDecrease
126:  uint128 liquidityDecrease,
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol

```
80: uint16 referralCode
134: uint16 referralCode
172: uint16 referralCode
```

### G-05 DUPLICATED REQUIRE() or REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

*Number of Instances Identified: 3*

saves deployment cost

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

The following require checks were being used multiple times within the same contract

```
85: require(isActive, Errors.RESERVE_INACTIVE);
86: require(!isPaused, Errors.RESERVE_PAUSED);
87: require(!isFrozen, Errors.RESERVE_FROZEN);
```

### G-06 REQUIRE or REVERT STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

*Number of Instances Identified: 1*

Each extra chunk of bytes past the original 32 [incurs an MSTORE](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs 3 gas

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
356: require(_twap > 0, "NFTOracle: price should be more than 0");
```


### G-07 MULTIPLICATION or DIVISION BY TWO SHOULD USE BIT SHIFTING

*Number of Instances Identified: 2*

`<x> * 2` is equivalent to `<x> << 1` and `<x> / 2` is the same as `<x> >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
259: * 2**96
271: * 2**96
```


### G-08 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

*Number of Instances Identified: 2*

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
240: returns (uint256 price)
256: returns (uint256 timestamp)
```


### G-09 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

*Number of Instances Identified: 4*

Not inlining costs **20 to 40 gas** because of two extra `JUMP` instructions and additional stack operations needed for function calls.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
265: function _whenNotPaused(address _asset) internal view
296: function _removeAsset(address _asset) internal
307: function _addFeeder(address _feeder) internal
326: function _removeFeeder(address _feeder) internal
```
