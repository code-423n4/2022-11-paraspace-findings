##

##   [GAS -1]    instead of using operator && on single require or if checks . Using Multiple REQUIRE or IF checks can save more gas. 


          2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

           require(
            // NOT criteria based and must be basic order
            resolvers.length == 0 && isBasicOrder(advancedOrder),
            Errors.INVALID_MARKETPLACE_ORDER
            );


          require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&          
                isBasicOrder(advancedOrders[0]) &&  
                isBasicOrder(advancedOrders[1]),
            Errors.INVALID_MARKETPLACE_ORDER
        );

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)


             File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

             331:  if (feederIndex >= 0 && feeders[feederIndex] == _feeder) { 

  [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 

            File:  2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol

             130:  if (price == 0 && address(_fallbackOracle) != address(0)) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol) 

            File:    2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol

           151:    if (params.usePTokens && params.amount == type(uint256).max) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol) 

           2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol


           if (
            superVars.auctionEnabled &&
            IAuctionableERC721(superVars.collateralXToken).isAuctioned(
                params.collateralTokenId
            )

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol) 


  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

              171:   require(
                    marketplaceIds.length == payloads.length &&
                    payloads.length == credits.length,
                    Errors.INCONSISTENT_PARAMS_LENGTH
                     );

            279:  require(
            marketplaceIds.length == payloads.length &&
                payloads.length == credits.length,
            Errors.INCONSISTENT_PARAMS_LENGTH
             );

           579:   if (
            vars.ethLeft > 0 &&
            orderInfo.consideration[0].itemType != ItemType.NATIVE
        ) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspacecore/contracts/protocol/libraries/logic/MarketplaceLogic.sol)


                  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

                  474 :   if (params.from != params.to && params.amount != 0) { 

                  662:    if (oldCollateralizedBalance == 0 && newCollateralizedBalance != 0) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol) 


2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol


             546:  require(
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
              );
            550:   require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
            );

         636:   require(
            vars.collateralReserveActive && vars.principalReserveActive,
            Errors.RESERVE_INACTIVE
              );
        640:    require(
            !vars.collateralReservePaused && !vars.principalReservePaused,
            Errors.RESERVE_PAUSED
             );

        672:   require(
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&
                (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
            Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
        );

        1202:   require(
                !vars.token0IsPaused && !vars.token1IsPaused,
                Errors.RESERVE_PAUSED
            );

         1208:  require(
                !vars.token0IsFrozen && !vars.token1IsFrozen,
                Errors.RESERVE_FROZEN
            );


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol) 



        2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol

        216:    require(
            value > MIN_AUCTION_HEALTH_FACTOR &&
                value <= MAX_AUCTION_HEALTH_FACTOR,
            Errors.INVALID_AMOUNT
              );


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol) 


 File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

           111 :  if (from != to && erc721Data.auctions[tokenId].startTime > 0) {

          145:    if (from != to && isUsedAsCollateral_) {

          229:    if (
                tokenData[index].useAsCollateral &&
                !erc721Data.isUsedAsCollateral[tokenId]
            )




[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 




       
                


##

##  [GAS-2]  Use uint256 instead of uint8 . uint8 Can Increase Gas Cost. Possible to save 6 gas as per Remix IDE gas reports

  ####  A smart contract's gas consumption can be higher if developers use items that are less than 32 bytes in size because the Ethereum Virtual Machine can only handle 32 bytes at a time. In order to increase the element's size to the necessary size, the EVM has to perform additional operations. 

        File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

       9:    uint8 constant MIN_ORACLES_NUM = 3;

      12:   uint128 constant EXPIRATION_PERIOD = 1800;   //21505 uint128,uint128 Execution cost   // 21493   uint256,uint256 Execution cost 

      14:   uint128 constant MAX_DEVIATION_RATE = 150;  //21505 uint128,uint128 Execution cost   // 21493   uint256,uint256 Execution cost 

      330:  uint8 feederIndex = feederPositionMap[_feeder].index;  

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)


2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol


        function verifyCreditSignature(
        DataTypes.Credit memory credit,
        address signer,
        uint8 v,   //@AUDIT UINT256 
        bytes32 r,
        bytes32 s
        ) private view returns (bool) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol) 

        

##

##   [GAS-3]  Compute known value off-chain

####   If You know what data to hash, there is no need to consume more computational power to hash it using keccak256 , you’ll end up consuming 2x amount of gas.

          File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

          70:     bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
  
[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

##

##   [GAS-4]   FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

####    If a function modifier such as onlyOwner,onlyRole is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

        File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

        function addAssets(address[] calldata _assets)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)  

       function removeAsset(address _asset) 
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
        onlyWhenAssetExisted(_asset)

        function addFeeders(address[] calldata _feeders) 
        external
        onlyRole(DEFAULT_ADMIN_ROLE)

       function removeFeeder(address _feeder)
        external
        onlyWhenFeederExisted(_feeder) 

        function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation) 
        external
        onlyRole(DEFAULT_ADMIN_ROLE)

        function setPause(address _asset, bool _flag) 
        external
        onlyRole(DEFAULT_ADMIN_ROLE)

       function setPrice(address _asset, uint256 _twap)
        public
        onlyRole(UPDATER_ROLE)
        onlyWhenAssetExisted(_asset)
        whenNotPaused(_asset)


       function setMultiplePrices(
        address[] calldata _assets,
        uint256[] calldata _twaps
    ) external onlyRole(UPDATER_ROLE) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

        2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

        function setAddressAsProxy(bytes32 id, address newImplementationAddress)
        external
        override
        onlyOwner

        function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
        external
        override
        onlyOwner

        function setWETH(address newWETH) external override onlyOwner {

       function setMarketplace(
        bytes32 id,
        address marketplace,
        address adapter,
        address operator,
        bool paused
    ) external override onlyOwner {

      


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspacecore/contracts/protocol/configuration/PoolAddressesProvider.sol)

##

## [GAS-5]  ++I/I++ OR --I/I-- SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} OR  UNCHECKED{--I}/UNCHECKED{I--}WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

     ####   The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

           File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

          229:  for (uint256 i = 0; i < _assets.length; i++) {

          291:   for (uint256 i = 0; i < _assets.length; i++) { 

          321:    for (uint256 i = 0; i < _feeders.length; i++) { 

          413:    for (uint256 i = 0; i < feederSize; i++) { 

         421:     validNum++;

         442:    while (arr[uint256(i)] < pivot) i++;

         443:     while (pivot < arr[uint256(j)]) j--;

         449:       i++;
               
         450:       j--;

       [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 


        File:  2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol

         95:      for (uint256 i = 0; i < assets.length; i++) {

         197:    for (uint256 i = 0; i < assets.length; i++) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol) 

         File: 2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

        193:   for (uint256 index = 0; index < tokenIds.length; index++) {

       210 :   for (uint256 index = 0; index < tokenIds.length; index++) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol) 

       File:    2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

        38:    for (i = 0; i < params.nftTokenIds.length; i++) {

        56:     for (i = 0; i < params.nftTokenIds.length; i++) {

 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol) 

       File:    2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

       466:    for (uint256 index = 0; index < totalBalance; index++) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol) 


      2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

      177:    for (uint256 i = 0; i < marketplaceIds.length; i++) {

      284:    for (uint256 i = 0; i < marketplaceIds.length; i++) {

     382:     for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

     470:     for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspacecore/contracts/protocol/libraries/logic/MarketplaceLogic.sol) 


       2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol
 
         58:     for (uint16 i = 0; i < params.reservesCount; i++) {

         104:   for (uint256 i = 0; i < assets.length; i++) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol) 


        2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

          195:    for (uint256 index = 0; index < params.tokenData.length; index++) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol) 

 
         2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

  
           131:      for (uint256 index = 0; index < amount; index++) {

           212 :     for (uint256 index = 0; index < tokenIds.length; index++) {

          465:        for (uint256 index = 0; index < tokenIds.length; index++) {

          910:       for (uint256 index = 0; index < tokenIds.length; index++) {

          1034:     for (uint256 i = 0; i < params.nftTokenIds.length; i++) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol) 


          2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

          72:    for (uint256 index = 0; index < _nfts.length; index++) {

         103:   for (uint256 index = 0; index < _nfts.length; index++) {

         138:   for (uint256 index = 0; index < _nftPairs.length; index++) {

         172:   for (uint256 index = 0; index < actualTransferAmount; index++) {

         199:    for (uint256 index = 0; index < _nftPairs.length; index++) {

         215:    for (uint256 index = 0; index < _nftPairs.length; index++) {

         279:    for (uint256 index = 0; index < _nfts.length; index++) {

         289:    for (uint256 index = 0; index < _nftPairs.length; index++) {

        305:     for (uint256 index = 0; index < _nftPairs.length; index++) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol)


          File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol


           634:   for (uint256 i = 0; i < reservesListCount; i++) { 

           638:    droppedReservesCount++;


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol)


       File:  2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

             82:       for (uint256 i = 0; i < punkIndexes.length; i++) {

            109:     for (uint256 i = 0; i < punkIndexes.length; i++) {

            113:     for (uint256 i = 0; i < punkIndexes.length; i++) {

            136:     for (uint256 i = 0; i < punkIndexes.length; i++) {

           174:      for (uint256 i = 0; i < punkIndexes.length; i++) {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol) 


             File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

             231:       for (uint256 index = 0; index < totalBalance; index++) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol) 


           File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

             207:    for (uint256 index = 0; index < tokenData.length; index++) 
 
             280:    for (uint256 index = 0; index < tokenIds.length; index++) {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 
       
       

          


        

     

##

##   [GAS -6] Use uint256 instead of uint128 in Function parameters . So Possible to save 16 gas for every variables and function calls as per remix gas report. 

                    File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

                   175:     function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation) 

                    343:    function _setConfig(uint128 _expirationPeriod, uint128 _maxPriceDeviation)


 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 



              2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol


               237:   function decreaseUniswapV3Liquidity(
                       address asset,
                       uint256 tokenId,
                      uint128 liquidityDecrease,        //@audit uint256
                        uint256 amount0Min,
                   uint256 amount1Min,
                   bool receiveEthAsWeth
                   ) external virtual override nonReentrant {


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol) 


               






##


##   [GAS-7]    For || operator checks we don't want to check all conditions. As per || results any one condition true over all condition checks become true. If any one condition is true then other condition checks are waste of computing power and gas . So its better we can avoid condition checks after true 

               File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

               362:   if (_priorTwap == 0 || _updatedAt == 0) {
           
 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 

           2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

             388:   require(
                item.itemType == ItemType.ERC20 ||
                    (vars.isETH && item.itemType == ItemType.NATIVE),
                Errors.INVALID_ASSET_TYPE
            );

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspacecore/contracts/protocol/libraries/logic/MarketplaceLogic.sol)

##

##   [GAS - 8]  <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES . FOR EVERY CALL CAN SAVE 13 GAS 

          File: 2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

         149:    token0Amount += positionData.tokensOwed0;

          150:   token1Amount += positionData.tokensOwed1;

          211    sum += getTokenPrice(tokenIds[index]);

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol) 


            File:    2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

            vars.payableDebtByERC20Assets += vars
                        .userBalanceInBaseCurrency
                        .percentDiv(vars.liquidationBonus);

           vars.totalCollateralInBaseCurrency += vars
                        .userBalanceInBaseCurrency;


          vars.avgLiquidationThreshold += vars.liquidationThreshold;

         vars.totalDebtInBaseCurrency += _getUserDebtInBaseCurrency(
                        params.user,
                        currentReserve,
                        vars.assetPrice,
                        vars.assetUnit
                    );

        vars.avgERC721LiquidationThreshold += vars
                        .liquidationThreshold;

                    vars.totalERC721CollateralInBaseCurrency += vars
                        .userBalanceInBaseCurrency;

                    vars.totalCollateralInBaseCurrency += vars
                        .userBalanceInBaseCurrency;

                    vars.avgLtv += vars.ltv;

                    vars.avgLiquidationThreshold += vars.liquidationThreshold;

                  479:    totalValue += tokenPrice;

                  496:   totalLTV += tmpLTV * tokenPrice;

                  497:    totalLiquidationThreshold +=
                             tmpLiquidationThreshold *
                             tokenPrice;

            

          
[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol) 

               2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol    

               397:   price += item.startAmount;

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspacecore/contracts/protocol/libraries/logic/MarketplaceLogic.sol) 


             File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

                   77:   amountToWithdraw += _nfts[index].amount;

                   166:   amountToWithdraw += _nftPairs[index].amount;


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol) 



                File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol


                         215:   totalAmount += getTokenIdStakingAmount(
                                  poolId,
                                _apeCoinStaking,
                                  tokenId
                                   );

                         257:   apeStakedAmount += bakcStakedAmount;


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol) 


         


            

##

##  [GAS - 9] We can use bytes32 instead of string for fixed length inputs

          File:  2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

         22:   string private _marketId;

##

##[GAS-10]   Use uint256 instead of uint16 in Function parameters AND varibales . So Possible to save 16 gas for every variables and function calls as per remix gas report. 


        2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

        function executeBuyWithCredit(
        bytes32 marketplaceId,
        bytes calldata payload,
        DataTypes.Credit calldata credit,
        DataTypes.PoolStorage storage ps,
        IPoolAddressesProvider poolAddressProvider,
        uint16 referralCode                                                      //@AUDIT UINT256
       ) external {

       229:   function executeAcceptBidWithCredit(
        bytes32 marketplaceId,
        bytes calldata payload,
        DataTypes.Credit calldata credit,
        address onBehalfOf,
        DataTypes.PoolStorage storage ps,
        IPoolAddressesProvider poolAddressProvider,
        uint16 referralCode                                                               //@AUDIT UINT256

       267:  function executeBatchAcceptBidWithCredit(
        bytes32[] calldata marketplaceIds,
        bytes[] calldata payloads,
        DataTypes.Credit[] calldata credits,
        address onBehalfOf,
        DataTypes.PoolStorage storage ps,
        IPoolAddressesProvider poolAddressProvider,
        uint16 referralCode                                                                     //@AUDIT UINT256


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspacecore/contracts/protocol/libraries/logic/MarketplaceLogic.sol) 


          2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol
 
         58:     for (uint16 i = 0; i < params.reservesCount; i++) {          //@AUDIT UINT256

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol) 


2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol
             
           function executeSupplyERC721Base(
        uint16 reserveId,                                                          //  @AUDIT UINT256
        address nTokenAddress,
        DataTypes.UserConfigurationMap storage userConfig,
        DataTypes.ExecuteSupplyERC721Params memory params
       ) internal {

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol) 


         2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol

              91:   function supply(
             address asset,
             uint256 amount,
             address onBehalfOf,
            uint16 referralCode                                                   //  @AUDIT UINT256
            ) external virtual override nonReentrant {



          156:   function supplyWithPermit(
        address asset,
        uint256 amount,
        address onBehalfOf,
        uint16 referralCode,                                            //  @AUDIT UINT256
        uint256 deadline,
        uint8 permitV,                                                        //  @AUDIT UINT256
        bytes32 permitR,
        bytes32 permitS
    ) external virtual override nonReentrant {



       function borrow(
        address asset,
        uint256 amount,
        uint16 referralCode,                                                     //  @AUDIT UINT256
        address onBehalfOf
    ) external virtual override nonReentrant {
        DataTypes.PoolStorage storage ps = poolStorage();


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol) 


         2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

         94:   uint16 referralCode;

         114:  uint16 referralCode;

         135:   uint16 referralCode;


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol) 


      File:   2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

     
       80:    uint16 referralCode;

      134:   uint16 referralCode;

     172:    uint16 referralCode


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol) 


        

      
    
##

##    [GAS-11]    Use uint256 instead of uint64  . Can save more gas 

2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol
        
           144:    uint64 oldCollateralizedBalance,

            145:    uint64 newCollateralizedBalance

            356:    uint64 oldCollateralizedBalance,
           
            357:     uint64 newCollateralizedBalance

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol) 

      File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

              42 :      uint64 balanceLimit;

              103:     uint64 oldSenderBalance = erc721Data.userState[from].balance;

              105:     uint64 oldRecipientBalance = erc721Data.userState[to].balance;
        
               106:    uint64 newRecipientBalance = oldRecipientBalance + 1;

               205:    uint64 collateralizedTokens = 0;

               247:    uint64 newBalance = oldBalance + uint64(tokenData.length);

              272:       uint64 burntCollateralizedTokens = 0;
        
               273:      uint64 balanceToBurn;

               405:      uint64 balance;

              408:      uint64 balanceLimit = erc721Data.balanceLimit;

             


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 

            




##

## [GAS-12]  Arithmetic operations can be uncheked if we know operands can't be underflow /Overflow 

             File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

                    185;    uint256 supplyAmount = unstakedAmount - repayAmount; 

                    260:     return apeStakedAmount + apeReward;

                    172:     unstakedAmount = unstakedAmount - incentiveAmount;

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol) 

           File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 

##

## [GAS-13]  In the structure instead of uint64,uint64,uint128 we can use uint256 for all variables .  It will saves 13 gas 



            File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

BEFORE : 

             12:  struct UserState {
             uint64 balance;
              uint64 collateralizedBalance;
             uint128 additionalData;
              }

Before modification the  gas cost is 77126 as per REMIX IDE

AFTER:

            12:  struct UserState {
             uint256 balance;
            uint256 collateralizedBalance;
            uint256 additionalData;
            }

 After modification the  gas cost is 77113 as per  REMIX IDE

So clearly possible to save 13 gas after modifications.

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

##

##   [GAS-14]  WE CAN USE ++X/--X  INSTEAD OF [Y]=X+1 OR [Y]=X-1 . LIKE THIS WAY WE CAN SAVE 63 GAS DURING EXECUTION


          File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

BEFORE :

          104:  erc721Data.userState[from].balance = oldSenderBalance - 1;

          106:   uint64 newRecipientBalance = oldRecipientBalance + 1;

> BEFORE THE EXECUTION GAS COST IS  21617

AFTER:  

           104:  erc721Data.userState[from].balance = --oldSenderBalance ;

          106:   uint64 newRecipientBalance = ++oldRecipientBalance;

 > AFTER THE EXECUTION GAS COST IS  21554


           178:   ? collateralizedBalance + 1
           
           179:   : collateralizedBalance - 1;

           523:   uint256 lastTokenIndex = userBalance - 1;

          552:    uint256 lastTokenIndex = length - 1;

          
[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 

##

## [GAS-15]  WHEN EVER WE NEED TO INCREAMENT OR DECREMENT VALUE BY 1 WE CAN USE ++X/--X INSTEAD OF  [X+=1/X-=1] . IN THIS WAY POSSIBLE TO SAVE 67 GAS FOR EVERY INCREMENT OR DECREMENT OPERATIONS AS PER REMIX GAS REPORTS 

> BEFORE :

X-=1;   THE GAS COST IS 26597

 >  AFTER : 

--X; THE GAS COST IS 26530


            File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

           146 :  erc721Data.userState[from].collateralizedBalance -= 1;

           318:   erc721Data.userState[user].balance -= balanceToBurn;


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 







  

          File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol) 

-------------------------------------------------------------------------------------------------------------------------------------------------------

GAS-1	Use assembly to check for address(0)	40
GAS-2	Using bools for storage incurs overhead	3
GAS-3	Cache array length outside of loop	45
GAS-4	State variables should be cached in stack variables rather than re-reading them from storage	4
GAS-5	Use calldata instead of memory for function arguments that do not get mutated	39
GAS-6	Use Custom Errors	16
GAS-7	Don't initialize variables with default value	59
GAS-8	++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)	58
GAS-9	Using private rather than public for constants, saves gas	8
GAS-10	Use shift Right/Left instead of division/multiplication if possible	2
GAS-11	Use != 0 instead of > 0 for unsigned integer comparison	23
GAS-12	internal functions not called by the contract should be removed	24