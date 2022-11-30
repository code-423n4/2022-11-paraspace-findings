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

##

##  [GAS-2]  Use uint256 instead of uint8 . uint8 Can Increase Gas Cost. Possible to save 6 gas as per Remix IDE gas reports

  ####  A smart contract's gas consumption can be higher if developers use items that are less than 32 bytes in size because the Ethereum Virtual Machine can only handle 32 bytes at a time. In order to increase the element's size to the necessary size, the EVM has to perform additional operations. 

        File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

       9:    uint8 constant MIN_ORACLES_NUM = 3;

      12:   uint128 constant EXPIRATION_PERIOD = 1800;   //21505 uint128,uint128 Execution cost   // 21493   uint256,uint256 Execution cost 

      14:   uint128 constant MAX_DEVIATION_RATE = 150;  //21505 uint128,uint128 Execution cost   // 21493   uint256,uint256 Execution cost 

      330:  uint8 feederIndex = feederPositionMap[_feeder].index;  

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

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

##

##   [GAS -6] Use uint256 instead of uint128 in Function parameters . So Possible to save 16 gas for every variables and function calls as per remix gas report. 

                    File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

                   175:     function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation) 

                    343:    function _setConfig(uint128 _expirationPeriod, uint128 _maxPriceDeviation)


 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 

##


##   [GAS-7]    For || operator checks we don't want to check all conditions. As per || results any one condition true over all condition checks become true. If any one condition is true then other condition checks are waste of computing power and gas . So its better we can avoid condition checks after true 

               File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

               362:   if (_priorTwap == 0 || _updatedAt == 0) {
           
 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 

##

##   [GAS - 8]  <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES . FOR EVERY CALL CAN SAVE 13 GAS 

          File: 2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

         149:    token0Amount += positionData.tokensOwed0;

          150:   token1Amount += positionData.tokensOwed1;

          211    sum += getTokenPrice(tokenIds[index]);

          
[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol) 





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