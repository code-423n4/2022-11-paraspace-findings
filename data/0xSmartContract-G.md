### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] | Remove the `initializer` modifier  | 5 |
| [G-02] |Structs can be packed into fewer storage slots | 17 |
| [G-03] | `Sample` struct can be packed with lower values | 1 |
| [G-04] | Use ``assembly`` to write _address storage values_  | 2 |
| [G-05] | Setting The Constructor To Payable | 2|
| [G-06] | Functions guaranteed to revert_ when callled by normal users can be marked `payable`   | 13 |
| [G-07] | Use ``double require`` instead of using ``&&``   | 7 |
| [G-08] | Sort Solidity operations using short-circuit mode | 30 |
| [G-09] | Optimize names to save gas  | All contracts |
| [G-10] | ```x -= y  (x += y )``` costs more gas than ```x = x – y  (x = x + y) ``` for state variables  | 43 |
| [G-11] | Use a different pattern when using for loops  |6  |
| [G-12] | The solady Library's Ownable contract is significantly gas-optimized, which can be used  | 13 |
| [G-13] | Inverting the condition of an if-else-statement wastes gas  |12  |
| [G-14] | Using contract instance as immutable provides gas optimization  |3 |
| [G-15] | Comparison operators |10  |

Total 15 issues

### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Use `v4.8.0 OpenZeppelin` contracts |

Total 1 suggestion

### [G-01] Remove the `initializer` modifier [20 K Gas Saved]

if we can just ensure that the `initialize()` function could only be called from within the constructor, we shouldn't need to worry about it getting called again. 

```solidity
5 results - 5 files

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  97:     function initialize(

paraspace-core/contracts/protocol/pool/PoolCore.sol:
  75:     function initialize(IPoolAddressesProvider provider)

paraspace-core/contracts/protocol/tokenization/NToken.sol:
  52:     function initialize(

paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol:
  36:     function initialize(

paraspace-core/contracts/ui/WPunkGateway.sol:
  57:     function initialize() external initializer {
 58          __Ownable_init();

```


In the EVM, the constructor's job is actually to return the bytecode that will live at the contract's address. So, while inside a constructor, your address `(address(this))` will be the deployment address, but there will be no bytecode at that address! So if we check `address(this).code.length` before the constructor has finished, even from within a delegatecall, we will get 0. So now let's update our `initialize()` function to only run if we are inside a constructor:

```diff

paraspace-core/contracts/ui/WPunkGateway.sol:
  56  
  57:     function initialize() external initializer {
+          require(address(this).code.length == 0, 'not in constructor');
  58          __Ownable_init();

```

Now the Proxy contract's constructor can still delegatecall initialize(), but if anyone attempts to call it again (after deployment) through the Proxy instance, or tries to call it directly on the above instance, it will revert because address(this).code.length will be nonzero. 

Also, because we no longer need to write to any state to track whether initialize() has been called, we can avoid the 20k storage gas cost. In fact, the cost for checking our own code size is only 2 gas, which means we have a 10,000x gas savings over the standard version. Pretty neat!

### [G-02] Structs can be packed into fewer storage slots (20k gas)

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

**Context:** 
```solidity
17 results - 10 files

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:
  33:     struct FeeParams {
  40:     struct PairOracleData {

paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol:
  26:     struct ERC721Pair {

paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol:
  27:     struct AuctionLocalVars {

paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol:
  32:     struct CalculateUserAccountDataVars {

paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol:
   99:     struct ExecuteLiquidateLocalVars {
  132:     struct LiquidateParametersLocalVars {

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  50:     struct MarketplaceLocalVars {

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:
   225:     struct ValidateBorrowLocalVars {
   478:     struct ValidateLiquidateLocalVars {
   487:     struct ValidateAuctionLocalVars {
  1146:     struct ValidateForUniswapV3LocalVars {

paraspace-core/contracts/protocol/pool/PoolApeStaking.sol:
  224:     struct BorrowAndStakeLocalVar {

paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:
  26:     struct APEStakingParameter {
  77:     struct UnstakeAndRepayParams {

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:
  12: struct UserState {
  18: struct MintableERC721Data {

```

### [G-03] `Sample` struct can be packed with lower values

While it's theoretically possible for values like Timestamp and blocknumber  to get the maximum value of uint256, it won't be possible in practice.
For example ; timestamp in uint128 is sufficient
By packing the struct as below, 1 slot is used less.

uint128 = mx value ; 24 April 3048 (1026 years)

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

_Current Code:_
```solidity
1 result - 1 file

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  23: struct PriceInformation {
  24    // last reported floor price(offchain twap)
  25    uint256 twap;		// Slot 0
  26    // last updated blocknumber
  27    uint256 updatedAt;	// Slot 1
  28    // last updated timestamp
  29     uint256 updatedTimestamp;	// Slot 2
  30   }
  31 
```

_Recommendation Code:_
```solidity
    paraspace-core/contracts/misc/NFTFloorOracle.sol:
  23: struct PriceInformation {
  24    // last reported floor price(offchain twap)
  25    uint256 twap;		// Slot 0
  26    // last updated blocknumber
  27    uint128 updatedAt;	// Slot 1
  28    // last updated timestamp
  29     uint128 updatedTimestamp;	// Slot 1
  30   }
  31 
```
### [G-04] Use ``assembly`` to write _address storage values_ 

**Context:**
```solidity
2 results 2 files

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  49:     constructor(
  50:         IPoolAddressesProvider provider,
  51:         address[] memory assets,
  52:         address[] memory sources,
  53:         address fallbackOracle,
  54:         address baseCurrency,
  55:         uint256 baseCurrencyUnit
  56:     ) {
  57:         ADDRESSES_PROVIDER = provider;
  58:         BASE_CURRENCY = baseCurrency;
  59:         BASE_CURRENCY_UNIT = baseCurrencyUnit;
  60:         _setFallbackOracle(fallbackOracle);
  61:         _setAssetsSources(assets, sources);
  62:         emit BaseCurrencySet(baseCurrency, baseCurrencyUnit);
  63:     }

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:
  23:     constructor(
  24:         address _factory,
  25:         address _manager,
  26:         address _addressProvider
  27:     ) {
  28:         UNISWAP_V3_FACTORY = IUniswapV3Factory(_factory);
  29:         UNISWAP_V3_POSITION_MANAGER = INonfungiblePositionManager(_manager);
  30:         ADDRESSES_PROVIDER = IPoolAddressesProvider(_addressProvider);
  31:     }
  
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L49-L63
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23-L31

### [G-05] Setting The Constructor To Payable

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

**Context:**
```solidity
2 results - 2 files

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  49:     constructor(

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:
  23:     constructor(

```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L49
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23


**Recommendation:**
Set the constructor to ```payable```

### [G-06]  Functions guaranteed to revert_ when callled by normal users can be marked `payable` 

If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

**Context:** 
```solidity
13 results - 3 files

paraspace-core/contracts/misc/ERC721OracleWrapper.sol:
  44:     function setOracle(address _oracleAddress)
  45         external
  46         onlyAssetListingOrPoolAdmins
  47     {

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  150          onlyRole(DEFAULT_ADMIN_ROLE)
  151:         onlyWhenAssetExisted(_asset)
  152      {

  167:     function removeFeeder(address _feeder)
  168         external
  169         onlyWhenFeederExisted(_feeder)
  170     {

  175:     function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
  176         external
  177         onlyRole(DEFAULT_ADMIN_ROLE)
  178     {

  183:     function setPause(address _asset, bool _flag)
  184         external
  185         onlyRole(DEFAULT_ADMIN_ROLE)
  186:    {
  
  195:     function setPrice(address _asset, uint256 _twap)
  196         public
  197         onlyRole(UPDATER_ROLE)
  198         onlyWhenAssetExisted(_asset)
  199         whenNotPaused(_asset)
  200    {

  221:     function setMultiplePrices(
  222         address[] calldata _assets,
  223         uint256[] calldata _twaps
  224     ) external onlyRole(UPDATER_ROLE) {

  278:     function _addAsset(address _asset)
  279         internal
  280         onlyWhenAssetNotExisted(_asset)
  281     {

  296:     function _removeAsset(address _asset)
  297         internal
  298         onlyWhenAssetExisted(_asset)
  299     {

  307:     function _addFeeder(address _feeder)
  308         internal
  309         onlyWhenFeederNotExisted(_feeder)
  310     {

  326:     function _removeFeeder(address _feeder)
  327         internal
  328         onlyWhenFeederExisted(_feeder)
  329     {

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  66:     function setAssetSources(
  67         address[] calldata assets,
  68         address[] calldata sources
  69     ) external override onlyAssetListingOrPoolAdmins {

  74:     function setFallbackOracle(address fallbackOracle)
  75         external
  76         override
  77         onlyAssetListingOrPoolAdmins
  78     {
```

**Recommendation:**
Functions guaranteed to revert when called by normal users can be marked payable  (for only ```onlyAssetListingOrPoolAdmins, onlyWhenAssetExisted, onlyWhenAssetNotExisted, onlyWhenFeederExisted, onlyWhenFeederNotExisted or onlyRole``` functions)

### [G-07] Use ``double require`` instead of using ``&&``

Using double require instead of operator && can save more gas
When having a require statement with 2 or more expressions needed, place the expression that cost less gas first.

**Context:** 
```solidity
7 results - 3 files

paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol:
  41         require(
  42             // NOT criteria based and must be basic order
  43:             resolvers.length == 0 && isBasicOrder(advancedOrder),
  44             Errors.INVALID_MARKETPLACE_ORDER
  45         );

  71         require(
  72             // NOT criteria based and must be basic order
  73             advancedOrders.length == 2 &&
  74:                 isBasicOrder(advancedOrders[0]) &&
  75                 isBasicOrder(advancedOrders[1]),
  76             Errors.INVALID_MARKETPLACE_ORDER
  77         );

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  388             require(
  389                 item.itemType == ItemType.ERC20 ||
  390:                     (vars.isETH && item.itemType == ItemType.NATIVE),
  391                 Errors.INVALID_ASSET_TYPE
  392             );

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol: 
  546         require(
  547:             vars.collateralReserveActive && vars.principalReserveActive,
  548             Errors.RESERVE_INACTIVE
  549         );

  550         require(
  551:             !vars.collateralReservePaused && !vars.principalReservePaused,
  552             Errors.RESERVE_PAUSED
  553         );
   
  636         require(
  637:             vars.collateralReserveActive && vars.principalReserveActive,
  638             Errors.RESERVE_INACTIVE
  639         );
  
  640         require(
  641:             !vars.collateralReservePaused && !vars.principalReservePaused,
  642             Errors.RESERVE_PAUSED
  643         );
```

**Recommendation:**
SeaportAdapter.sol :L#80;
 ```solidity
  41         require(
  42             // NOT criteria based and must be basic order
  43:             resolvers.length == 0 && isBasicOrder(advancedOrder),
  44             Errors.INVALID_MARKETPLACE_ORDER
  45         );
```

Recommendation Code:
```solidity
    require(resolvers.length == 0 , Errors.INVALID_MARKETPLACE_ORDER”);
    require(isBasicOrder(advancedOrder),” Errors.INVALID_MARKETPLACE_ORDER”)
```

### [G-08] Sort Solidity operations using short-circuit mode

**Description:**
Short-circuiting is a solidity contract development model that uses ```OR/AND``` logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.
```
//f(x) is a low gas cost operation 
//g(y) is a high gas cost operation 

//Sort operations with different gas costs as follows 
f(x) || g(y) 
f(x) && g(y)
```

**Context:**
```solidity
30 results - 12 files

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  331:         if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
  362:         if (_priorTwap == 0 || _updatedAt == 0) {


paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  130:         if (price == 0 && address(_fallbackOracle) != address(0)) {

paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol:
  43:             resolvers.length == 0 && isBasicOrder(advancedOrder),

paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol:
  151:         if (params.usePTokens && params.amount == type(uint256).max) {

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  390:                     (vars.isETH && item.itemType == ItemType.NATIVE),

paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol:
  474:         if (params.from != params.to && params.amount != 0) {
  662:         if (oldCollateralizedBalance == 0 && newCollateralizedBalance != 0) {

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:
   379:             amountSent != type(uint256).max || msg.sender == onBehalfOf,
   521:             msg.value == 0 || params.liquidationAsset == params.weth,
   526:             msg.value == 0 || msg.value >= params.actualLiquidationAmount,
   547:             vars.collateralReserveActive && vars.principalReserveActive,
   551:             !vars.collateralReservePaused && !vars.principalReservePaused,
   637:             vars.collateralReserveActive && vars.principalReserveActive,
   641:             !vars.collateralReservePaused && !vars.principalReservePaused,
   674:                 (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
   870:             !hasZeroLtvCollateral || reserve.configuration.getLtv() == 0,
   977:             reserve.id != 0 || reservesList[0] == asset,
  1197:                 vars.token0IsActive && vars.token1IsActive,
  1203:                 !vars.token0IsPaused && !vars.token1IsPaused,
  1209:                 !vars.token0IsFrozen && !vars.token1IsFrozen,

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:
  111:         if (from != to && erc721Data.auctions[tokenId].startTime > 0) {
  145:         if (from != to && isUsedAsCollateral_) {
  410:                 balanceLimit == 0 || balance <= balanceLimit,

paraspace-core/contracts/protocol/pool/PoolCore.sol:
  762:             reserve.id != 0 || ps._reservesList[0] == underlyingAsset,

paraspace-core/contracts/protocol/pool/PoolParameters.sol:
  159:             ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
  174:             ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
  189:             ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,

paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol:
  94:             (token0 == weth || token1 == weth));

paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:
  196:             _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
```

### [G-09] Optimize names to save gas

**Context:** 
All Contracts

**Description:** 
Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ``` PoolCore.sol ``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

PoolCore.sol function names can be named and sorted according to METHOD ID

```js
Sighash   |   Function Signature
========================
52751797  =>  getReserveAddressById(uint16)
1316529d  =>  getRevision()
10c1ce58  =>  initialize(IPoolAddressesProvider)
617ba037  =>  supply(address,uint256,address,uint16)
0dc0ae9c  =>  supplyERC721(address,DataTypes.ERC721SupplyParams[],address,uint16)
a3257a88  =>  supplyERC721FromNToken(address,DataTypes.ERC721SupplyParams[],address)
02c205f0  =>  supplyWithPermit(address,uint256,address,uint16,uint256,uint8,bytes32,bytes32)
69328dec  =>  withdraw(address,uint256,address)
3786ddfc  =>  withdrawERC721(address,uint256[],address)
00b708c6  =>  decreaseUniswapV3Liquidity(address,uint256,uint128,uint256,uint256,bool)
1d5d7237  =>  borrow(address,uint256,uint16,address)
5ceae9c4  =>  repay(address,uint256,address)
851def34  =>  repayWithPTokens(address,uint256)
01db53c0  =>  repayWithPermit(address,uint256,address,uint256,uint8,bytes32,bytes32)
c5fa1ed2  =>  setUserUseERC20AsCollateral(address,bool)
58b666b1  =>  setUserUseERC721AsCollateral(address,uint256[],bool)
3d7b66bf  =>  liquidateERC20(address,address,address,uint256,bool)
d134142e  =>  liquidateERC721(address,address,uint256,uint256,bool)
685b8517  =>  startAuction(address,address,uint256)
459ac032  =>  endAuction(address,address,uint256)
759de116  =>  flashClaim(address,address,uint256[],bytes)
35ea6a75  =>  getReserveData(address)
c44b11f7  =>  getConfiguration(address)
4417a583  =>  getUserConfiguration(address)
d15e0053  =>  getReserveNormalizedIncome(address)
386497fd  =>  getReserveNormalizedVariableDebt(address)
d1946dbc  =>  getReservesList()
f8119d51  =>  MAX_NUMBER_RESERVES()
76d61799  =>  AUCTION_RECOVERY_HEALTH_FACTOR()
d59544cb  =>  finalizeTransfer(address,address,address,bool,uint256,uint256,uint256)
366740c0  =>  finalizeTransferERC721(address,uint256,address,address,bool,uint256,uint256)
bb5ce40d  =>  getAuctionData(address,uint256)
150b7a02  =>  onERC721Received(address,address,uint256,bytes)
```

### [G-10] ```x -= y  (x += y )``` costs more gas than ```x = x – y  (x = x + y) ``` for state variables

```x -= y``` costs more gas than ```x = x – y``` for state variables.

**Context:**
```solidity
4 results  2 files

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
   83:         vars.ethLeft -= _buyWithCredit(
  205:             vars.ethLeft -= _buyWithCredit(

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:
  146:             erc721Data.userState[from].collateralizedBalance -= 1;
  318:         erc721Data.userState[user].balance -= balanceToBurn;```
```

```solidity
39 results  5 files

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:
  149:         token0Amount += positionData.tokensOwed0;
  150:         token1Amount += positionData.tokensOwed1;
  211:             sum += getTokenPrice(tokenIds[index]);

paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol:
  169:                     vars.payableDebtByERC20Assets += vars
  176:                     vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
  178:                     vars.totalCollateralInBaseCurrency += vars
  185:                     vars.avgLiquidationThreshold += vars.liquidationThreshold;
  189:                     vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency(
  231:                     vars.avgERC721LiquidationThreshold += vars
  233:                     vars.totalERC721CollateralInBaseCurrency += vars
  235:                     vars.totalCollateralInBaseCurrency += vars
  237:                     vars.avgLtv += vars.ltv;
  238:                     vars.avgLiquidationThreshold += vars.liquidationThreshold;
  380:                     totalValue += _getTokenPrice(
  479:                 totalValue += tokenPrice;
  496:                 totalLTV += tmpLTV * tokenPrice;

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  397:             price += item.startAmount;

paraspace-core/contracts/protocol/pool/PoolApeStaking.sol:
   77:             amountToWithdraw += _nfts[index].amount;
  166:             amountToWithdraw += _nftPairs[index].amount;

paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:
  215:             totalAmount += getTokenIdStakingAmount(
  257:             apeStakedAmount += bakcStakedAmount;
```

### [G-11] Use a different pattern when using for loops

In the use of for loops, this structure, which will reduce gas costs, can be preferred. It saves approximately 400 gas in an array with 6 members.

**Context:**
```solidity
6 results - 3 files

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  222         address[] calldata _assets,
  229:         for (uint256 i = 0; i < _assets.length; i++) {
  230             setPrice(_assets[i], _twaps[i]);
  231         }
  232:    }

  290     function _addAssets(address[] memory _assets) internal {
  291:         for (uint256 i = 0; i < _assets.length; i++) {
  292             _addAsset(_assets[i]);
  293         }
  294     }

  320     function _addFeeders(address[] memory _feeders) internal {
  321:         for (uint256 i = 0; i < _feeders.length; i++) {
  322             _addFeeder(_feeders[i]);
  323         }
  324     }

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
   87     function _setAssetsSources(
   88         address[] memory assets,
   89         address[] memory sources    
   95:         for (uint256 i = 0; i < assets.length; i++) {

  190     function getAssetsPrices(address[] calldata assets)
  197:         for (uint256 i = 0; i < assets.length; i++) {

paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol:
  100     function executeMintToTreasury(
  101         mapping(address => DataTypes.ReserveData) storage reservesData,
  102         address[] calldata assets
  103     ) external {
  104:         for (uint256 i = 0; i < assets.length; i++) {
  105             address assetAddress = assets[i];
```


**Current Code:**
```solidity
paraspace-core/contracts/misc/NFTFloorOracle.sol#L222
           address[] calldata _assets,
           for (uint256 i = 0; i < _assets.length; i++) {
               setPrice(_assets[i], _twaps[i]);
           }
     }
```

**Recommendation Code:**
```solidity
paraspace-core/contracts/misc/NFTFloorOracle.sol#L222
           address[] calldata _assets,
           for (uint256 i; i < _assets.length; i = unchecked_inc(i)) {
            // do something that doesn't change the value of i
               setPrice(_assets[i], _twaps[i]);
           }
      }

    function unchecked_inc(uint i) internal pure returns (uint) {
        unchecked {
            return i + 1;
        }
    }
```

### [G-12] The solady Library's Ownable contract is significantly gas-optimized, which can be used

The project uses the ` onlyAssetListingOrPoolAdmins, onlyWhenAssetExisted, onlyWhenAssetNotExisted, onlyWhenFeederExisted, onlyWhenFeederNotExisted or onlyRole ` authorization model I recommend using _Solady's highly gas optimized contract._

https://github.com/Vectorized/solady/blob/main/src/auth/OwnableRoles.sol

```solidity
13 results - 3 files

paraspace-core/contracts/misc/ERC721OracleWrapper.sol:
  44:     function setOracle(address _oracleAddress)
  45         external
  46         onlyAssetListingOrPoolAdmins
  47     {


paraspace-core/contracts/misc/NFTFloorOracle.sol:
  150          onlyRole(DEFAULT_ADMIN_ROLE)
  151:         onlyWhenAssetExisted(_asset)
  152      {

  167:     function removeFeeder(address _feeder)
  168         external
  169         onlyWhenFeederExisted(_feeder)
  170     {

  175:     function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
  176         external
  177         onlyRole(DEFAULT_ADMIN_ROLE)
  178     {

  183:     function setPause(address _asset, bool _flag)
  184         external
  185         onlyRole(DEFAULT_ADMIN_ROLE)
  186:    {
  
  195:     function setPrice(address _asset, uint256 _twap)
  196         public
  197         onlyRole(UPDATER_ROLE)
  198         onlyWhenAssetExisted(_asset)
  199         whenNotPaused(_asset)
  200    {

  221:     function setMultiplePrices(
  222         address[] calldata _assets,
  223         uint256[] calldata _twaps
  224     ) external onlyRole(UPDATER_ROLE) {

  278:     function _addAsset(address _asset)
  279         internal
  280         onlyWhenAssetNotExisted(_asset)
  281     {

  296:     function _removeAsset(address _asset)
  297         internal
  298         onlyWhenAssetExisted(_asset)
  299     {

  307:     function _addFeeder(address _feeder)
  308         internal
  309         onlyWhenFeederNotExisted(_feeder)
  310     {

  326:     function _removeFeeder(address _feeder)
  327         internal
  328         onlyWhenFeederExisted(_feeder)
  329     {

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  66:     function setAssetSources(
  67         address[] calldata assets,
  68         address[] calldata sources
  69     ) external override onlyAssetListingOrPoolAdmins {

  74:     function setFallbackOracle(address fallbackOracle)
  75         external
  76         override
  77         onlyAssetListingOrPoolAdmins
  78     {

```


### [G-13] Inverting the condition of an if-else-statement wastes gas

Flipping the true and false blocks instead saves 3 gas

```solidity
12 results 6 files
paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol:
   97              0,
   98:             params.releaseUnderlying ? params.amount : 0
   99          );

  172              params.asset,
  173:             params.usePTokens ? 0 : paybackAmount,
  174              0

paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol:
  249                  ? vars.avgLtv / vars.totalCollateralInBaseCurrency
  250:                 : 0;
  251              vars.avgLiquidationThreshold = vars.totalCollateralInBaseCurrency !=

  254                      vars.totalCollateralInBaseCurrency
  255:                 : 0;
  256  

  260                      vars.totalERC721CollateralInBaseCurrency
  261:                 : 0;
  262          }

  265              ? type(uint256).max
  266:             : (
  267                  vars.totalCollateralInBaseCurrency.percentMul(

  274              ? type(uint256).max
  275:             : (
  276                  vars.totalERC721CollateralInBaseCurrency.percentMul(

paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol:
  616              ? DEFAULT_LIQUIDATION_CLOSE_FACTOR
  617:             : MAX_LIQUIDATION_CLOSE_FACTOR;
  618  

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  564          vars.isETH = params.credit.token == address(0);
  565:         vars.creditToken = vars.isETH ? params.weth : params.credit.token;
  566          vars.creditAmount = params.credit.amount;


paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol:
   98                  tokenId: tokenId,
   99:                 recipient: receiveEthAsWeth ? address(this) : user,
  100                  amount0Max: type(uint128).max,

  113  
  114:             address pairToken = (token0 == weth) ? token1 : token0;
  115              uint256 balanceToken = IERC20(pairToken).balanceOf(address(this));

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:
  178              ? collateralizedBalance + 1
  179:             : collateralizedBalance - 1;
  180          erc721Data

```
### [G-14] Using contract instance as immutable provides gas optimization

In case of using Solidity version 0.8.13, using contract instance as immutable provides gas optimization.

```solidity

3 results - 3 files

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  22:     IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;

paraspace-core/contracts/misc/ProtocolDataProvider.sol:
  33:     IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;

paraspace-core/contracts/protocol/pool/PoolCore.sol:
  54:     IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;

```

### [G-15] Comparison operators

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `> and =`.
 Comment
Using strict comparison operators hence saves gas

**Context:**
```solidity
10 results - 5 files

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  412:             require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:
  526:             msg.value == 0 || msg.value >= params.actualLiquidationAmount,
  673:             params.maxLiquidationAmount >= params.actualLiquidationAmount &&
  674:                 (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
  733:             healthFactor >= HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

paraspace-core/contracts/protocol/tokenization/NToken.sol:
  176:         require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);

paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol:
  24:      * Each BAYC committed must attach an ApeCoin amount >= 1 ApeCoin and <= the BAYC pool cap amount.
  63:      * Each BAKC committed must attach an ApeCoin amount >= 1 ApeCoin and <= the Pair pool cap amount.\

paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol:
  24:      * Each MAYC committed must attach an ApeCoin amount >= 1 ApeCoin and <= the MAYC pool cap amount.
  63:      * Each BAKC committed must attach an ApeCoin amount >= 1 ApeCoin and <= the Pair pool cap amount.\

```

**Recommendation:** 
Replace <= with <, and >= with >. Do not forget to increment/decrement the compared variable.

```diff
paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
- 412:        require(params.ethLeft >= downpayment, Errors.PAYNOW_NOT_ENOUGH);
+               require(params.ethLeft > downpayment - 1, Errors.PAYNOW_NOT_ENOUGH);
```

### [S-01] Use `v4.8.0 OpenZeppelin` contracts

**Description:**
The upcoming v4.8.0 version of OpenZeppelin provides many small gas optimizations.

https://github.com/OpenZeppelin/openzeppelin-contracts/releases/tag/v4.8.0-rc.0

```js
v4.8.0-rc.0
⛽ Many small optimizations
```

