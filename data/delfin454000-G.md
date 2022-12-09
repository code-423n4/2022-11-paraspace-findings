### Use of `if/else` when `require` functions could be used
Using `requires` will save gas in the case below

[PoolAddressesProvider.sol: L213-220](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L213-L220)
```solidity
        if (
            marketplace.marketplace != address(0) &&
            Address.isContract(marketplace.marketplace)
        ) {
            return marketplace;
        } else {
            revert(Errors.INVALID_MARKETPLACE_ID);
        }
```
Recommendation:
```solidity
        require(marketplace.marketplace != address(0), Errors.INVALID_MARKETPLACE_ID);
        require(Address.isContract(marketplace.marketplace), Errors.INVALID_MARKETPLACE_ID);
            return marketplace;
```
___
___

### Use `private` constants rather than `public` constants
Gas is saved by using private constants since the compiler does not have to (1) create non-payable getter functions for deployment calldata; and (2) add another entry to the method ID table. The value of the constant can be read from the verified contract source code, if necessary. 

___
[LiquidationLogic.sol: L57](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L57)
```solidity
    uint256 public constant MAX_LIQUIDATION_CLOSE_FACTOR = 1e4;
```
___
[LiquidationLogic.sol: L64](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L64)
```solidity
    uint256 public constant CLOSE_FACTOR_HF_THRESHOLD = 0.95e18;
```
___
[ValidationLogic.sol: L45](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45)
```solidity
    uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;
```
___
[ValidationLogic.sol: L49](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L49)
```solidity
    uint256 public constant MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD =
```
___
[ValidationLogic.sol: L56](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L56)
```solidity
    uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;
```
___
[PoolCore.sol: L53](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L53)
```solidity
    uint256 public constant POOL_REVISION = 1;
```
___
[NToken.sol: L32](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L32)
```solidity
    uint256 public constant NTOKEN_REVISION = 0x1;
```
___
___

### Avoid storage of uints or ints smaller than 32 bytes, if possible
Storage of uints or ints smaller than 32 bytes incurs overhead. Instead, use size of at least 32, then downcast where needed

___
[NFTFloorOracle.sol: L9](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9)
```solidity
uint8 constant MIN_ORACLES_NUM = 3;
```
___
[NFTFloorOracle.sol: L36](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L36)
```solidity
    uint8 index;
```
___
[NFTFloorOracle.sol: L47](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L47)
```solidity
    uint8 index;
```
___
[NFTFloorOracle.sol: L300](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L300)
```solidity
        uint8 assetIndex = assetFeederMap[_asset].index;
```
___
[NFTFloorOracle.sol: L330](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L330)
```solidity
        uint8 feederIndex = feederPositionMap[_feeder].index;
```
___
[UniswapV3OracleWrapper.sol: L43-44](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L43-L44)
```solidity
        uint8 token0Decimal;
        uint8 token1Decimal;
```
___
[UniswapV3OracleWrapper.sol: L61-63](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L61-L63)
```solidity
            uint24 fee,
            int24 tickLower,
            int24 tickUpper,
```
___
[BorrowLogic.sol: L33](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L33)
```solidity
        uint16 indexed referralCode
```
___
[LiquidationLogic.sol: L125](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L125)
```solidity
        uint16 liquidationAssetReserveId;
```
___
[MarketplaceLogic.sol: L69](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L69)
```solidity
        uint16 referralCode
```
___
[MarketplaceLogic.sol: L166](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L166)
```solidity
        uint16 referralCode
```
___
[MarketplaceLogic.sol: L236](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L236)
```solidity
        uint16 referralCode
```
___
[MarketplaceLogic.sol: L274](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L274)
```solidity
        uint16 referralCode
```
___
[PoolLogic.sol: L58](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58)
```solidity
        for (uint16 i = 0; i < params.reservesCount; i++) {
```
___
[SupplyLogic.sol: L48](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L48)
```solidity
        uint16 indexed referralCode
```
___
[SupplyLogic.sol: L61](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L61)
```solidity
        uint16 indexed referralCode,
```
___
[SupplyLogic.sol: L135](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L135)
```solidity
        uint16 reserveId,
```
___
[ValidationLogic.sol: L1095](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1095)
```solidity
        uint8 v,
```
___
[PoolCore.sol: L95](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L95)
```solidity
        uint16 referralCode
```
___
[PoolCore.sol: L117](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L117)
```solidity
        uint16 referralCode
```
___
[PoolCore.sol: L160](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L160)
```solidity
        uint16 referralCode,
```
___
[PoolCore.sol: L270](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L270)
```solidity
        uint16 referralCode,
```
___
[PoolCore.sol: L650](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L650)
```solidity
    function getReserveAddressById(uint16 id) external view returns (address) {
```
___
[PoolCore.sol: L662](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L662)
```solidity
        returns (uint16)
```
___
[PoolMarketplace.sol: L75](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L75)
```solidity
        uint16 referralCode
```
___
[PoolMarketplace.sol: L94](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L94)
```solidity
        uint16 referralCode
```
___
[PoolMarketplace.sol: L114](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L114)
```solidity
        uint16 referralCode
```
___
[PoolMarketplace.sol: L135](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L135)
```solidity
        uint16 referralCode
```
___
[WPunkGateway.sol: L80](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L80)
```solidity
        uint16 referralCode
```
___
[WPunkGateway.sol: L134](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L134)
```solidity
        uint16 referralCode
```
___
[WPunkGateway.sol: L172](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L172)
```solidity
        uint16 referralCode
```
___
___

### Avoid use of default "checked" behavior in a `for` loop
Underflow/overflow checks are made every time `++i` (or `i++` or equivalent counter) is called. Such checks are unnecessary since `i` is already limited. Therefore, use `unchecked {++i}`/`unchecked {i++}` instead, as shown below:

___
[NFTFloorOracle.sol: L229-231](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229-L231)
```solidity
        for (uint256 i = 0; i < _assets.length; i++) {
            setPrice(_assets[i], _twaps[i]);
        }
```
Suggestion:
```solidity
        for (uint256 i = 0; i < _assets.length;) {
            setPrice(_assets[i], _twaps[i]);

            unchecked {
              ++i;
          }
        }
```
___
Similarly, for the following 48 `for` loops:

**NFTFloorOracle.sol**

[L291-293](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291-L293), [L321-323](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321-L323), [L413-424](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413-L424)

**ParaSpaceOracle.sol**

[L95-102](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95-L102), [L197-199](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197-L199)

**UniswapV3OracleWrapper.sol**

[L193-195](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193-L195), [L210-212](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210-L212)

**FlashClaimLogic.sol**

[L38-43](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38-L43), [L56-69](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56-L69)

**GenericLogic.sol**

[L371-386](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371-L386), [L466-501](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466-L501)

**MarketplaceLogic.sol**

[L177-180](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177-L180), [L284-287](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284-L287), [L382-383](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382-L383), [L470-471](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470-L471)

**PoolLogic.sol**

[L58-64](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58-L64), [L104-105](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104-L105)

**SupplyLogic.sol**

[L184-193](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184-L193), [L195-201](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195-L201)

**ValidationLogic.sol**

[L131-146](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131-L146), [L212-221](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212-L221), [L465-474](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465-L474), [L910-919](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910-L919), [L1034-1039](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034-L1039)

**PoolApeStaking.sol**

[L72-78](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72-L78), [L103-108](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103-L108), [L138-167](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138-L167), [L172-178](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172-L178), [L199-210](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199-L210)

[L215-221](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215-L221), [L279-285](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279-L285), [L289-302](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289-L302), [L305-311](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305-L311)

**PoolCore.sol**

[L634-640](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634-L640)

**MintableIncentivizedERC721.sol**

[L493-501](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493-L501)

**NToken.sol**

[L106-112](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106-L112), [L145-147](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145-L147)

**NTokenApeStaking.sol**

[L107-120](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107-L120)

**NTokenMoonBirds.sol**

[L51-57](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51-L57), [L97-102](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97-L102)

**MintableERC721Logic.sol**

[L207-238](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207-L238), [L280-315](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280-L315)

**ApeStakingLogic.sol**

[L213-220](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213-L220)

**WPunkGateway.sol**

[L82-87](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82-L87), [L109-111](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L109-L111), [L113-116](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L113-L116), [L136-147](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L136-L147), [L174-185](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L174-L185)
___
___

### Constant initialized but never used
Explanation: Unused constants should be removed since constant initialization costs gas
 
[ValidationLogic.sol: L45](https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45)
```solidity
    uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;
```
Remove unused `REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD`
___
___

### Inline a function/modifier that’s only used once
It costs less gas to inline the code of functions that are only called once. Doing so saves the cost of allocating the function selector, and the cost of the jump when the function is called.

__Modifiers__

[NFTFloorOracle.sol: L122-125](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L122-L125)
```solidity
    modifier onlyWhenAssetNotExisted(address _asset) {
        require(!_isAssetExisted(_asset), "NFTOracle: asset existed");
        _;
    }
```
Since `onlyWhenAssetNotExisted()` is used only once in this contract (in `function _addAsset()` as shown below), it should be inlined to save gas

[NFTFloorOracle.sol: L278-281](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L278-L281)
```solidity
    function _addAsset(address _asset)
        internal
        onlyWhenAssetNotExisted(_asset)
    {
```
___
[NFTFloorOracle.sol: L132-135](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L132-L135)
```solidity
    modifier onlyWhenFeederNotExisted(address _feeder) {
        require(!_isFeederExisted(_feeder), "NFTOracle: feeder existed");
        _;
    }
```
Since `onlyWhenFeederNotExisted()` is used only once (in `function _addFeeder()` as shown below), it should be inlined to save gas

[NFTFloorOracle.sol: L307-310](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L307-L310)
```solidity
    function _addFeeder(address _feeder)
        internal
        onlyWhenFeederNotExisted(_feeder)
    {
```
___
___

__Functions__

[NFTFloorOracle.sol: L265](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L265)
```solidity
    function _whenNotPaused(address _asset) internal view {
```
Since `_whenNotPaused` is used only once, as shown below, it should be inlined to save gas
[NFTFloorOracle.sol: L111-113](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L111-L113)
```solidity
        if (!hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) {
            _whenNotPaused(_asset);
        }
```
The following `internal` or `private` `functions` are also called only once and should be inlined:

**NFTFloorOracle.sol**

[L296-297](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296-L297), [L307-308](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L307-L308), [L326-327](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L326-L327)

[L351-352](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L351-L352), [L388](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L388), [L397-398](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L397-L398)

**UniswapV3OracleWrapper.sol**

[L221-222](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L221-L222), [L282-283](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L282-L283)

**PoolAddressesProvider.sol**

[L302-307](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L302-L307)

**GenericLogic.sol**

[L334-339](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L334-L339), [L362-365](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L362-L365), [L450-455](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L450-L455), [L512-518](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L512-L518)

**LiquidationLogic.sol**

[L435-439](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L435-L439), [L465-468](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L465-L468), [L488-493](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L488-L493), [L523-527](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L523-L527)

[L564-569](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L564-L569), [L605-608](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L605-L608), [L659-664](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L659-L664), [L757-762](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L757-L762)

**MarketplaceLogic.sol**

[L376-379](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L376-L379), [L552](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L552), [L593-600](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L593-L600)

**ValidationLogic.sol**

[L1092-1098](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1092-L1098), [L1110-1111](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1110-L1111), [L1133](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1133)

**DefaultReserveAuctionStrategy.sol**

[L101-102](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L101-L102)

**PoolApeStaking.sol**

[L413](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413)

**PoolParameters.sol**

[L69](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69)

**NTokenApeStaking.sol**

[L130](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L130)

**NTokenUniswapV3.sol**

[L51-58](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L51-L58)
___
___
