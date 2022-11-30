## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1] | Using bools for storage incurs overhead | 25 |
| [GAS-2] | Loop can be more optimizable | 48 |
| [GAS-3] | Split require() statement that use && operator can help in save gas | 12 |
| [GAS-4] | <x> += <y> costs more gas than <x> = <x> + <y> for state variables | 25 |
| [GAS-5] | Arithmetic operations that will not over or under flow should made uncheck   | 3 |
| [GAS-6] | Function that guaranteed to revert when called by normal user can be marked payable  | 36 |
| [GAS-7] | >= COSTS LESS GAS THAN > | 12 |
| [GAS-8] | Function that could be external | 1 |
 


### [GAS-01] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the  slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

*Instances (25)*:
```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

38:   bool paused;
45:   bool registered;

```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

52:  bool hasZeroLtvCollateral;
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

127:  bool auctionEnabled;
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

239:  bool isActive;  
240:  bool isFrozen; 
241:  bool isPaused;  
242:  bool borrowingEnabled;  
243:  bool siloedBorrowingEnabled;

479-483: bool collateralReserveActive;  
               bool collateralReservePaused;  
               bool principalReserveActive;  
               bool principalReservePaused;  
               bool isCollateralEnabled; 

488-490: bool collateralReserveActive;  
               bool collateralReservePaused;  
               bool isCollateralEnabled; 

1147-1152:
        bool token0IsActive;   
        bool token0IsFrozen;   
        bool token0IsPaused;   
        bool token1IsActive;   
        bool token1IsFrozen;   
        bool token1IsPaused;
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

38:  mapping(address => mapping(address => bool)) operatorApprovals;
43:  mapping(uint256 => bool) isUsedAsCollateral;
```


### [GAS-02] Loop can be more optimizable

. Instead of using <, <= can use to save gas(its save around 4gas, which is more effective in loop cases)
           =: To do that first catch array length outside of loop by decreasing by one
           =: like ```uint len = arr.length - 1;
           =: Then use it in loop with <= operator,

. Should not initialize uint with default value i.e uint i=0 **TO** uint i;
. Should use ++i instead i++
. Should uncheck ++i


*Instances (48)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:    for (uint256 index = 0; index < tokenIds.length; index++) {
210:    for (uint256 index = 0; index < tokenIds.length; index++) {

```

```solidity
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol

95:   for (uint256 i = 0; i < assets.length; i++) {
197:  for (uint256 i = 0; i < assets.length; i++) {
```

```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

229:   for (uint256 i = 0; i < _assets.length; i++) {
291:   for (uint256 i = 0; i < _assets.length; i++) { 
321:   for (uint256 i = 0; i < _feeders.length; i++) {   
413:   for (uint256 i = 0; i < feederSize; i++) {
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38:  for (i = 0; i < params.nftTokenIds.length; i++) {
56:  for (i = 0; i < params.nftTokenIds.length; i++) {
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

111:  while (vars.i < params.reservesCount) { 
371:  for (uint256 index = 0; index < totalBalance; index++) {
466:  for (uint256 index = 0; index < totalBalance; index++) {
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:  for (uint256 i = 0; i < marketplaceIds.length; i++) {
284:  for (uint256 i = 0; i < marketplaceIds.length; i++) {
382:  for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
470:  for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```  

```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58:  for (uint16 i = 0; i < params.reservesCount; i++) {
104:  for (uint16 i = 0; i < params.reservesCount; i++) {
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:  for (uint256 index = 0; index < params.tokenData.length; index++) {
195:  for (uint256 index = 0; index < params.tokenData.length; index++) {
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

131:  for (uint256 index = 0; index < amount; index++) {
212:  for (uint256 index = 0; index < tokenIds.length; index++) {
465:  for (uint256 index = 0; index < tokenIds.length; index++) {
910:  for (uint256 index = 0; index < tokenIds.length; index++) {
1034: for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
72:  for (uint256 index = 0; index < _nfts.length; index++) {
103:  for (uint256 index = 0; index < _nfts.length; index++) { 
138:  for (uint256 index = 0; index < _nftPairs.length; index++) {
172:  for (uint256 index = 0; index < actualTransferAmount; index++) {
199:  for (uint256 index = 0; index < _nftPairs.length; index++) {
215:  for (uint256 index = 0; index < _nftPairs.length; index++) {
279:  for (uint256 index = 0; index < _nfts.length; index++) {
289:  for (uint256 index = 0; index < _nftPairs.length; index++) {
305:  for (uint256 index = 0; index < _nftPairs.length; index++) {
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NToken.sol
106:  for (uint256 index = 0; index < tokenIds.length; index++) {
145:  for (uint256 i = 0; i < ids.length; i++) {
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
107:  for (uint256 index = 0; index < tokenIds.length; index++) {
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:  for (uint256 index = 0; index < tokenIds.length; index++) {
97:  for (uint256 index = 0; index < tokenIds.length; index++) { 
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213:  for (uint256 index = 0; index < totalBalance; index++) { 
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:   for (uint256 index = 0; index < tokenData.length; index++) {
280:   for (uint256 index = 0; index < tokenIds.length; index++) {
```
```solidity
File:   paraspace-core/contracts/ui/WPunkGateway.sol

82:  for (uint256 i = 0; i < punkIndexes.length; i++) {
109:  for (uint256 i = 0; i < punkIndexes.length; i++) {
113:  for (uint256 i = 0; i < punkIndexes.length; i++) {
136:  for (uint256 i = 0; i < punkIndexes.length; i++) { 
174:  for (uint256 i = 0; i < punkIndexes.length; i++) { 
```

### [GAS-03] Split require() statement that use && operator can help in save gas

*Instances (12)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

41-45:  require(
            // NOT criteria based and must be basic order
            resolvers.length == 0 && isBasicOrder(advancedOrder),  
            Errors.INVALID_MARKETPLACE_ORDER
        );

71-77:  require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&  
                isBasicOrder(advancedOrders[0]) &&  
                isBasicOrder(advancedOrders[1]),
            Errors.INVALID_MARKETPLACE_ORDER
        );

```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

171-175:  require(
            marketplaceIds.length == payloads.length && 
                payloads.length == credits.length,
            Errors.INCONSISTENT_PARAMS_LENGTH
        );  
279-283:  require(
            marketplaceIds.length == payloads.length &&   
                payloads.length == credits.length,
            Errors.INCONSISTENT_PARAMS_LENGTH
        );
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

546-549:  require(
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
        );  
550-553:  require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
        ); 
636-639: require(  
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
        );
640-643: require(  
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
        );
1196-1199:  require(
                vars.token0IsActive && vars.token1IsActive,  
                Errors.RESERVE_INACTIVE
            );
1202-1205:  require(
                !vars.token0IsPaused && !vars.token1IsPaused,  
                Errors.RESERVE_PAUSED
            );
1208-1211:  require(
                !vars.token0IsFrozen && !vars.token1IsFrozen, 
                Errors.RESERVE_FROZEN
            );
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolParameters.sol

216-220:  require(
            value > MIN_AUCTION_HEALTH_FACTOR &&  
                value <= MAX_AUCTION_HEALTH_FACTOR,
            Errors.INVALID_AMOUNT
        );
```

### [GAS-04] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

*Instances (25)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

149:   token0Amount += positionData.tokensOwed0;  
150:   token1Amount += positionData.tokensOwed1;
211:   sum += getTokenPrice(tokenIds[index]);

```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

169-172:  vars.payableDebtByERC20Assets += vars  
                        .userBalanceInBaseCurrency
                        .percentDiv(vars.liquidationBonus);
176:  vars.avgLtv += vars.userBalanceInBaseCurrency * vars.ltv;
178-179:  vars.totalCollateralInBaseCurrency += vars  
                        .userBalanceInBaseCurrency;  
185:  vars.avgLiquidationThreshold += vars.liquidationThreshold;
189-194:  vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency(  
                        params.user,
                        currentReserve,
                        vars.assetPrice,
                        vars.assetUnit
                    );
231-232:  vars.avgERC721LiquidationThreshold += vars  
                        .liquidationThreshold;
233-234:  vars.totalERC721CollateralInBaseCurrency += vars  
                        .userBalanceInBaseCurrency;
235-236:  vars.totalCollateralInBaseCurrency += vars  
                        .userBalanceInBaseCurrency;
237-238:  vars.avgLtv += vars.ltv;  
                    vars.avgLiquidationThreshold += vars.liquidationThreshold; 
380-384:  totalValue += _getTokenPrice(  
                        params.oracle,
                        vars.currentReserveAddress,
                        tokenId
                    );
479:  totalValue += tokenPrice;
496:  totalLTV += tmpLTV * tokenPrice;
497-500:  totalLiquidationThreshold +=  
                    tmpLiquidationThreshold *
                    tokenPrice;
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

83:  vars.ethLeft -= _buyWithCredit(
205:  vars.ethLeft -= _buyWithCredit(
397:  price += item.startAmount;
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
77:  amountToWithdraw += _nfts[index].amount;
166:  amountToWithdraw += _nftPairs[index].amount;
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

215:  totalAmount += getTokenIdStakingAmount(
257:  apeStakedAmount += bakcStakedAmount;
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

146:  erc721Data.userState[from].collateralizedBalance -= 1;
318:  erc721Data.userState[user].balance -= balanceToBurn;
```


### [GAS-05] Arithmetic operations that will not over or under flow should made uncheck  

*Instances (3)*:
```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

442:   while (arr[uint256(i)] < pivot) i++;   
449:   i++;
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolParameters.sol

134:  ps._reservesCount++; 
```

### [GAS-06] Function that guaranteed to revert when called by normal user can be marked payable 
If a function modifier such as onlyOwner or onlyPool, onlyPoolAdmin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 


*Instances (36)*:
```solidity
File:   paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

56-60:  function setMarketId(string memory newMarketId)
           external
           override
           onlyOwner  
         {

70-73:  function setAddress(bytes32 id, address newAddress)
        external
        override
        onlyOwner 

81-84: function setAddressAsProxy(bytes32 id, address newImplementationAddress)
        external
        override
        onlyOwner

105-109: function updatePoolImpl(
               IParaProxy.ProxyImplementation[] calldata implementationParams,
              address _init,
              bytes calldata _calldata
              ) external override onlyOwner {

121-124: function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
               external
               override
               onlyOwner

142-145: function setPriceOracle(address newPriceOracle)   
               external
               override
               onlyOwner

158: function setACLManager(address newAclManager) external override onlyOwner {
170: function setACLAdmin(address newAclAdmin) external override onlyOwner { 
182: function setPriceOracleSentinel(address newPriceOracleSentinel)

224-227: function setProtocolDataProvider(address newDataProvider)  
               external
               override
               onlyOwner
235: function setWETH(address newWETH) external override onlyOwner { 

242-248: function setMarketplace(  
        bytes32 id,
        address marketplace,
        address adapter,
        address operator,
        bool paused
    ) external override onlyOwner {

```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NToken.sol

79-82:  function mint(
        address onBehalfOf,
        DataTypes.ERC721SupplyParams[] calldata tokenData
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

87-91:  function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

119-123: function transferOnLiquidation(
        address from,
        address to,
        uint256 value
    ) external onlyPool nonReentrant { 

136-140:  function rescueERC721(
        address token,
        address to,
        uint256[] calldata ids
    ) external override onlyPoolAdmin {

127-131: function rescueERC20(
        address token,
        address to,
        uint256 amount
    ) external override onlyPoolAdmin {

151-157:  function rescueERC1155(
        address token,
        address to,
        uint256[] calldata ids,
        uint256[] calldata amounts,
        bytes calldata data
    ) external override onlyPoolAdmin {

168-171:  function executeAirdrop(
        address airdropContract,
        bytes calldata airdropParams 
    ) external override onlyPoolAdmin {
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

102-106:  function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
136:  function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin { 
159-163:  function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)
        external
        onlyPool  
        nonReentrant
    {  

```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

26-30:  function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool  
        nonReentrant
    {
39-43:  function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool  
        nonReentrant
    {
52-55:  function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {
66-70:  function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool  
        nonReentrant
    {
82-85:  function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {
97-100:  function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {
```

```solidity
File:   paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

26-29:  function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool  
        nonReentrant
39-42:  function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool  
        nonReentrant
52-55:  function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {  
66-69:  function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool   
        nonReentrant
82-85:  function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {
97-100:  function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant { 
```
```solidity
File:   paraspace-core/contracts/ui/WPunkGateway.sol

202-206:  function emergencyERC721TokenTransfer(
        address token,
        uint256 tokenId,
        address to
    ) external onlyOwner {

217-219:  function emergencyPunkTransfer(address to, uint256 punkIndex)
                external
                onlyOwner
```

### [GAS-07] >= COSTS LESS GAS THAN >

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas

*Instances (12)*:
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

67:  params.reservesCount < params.maxNumberReserves, 
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

364-365:  bool isWithdrawCollateral = (newCollateralizedBalance <
            oldCollateralizedBalance);   
```
```solidity
File:   paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

341-344:  require(
            vars.healthFactor > HEALTH_FACTOR_LIQUIDATION_THRESHOLD, 
            Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
        );
401-404:  require(
            (variableDebtPreviousIndex < reserveCache.nextVariableBorrowIndex),  
            Errors.SAME_BLOCK_BORROW_REPAY
        );
557-558:  params.healthFactor <
                MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD ||
565-566:  params.healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
            Errors.HEALTH_FACTOR_NOT_BELOW_THRESHOLD
656:  params.healthFactor < params.auctionRecoveryHealthFactor,
667:  params.healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
834:  params.erc721HealthFactor > params.auctionRecoveryHealthFactor,
```
```solidity
File:   paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

356-360:  require(
                getUserHf(positionOwner) <
                    DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,  
                Errors.HEALTH_FACTOR_NOT_BELOW_THRESHOLD
            );
```

```solidity
File:   paraspace-core/contracts/protocol/pool/PoolParameters.sol

281-284:  require(
            erc721HealthFactor > ps._auctionRecoveryHealthFactor, 
            Errors.ERC721_HEALTH_FACTOR_NOT_ABOVE_THRESHOLD
        );
```
```solidity
File:   paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
67-70:  require(
            incentive < PercentageMath.HALF_PERCENTAGE_FACTOR,  
            "Value Too High"
        );

```

### [GAS-8] Function that could be external

*Instances (1)*:
```solidity
File:   paraspace-core/contracts/ui/WPunkGateway.sol

232:  function onERC721Received(
```
