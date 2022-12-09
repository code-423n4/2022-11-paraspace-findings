## G-01 INCREMENTS CAN BE UNCHECKED

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline

Prior to Solidity 0.8.0, arithmetic operations would always wrap in case of under- or overflow leading to widespread use of libraries that introduce additional checks.

Since Solidity 0.8.0, all arithmetic operations revert on over- and underflow by default, thus making the use of these libraries unnecessary.

To obtain the previous behaviour, an unchecked block can be used

_There are 48 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
File: contracts/misc/NFTFloorOracle.sol

229: for (uint256 i = 0; i < _assets.length; i++) {
291: for (uint256 i = 0; i < _assets.length; i++) {
321: for (uint256 i = 0; i < _feeders.length; i++) {
413: for (uint256 i = 0; i < feederSize; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol

```
File: contracts/misc/ParaSpaceOracle.sol

95: for (uint256 i = 0; i < assets.length; i++) {
197: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
File: contracts/misc/UniswapV3OracleWrapper.sol

193: for (uint256 index = 0; index < tokenIds.length; index++) {
210: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

```
File: contracts/protocol/libraries/logic/FlashClaimLogic.sol

38: for (i = 0; i < params.nftTokenIds.length; i++) {
56: for (i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

```
File: contracts/protocol/libraries/logic/GenericLogic.sol

371: for (uint256 index = 0; index < totalBalance; index++) {
466: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
File: contracts/protocol/libraries/logic/MarketplaceLogic.sol

177: for (uint256 i = 0; i < marketplaceIds.length; i++) {
284: for (uint256 i = 0; i < marketplaceIds.length; i++) {
382: for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
470: for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

```
File: contracts/protocol/libraries/logic/PoolLogic.sol

58: for (uint16 i = 0; i < params.reservesCount; i++) {
104: for (uint256 i = 0; i < assets.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

```
File: contracts/protocol/libraries/logic/SupplyLogic.sol

184: for (uint256 index = 0; index < params.tokenData.length; index++) {
195: for (uint256 index = 0; index < params.tokenData.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```
File: contracts/protocol/libraries/logic/ValidationLogic.sol

212: for (uint256 index = 0; index < tokenIds.length; index++) {
465: for (uint256 index = 0; index < tokenIds.length; index++) {
910: for (uint256 index = 0; index < tokenIds.length; index++) {
1034: for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```
File: contracts/protocol/pool/PoolApeStaking.sol

72: for (uint256 index = 0; index < _nfts.length; index++) {
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
File: contracts/protocol/pool/PoolCore.sol

634: for (uint256 i = 0; i < reservesListCount; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol

```
File: contracts/protocol/tokenization/NToken.sol

106: for (uint256 index = 0; index < tokenIds.length; index++) {
145: for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```
File: contracts/protocol/tokenization/NTokenApeStaking.sol

107: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

```
File: contracts/protocol/tokenization/NTokenMoonBirds.sol

51: for (uint256 index = 0; index < tokenIds.length; index++) {
97: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

```
File: contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

```
File: contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213: for (uint256 index = 0; index < totalBalance; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

```
File: contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207: for (uint256 index = 0; index < tokenData.length; index++) {
280: for (uint256 index = 0; index < tokenIds.length; index++) {
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol

```
File: contracts/ui/WPunkGateway.sol

82: for (uint256 i = 0; i < punkIndexes.length; i++) {
109: for (uint256 i = 0; i < punkIndexes.length; i++) {
113: for (uint256 i = 0; i < punkIndexes.length; i++) {
136: for (uint256 i = 0; i < punkIndexes.length; i++) {
174: for (uint256 i = 0; i < punkIndexes.length; i++) {
```

------

## G-02 x += y COSTS MORE GAS THAN x = x + y FOR STATE VARIABLES (SAME APPLIES FOR x -= y , x = x - y)

_There are 26 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
File: contracts/misc/UniswapV3OracleWrapper.sol

149: token0Amount += positionData.tokensOwed0;
150: token1Amount += positionData.tokensOwed1;
211: sum += getTokenPrice(tokenIds[index]);
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

```
File: contracts/protocol/libraries/logic/GenericLogic.sol

169: vars.payableDebtByERC20Assets += vars
176: vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
178: vars.totalCollateralInBaseCurrency += vars
185: vars.avgLiquidationThreshold += vars.liquidationThreshold;
189: vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency(
231: vars.avgERC721LiquidationThreshold += vars
233: vars.totalERC721CollateralInBaseCurrency += vars
235: vars.totalCollateralInBaseCurrency += vars
237: vars.avgLtv += vars.ltv;
238: vars.avgLiquidationThreshold += vars.liquidationThreshold;
380: totalValue += _getTokenPrice(
479: totalValue += tokenPrice;
496: totalLTV += tmpLTV * tokenPrice;
497: totalLiquidationThreshold +=
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
File: contracts/protocol/libraries/logic/MarketplaceLogic.sol

83: vars.ethLeft -= _buyWithCredit(
205: vars.ethLeft -= _buyWithCredit(
397: price += item.startAmount;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```
File: contracts/protocol/pool/PoolApeStaking.sol

77: amountToWithdraw += _nfts[index].amount;
166: amountToWithdraw += _nftPairs[index].amount;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

```
File: contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

215: totalAmount += getTokenIdStakingAmount
257: apeStakedAmount += bakcStakedAmount;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

```
File: contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

146: erc721Data.userState[from].collateralizedBalance -= 1;
318: erc721Data.userState[user].balance -= balanceToBurn;
```

-----

## G-03 SPLITTING REQURE() STATEMENTS THAT USE && SAVES GAS

_There are 13 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

```
File: contracts/misc/marketplaces/SeaportAdapter.sol

41-43: require(
            // NOT criteria based and must be basic order
            resolvers.length == 0 && isBasicOrder(advancedOrder),
71-74: require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&
                isBasicOrder(advancedOrders[0]) &&
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
File: contracts/protocol/libraries/logic/MarketplaceLogic.sol

171-172:  require(
            marketplaceIds.length == payloads.length &&
279-280: require(
            marketplaceIds.length == payloads.length &&
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```
File: contracts/protocol/libraries/logic/ValidationLogic.sol

546-547: require(
            vars.collateralReserveActive && vars.principalReserveActive,
550-551: require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
636-637: require(
            vars.collateralReserveActive && vars.principalReserveActive,
640-641: require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
672-673: require(
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&
1196-1197: require(
                vars.token0IsActive && vars.token1IsActive,
1202-1203: require(
                !vars.token0IsPaused && !vars.token1IsPaused,
1208-1209: require(
                !vars.token0IsFrozen && !vars.token1IsFrozen,
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol

```
File: contracts/protocol/pool/PoolParameters.sol

216-217: require(
            value > MIN_AUCTION_HEALTH_FACTOR &&
```

------

## G-04 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

_There are 7 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
File: contracts/misc/NFTFloorOracle.sol

265: function _whenNotPaused(address _asset) internal view {
278: function _addAsset(address _asset)
296: function _removeAsset(address _asset)
307: function _addFeeder(address _feeder)
326: function _removeFeeder(address _feeder)
351: function _checkValidity(address _asset, uint256 _twap)
388: function _addRawValue(address _asset, uint256 _twap) internal {
```

-------

## G-05 MULTIPLICATION OR DIVISION BY 2 SHOULD USE BIT SHIFTING

`<x> * 2` is equivalent to `<x> << 1` and `<x> / 2` is the same as `<x> >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
File: contracts/misc/UniswapV3OracleWrapper.sol

247: ) * 2**96) / 1E9
259: ) * 2**96) / 1E9
271: ) * 2**96) /
```

-----

## G-06 EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
File: contracts/misc/NFTFloorOracle.sol

16-17: bytes32 constant POOL_STORAGE_POSITION =
        bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol

```
File: contracts/protocol/pool/PoolStorage.sol

16-17: bytes32 constant POOL_STORAGE_POSITION =
        bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```
File: contracts/protocol/tokenization/NTokenApeStaking.sol

23-25: bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
        bytes32(
            uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
```

------

## G-07 DUPLICATED REQUIRE() OR REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```
File: contracts/protocol/libraries/logic/ValidationLogic.sol

85: require(isActive, Errors.RESERVE_INACTIVE);
86: require(!isPaused, Errors.RESERVE_PAUSED);
87: require(!isFrozen, Errors.RESERVE_FROZEN);
```

------

## G-08 USE msg.sender INSTEAD OF OPENZEPPELIN'S msgsender() WHEN META-TRANSACTIONS CAPABILITIES AREN'T USED

_There are 5 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

```
File: contracts/protocol/tokenization/NTokenMoonBirds.sol

99: _isApprovedOrOwner(_msgSender(), tokenIds[index]),
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

```
File: contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

60: require(_msgSender() == address(POOL), Errors.CALLER_MUST_BE_POOL);
196: _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
260:  _isApprovedOrOwner(_msgSender(), tokenId),
297: _isApprovedOrOwner(_msgSender(), tokenId),
```

-----

## G-09 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

_There are 2 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
File: contracts/misc/NFTFloorOracle.sol

236-240: function getPrice(address _asset)
        external
        view
        override
        returns (uint256 price)
252-256: function getLastUpdateTime(address _asset)
        external
        view
        override
        returns (uint256 timestamp)
```

-------

## G-10 REQUIRE() OR REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
File: contracts/misc/NFTFloorOracle.sol

356: require(_twap > 0, "NFTOracle: price should be more than 0");
```

---

## G-11 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

_There are several instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol

```
File: contracts/ui/WPunkGateway.sol

80: uint16 referralCode
134: uint16 referralCode
172: uint16 referralCode
```

---
