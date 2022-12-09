# Low, QA, NC

## Low

### L-00 `executeBatchBuyWithCredit` does not follow the CHECK-EFFECT-ITERATION pattern
Add a non reentrant modifier to avoid unexpected behaviour

### L-01 `_checkValidity` should return true if priceDeviation is `maxPriceDeviation`

On [NFTFloorOracle.sol#L370](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L370);
Instead of `if (priceDeviation >= config.maxPriceDeviation) {` should be `if (priceDeviation > config.maxPriceDeviation)` otherwise if the `priceDeviation` is the `maxPriceDeviation` the function `setPrice` will fail.

### L-02 `WETH` address shouldnt be pass as an argument to avoid unwanted errors
On [LooksRareAdapter.sol#L25](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L25) the function `getAskOrderInfo` is receiving `WETH` address as a parameter and `WETH` should be setted as an immutable on the constructor. Doing this the complexity will be lower and you will save gas.

### L-03 `WETH` should be a constant or an immtuable and owner shoulndt change it
On [PoolAddressesProvider.sol#L235-L239](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235-L239) you can change the `WETH` address, but `WETH` contract address should always be the same, you should remove this function and set `WETH` on the constructor or as a constant.

### L-04 Variable `owner` is shadowed
Just change `owner` to `_owner` to avoid a shadowed variable.
```diff
diff --git a/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol b/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol
index 5f1e3c8..8381a65 100644
--- a/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol
+++ b/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol
@@ -42,9 +42,9 @@ contract PoolAddressesProvider is Ownable, IPoolAddressesProvider {
      * @param marketId The identifier of the market.
      * @param owner The owner address of this contract.
      */
-    constructor(string memory marketId, address owner) {
+    constructor(string memory marketId, address _owner) {
         _setMarketId(marketId);
-        transferOwnership(owner);
+        transferOwnership(_owner);
     }
 
     /// @inheritdoc IPoolAddressesProvider
```

### L-05 Missing address(0) checks

- On `setACLAdmin(address newAclAdmin)` [PoolAddressesProvider.sol#L170-L174](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170-L174), `newAclAdmin` can be address(0)?
- On `setPriceOracleSentinel(address newPriceOracleSentinel)` [PoolAddressesProvider.sol#L182](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182), `newPriceOracleSentinel` can be address(0)?
- On `setProtocolDataProvider(address newDataProvider)` [PoolAddressesProvider.sol#L224](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224), `newDataProvider` can be address(0)?

### Q-01 Unsolved TODOs
On [LooksRareAdapter.sol#L59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59) you have an unsolved TODO that says;
`// TODO: take minPercentageToAsk into account`

On [MarketplaceLogic.sol#L442](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442) you have an unsolved TODO that says;
`// TODO: support PToken`

On [INToken.sol#L97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/interfaces/INToken.sol#L97) you have an unsolved TODO that says;
`// TODO are we using the Treasury at all? Can we remove?`

On [UiIncentiveDataProvider.sol#L68](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol#L68) you have an unsolved TODO that says;
`// TODO: check that this is deployed correctly on contract and remove casting`

On [UniswapV3OracleWrapper.sol#L238](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238) you have an unsolved TODO that says;
`// TODO using bit shifting for the 2^96`

Consider resolving this before going into production


### Q-02 Unused imports;
These imports arent used by the contract;

```
contracts/misc/marketplaces/SeaportAdapter.sol:6:import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol"; // @audit unused
contracts/misc/marketplaces/SeaportAdapter.sol:8:import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol"; // @audit unused
contracts/misc/marketplaces/SeaportAdapter.sol:9:import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol"; // @audit unused
contracts/misc/marketplaces/SeaportAdapter.sol:10:import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol"; // @audit unused
contracts/misc/marketplaces/SeaportAdapter.sol:11:import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol"; // @audit unused OfferItem, ItemType
contracts/misc/marketplaces/SeaportAdapter.sol:13:import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol"; // @audit unused
contracts/misc/marketplaces/SeaportAdapter.sol:15:import {PoolStorage} from "../../protocol/pool/PoolStorage.sol"; // @audit unused
contracts/misc/marketplaces/X2Y2Adapter.sol:6:import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol"; // @audit unused import
contracts/misc/marketplaces/X2Y2Adapter.sol:7:import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol"; // @audit unused import
contracts/misc/marketplaces/X2Y2Adapter.sol:8:import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol"; // @audit unused import
contracts/misc/marketplaces/X2Y2Adapter.sol:10:import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol"; // @audit unused import
contracts/misc/marketplaces/X2Y2Adapter.sol:12:import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol"; // @audit unused AdvancedOrder, CriteriaResolver, Fulfillment
contracts/misc/marketplaces/X2Y2Adapter.sol:14:import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol"; // @audit unused import
contracts/misc/marketplaces/X2Y2Adapter.sol:16:import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";// @audit unused import
contracts/misc/marketplaces/LooksRareAdapter.sol:7:import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol"; //@audit unused
contracts/misc/marketplaces/LooksRareAdapter.sol:9:import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol"; // @audit unused
contracts/misc/marketplaces/LooksRareAdapter.sol:11:import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol"; // @audit unused AdvancedOrder, CriteriaResolver, Fulfillment
contracts/misc/marketplaces/LooksRareAdapter.sol:13:import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol"; // @audit unused import
contracts/misc/marketplaces/LooksRareAdapter.sol:15:import {PoolStorage} from "../../protocol/pool/PoolStorage.sol"; // @audit unused import
contracts/protocol/libraries/logic/FlashClaimLogic.sol:11:import {XTokenType, IXTokenType} from "../../../interfaces/IXTokenType.sol";//@audit unused import
contracts/protocol/libraries/logic/BorrowLogic.sol:5:import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";// @audit unused import
contracts/protocol/libraries/logic/BorrowLogic.sol:10:import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol"; // @audit unused import
contracts/protocol/libraries/logic/AuctionLogic.sol:8:import {IReserveAuctionStrategy} from "../../../interfaces/IReserveAuctionStrategy.sol"; // @audit unused import
contracts/protocol/pool/PoolCore.sol:7:import {PoolLogic} from "../libraries/logic/PoolLogic.sol"; // @audit Q unused import
contracts/protocol/pool/PoolCore.sol:10:import {MarketplaceLogic} from "../libraries/logic/MarketplaceLogic.sol";// @audit Q unused import
contracts/protocol/pool/PoolCore.sol:19:import {IACLManager} from "../../interfaces/IACLManager.sol"; // @audit Q unused import
contracts/protocol/pool/PoolCore.sol:23:import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";// @audit Q unused import
contracts/protocol/pool/PoolCore.sol:24:import {IMarketplace} from "../../interfaces/IMarketplace.sol";// @audit Q unused import
contracts/protocol/pool/PoolCore.sol:29:import {IWETH} from "../../misc/interfaces/IWETH.sol"; // @audit Q unused import
```

### Q-03 Import the same file twice
On `contracts/protocol/pool/PoolCore.sol:25`
Yoa re importing `libraries/helpers/Errors.sol` twice;
`import {Errors} from "../libraries/helpers/Errors.sol";` <- this is repeated

### Q-04 Use the pattern `import {contract} from "file.sol";`
On `contracts/protocol/libraries/logic/FlashClaimLogic.sol:10` you are using a plain import instead of a named import;
`import "../../../interfaces/INTokenApeStaking.sol";`
