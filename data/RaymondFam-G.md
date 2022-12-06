## Unused Constructor
Empty `constructor()` that is not used can be removed to save deployment gas. 

Here are the instances entailed:

[File: LooksRareAdapter.sol#L23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L23)

```
    constructor() {}
```
[File: SeaportAdapter.sol#L23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L23)

```
    constructor() {}
```
[File: X2Y2Adapter.sol#L24](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L24)

```
    constructor() {}
```
## Use Storage Instead of Memory for Structs/Arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct. 

Here are the instances entailed:

[File: LooksRareAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol)

```
32:            OrderTypes.TakerOrder memory takerBid,
33:            OrderTypes.MakerOrder memory makerAsk

37:        OfferItem[] memory offer = new OfferItem[](1);

47:        ConsiderationItem[] memory consideration = new ConsiderationItem[](1);
```
[File: SeaportAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

```
32:            AdvancedOrder memory advancedOrder,
33:            CriteriaResolver[] memory resolvers,

66:        (AdvancedOrder[] memory advancedOrders, , ) = abi.decode(
```
[File: X2Y2Adapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol)

```
37:        IX2Y2.RunInput memory runInput = abi.decode(params, (IX2Y2.RunInput));

40:        IX2Y2.SettleDetail memory detail = runInput.details[0];
41:        IX2Y2.Order memory order = runInput.orders[detail.orderIdx];
42:        IX2Y2.OrderItem memory item = order.items[detail.itemIdx];

49:        ERC721Pair[] memory nfts = abi.decode(item.data, (ERC721Pair[]));

53:        OfferItem[] memory offer = new OfferItem[](1);

63:        ConsiderationItem[] memory consideration = new ConsiderationItem[](1);
```
[File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```
357:        PriceInformation memory assetPriceMapEntry = assetPriceMap[_asset];

410:        uint256[] memory validPriceList = new uint256[](feederSize);

414:            PriceInformation memory priceInfo = feederRegistrar.feederPrice[
```
[File: ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

```
196:        uint256[] memory prices = new uint256[](assets.length);
```
[File: UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

```
101:        UinswapV3PositionData memory positionData = getOnchainPositionData(

132:        UinswapV3PositionData memory positionData = getOnchainPositionData(

157:        UinswapV3PositionData memory positionData = getOnchainPositionData(

161:        PairOracleData memory oracleData = _getOracleData(positionData);

191:        uint256[] memory prices = new uint256[](tokenIds.length);

226:        PairOracleData memory oracleData;

294:        FeeParams memory feeParams;
```
## Ternary Over if ... else
Using ternary operator instead of the if else statement saves gas. 

For instance, the code block below may be refactored as follows:

[LooksRareAdapter.sol#L79-L85](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L79-L85)

```
    value == 0
        ? {
            selector = ILooksRareExchange.matchAskWithTakerBid.selector;
        } 
        : {
            selector = ILooksRareExchange
                .matchAskWithTakerBidUsingETHAndWETH
                .selector;
        }
```
All other instances entailed:

[File: UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

```
324:             if (positionData.currentTick >= positionData.tickLower) {

339:            if (positionData.currentTick < positionData.tickUpper) {
```
## Use of Named Returns for Local Variables Saves Gas
You can have further advantages in term of gas cost by simply using named return values as temporary local variable.

As an example, the following instance of code block can refactored as follows:

[File: LooksRareAdapter.sol#L73-L94](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L73-L94)

```
    function matchAskWithTakerBid(
        address marketplace,
        bytes calldata params,
        uint256 value
    ) external payable override returns (bytes memory _data) {
       
        ...

        _data =
            Address.functionCallWithValue(
                marketplace,
                data,
                value,
                Errors.CALL_MARKETPLACE_FAILED
            );
    }
```
All other instances entailed:

[File: SeaportAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)
```
97:    ) external payable override returns (bytes memory) {

112:        returns (bytes memory)

127:        returns (bool)
```
[File: X2Y2Adapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol)

```
92:    ) external payable override returns (bytes memory) {
```
[File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```
261:    function getFeederSize() public view returns (uint256) {

270:    function _isAssetExisted(address _asset) internal view returns (bool) {

274:    function _isFeederExisted(address _feeder) internal view returns (bool) {

354:        returns (bool)

400:        returns (bool, uint256)
```
[File: ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

```
119:        returns (uint256)

142:        returns (uint256)

159:        returns (uint256[] memory)

176:        returns (uint256)

194:        returns (uint256[] memory)

208:        returns (address)

214:    function getFallbackOracle() external view returns (address) {
```
[File: UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

```
54:        returns (UinswapV3PositionData memory)

156:    function getTokenPrice(uint256 tokenId) public view returns (uint256) {

189:        returns (uint256[] memory)

206:        returns (uint256)

224:        returns (PairOracleData memory)
```
## Merging and Filtering Off Identical Imports
In `LooksRareAdapter.sol`, the following two lines of imports originate from the same `ConsiderationStructs.sol`:

[File: LooksRareAdapter.sol#L10-L11](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L10-L11)

```
import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```
Additionally, {AdvancedOrder, CriteriaResolver, Fulfillment} are not referenced in the contract. They may be merged into one import and refactored as follows:

```
import {ConsiderationItem, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
``` 
All other instances entailed:

[File: SeaportAdapter.sol#L10-L11](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L10-L11)

```
import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```
With {ConsiderationItem, OfferItem, ItemType} not refrenced in the contract, the above imports may be merged and refactored as follows:

```
import {AdvancedOrder, CriteriaResolver, Fulfillment} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```
[File: X2Y2Adapter.sol#L11-L12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L11-L12)

```
import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```
With {AdvancedOrder, CriteriaResolver, Fulfillment} not refrenced in the contract, the above imports may be merged and refactored as follows:

```
import {ConsiderationItem, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```
## Unused Imports
Importing source units that are not referenced in the contract increases the size of deployment. Consider removing them to save some gas.

Here are the instances entailed:

[File: LooksRareAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol)

```
7: import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";

9: import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";

13: import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";

15: import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
```
[File: SeaportAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

```
6: import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol";

8: import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol";

9: import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";

10: import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";

13: import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";

15: import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
```
[File: X2Y2Adapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol)

```
6: import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol";

7: import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";

8: import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol";

10: import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";

14: import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";

16: import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
```
## Split Require Statements Using &&
Instead of using the && operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per require statement will save 3 GAS per &&. 

Here are the instances entailed:

[File: SeaportAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

```
43:            resolvers.length == 0 && isBasicOrder(advancedOrder)

73:            advancedOrders.length == 2 &&
74:                isBasicOrder(advancedOrders[0]) &&
```
## || Costs Less Gas Than Its Equivalent &&
Rule of thumb: (x && y) is (!(!x || !y))

Even with the 10k Optimizer enabled: `||`, OR conditions cost less than their equivalent `&&`, AND conditions.

As an example, the code block below may be refactored as follows:

[File: ParaSpaceOracle.sol#L130](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L130)

```
        if (!(price != 0 || address(_fallbackOracle) == address(0))) {
```
## Private Function Embedded Modifier Reduces Contract Size
Consider having the logic of a modifier embedded through a private function to reduce contract size if need be. A private visibility that saves more gas on function calls than the internal visibility is adopted because the modifier will only be making this call inside the contract.

For instance, the modifier instance below may be refactored as follows:

[File: NFTFloorOracle.sol#L110-L115](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L110-L115)

```
    function _whenNotPaused(address _asset) private view {
        if (!hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) {
            _whenNotPaused(_asset);
        }
    }

    modifier whenNotPaused(address _asset) {
        _whenNotPaused(_asset);
        _;
    }
```
All other modifier instances entailed:

[File: NFTFloorOracle.sol#L117-L135](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L117-L135)

```
117:    modifier onlyWhenAssetExisted(address _asset) {

122:    modifier onlyWhenAssetNotExisted(address _asset) {

127:    modifier onlyWhenFeederExisted(address _feeder) {

132:    modifier onlyWhenFeederNotExisted(address _feeder) {
```
## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
  solidity: {
    version: "0.8.10",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:
```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```
Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## Payable Access Control Functions Costs Less Gas
Consider marking functions with access control as `payable`. This will save 20 gas on each call by their respective permissible callers for not needing to have the compiler check for `msg.value`. 

Here are the instances entailed:

[File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```
141:        onlyRole(DEFAULT_ADMIN_ROLE)

150:        onlyRole(DEFAULT_ADMIN_ROLE)

160:        onlyRole(DEFAULT_ADMIN_ROLE)

177:        onlyRole(DEFAULT_ADMIN_ROLE)

185:        onlyRole(DEFAULT_ADMIN_ROLE)

197:        onlyRole(UPDATER_ROLE)
```
[File: ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

```
69:    ) external override onlyAssetListingOrPoolAdmins {

77:        onlyAssetListingOrPoolAdmins


```
## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =).

As an example, consider replacing `>=` with the strict counterpart `>` in the following inequality instance:

[File: NFTFloorOracle.sol#L370](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L370)

```
        if (priceDeviation > config.maxPriceDeviation - 1) {
```
All other instances entailed:

[File: UniswapV3OracleWrapper.sol#L324](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L324)

```
            if (positionData.currentTick >= positionData.tickLower) {
```
Similarly, as an example, consider replacing `<=` with the strict counterpart `<` in the following inequality instance:

[File: NFTFloorOracle.sol#L244](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L244)

```
            (block.number - updatedAt) < config.expirationPeriod + 1,
```
All other `<=` instances entailed:

[File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```
419:                if (diffBlock <= config.expirationPeriod) {

441:        while (i <= j) {

444:            if (i <= j) {
```
## Unchecked SafeMath Saves Gas
"Checked" math, which is default in ^0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. When no arithmetic overflow/underflow is going to happen, `unchecked { ... }` to use the previous wrapping behavior further saves gas just as in the for loop below as an example:

[File: NFTFloorOracle.sol#L229-L231](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229-L231)

```
    unchecked {
        for (uint256 i = 0; i < _assets.length; i++) {
            setPrice(_assets[i], _twaps[i]);
        }
    }
```
All other instances enailed:

[File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```
321:        for (uint256 i = 0; i < _feeders.length; i++) {

413:        for (uint256 i = 0; i < feederSize; i++) {

442:            while (arr[uint256(i)] < pivot) i++;
443:            while (pivot < arr[uint256(j)]) j--;
```
[File: ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

```
95:        for (uint256 i = 0; i < assets.length; i++) {
```
[File: UniswapV3OracleWrapper.sol#L324](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L324)

```
193:        for (uint256 index = 0; index < tokenIds.length; index++) {

210:        for (uint256 index = 0; index < tokenIds.length; index++) {
```
## += and -= Costs More Gas
`+=` generally costs 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop. As an example, the following line of code could be rewritten as:

[File: UniswapV3OracleWrapper.sol#L211](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L211)

```
            sum = sum + getTokenPrice(tokenIds[index]);
```
All other instances entailed:

[File: UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

```
        token0Amount += positionData.tokensOwed0;
        token1Amount += positionData.tokensOwed1;
```
