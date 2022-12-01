##

##   [L-1]   USE A MORE RECENT VERSION OF SOLIDITY

   
  ####  latest solidity version is 0.8.17 . Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>)

There are 22 instances of this issue

      pragma solidity 0.8.10;


##

##  [L-2]  NOT USING THE NAMED RETURN VARIABLES ANYWHERE IN THE FUNCTION IS CONFUSING

  ####   Consider changing the variable to be an unnamed one

        File:  2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

        function getAskOrderInfo(bytes memory params, address weth)
        external
        pure
        override
        returns (DataTypes.OrderInfo memory orderInfo)  @Audit  NAMED RETURN VARIABLES


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

        File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol


       function getPrice(address _asset)
        external
        view
        override
        returns (uint256 price) 

 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol) 

        2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol


         function getLiquidityAmount(uint256 tokenId)
        external
        view
        returns (uint256 token0Amount, uint256 token1Amount)

      function getLiquidityAmountFromPositionData(
        UinswapV3PositionData memory positionData
    ) public pure returns (uint256 token0Amount, uint256 token1Amount) {

      function getLpFeeAmount(uint256 tokenId)
        external
        view
        returns (uint256 token0Amount, uint256 token1Amount)

        function getLpFeeAmountFromPositionData(
        UinswapV3PositionData memory positionData
    ) public view returns (uint256 token0Amount, uint256 token1Amount) {

 [Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol) 


     2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

        function executeGetUserAccountData(
        address user,
        DataTypes.PoolStorage storage ps,
        address oracle
         )
        external
        view
        returns (
            uint256 totalCollateralBase,
            uint256 totalDebtBase,
            uint256 availableBorrowsBase,
            uint256 currentLiquidationThreshold,
            uint256 ltv,
            uint256 healthFactor,
            uint256 erc721HealthFactor
           )


[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol)

##

## [L-3]   STRUCT SHOULD BE DECLARED IN THE DECLARATION SECTIONS. IN ValidationLogic.sol FILE STRUCT DECLARED IN CENTER OF THE CONTRACT. THIS IS NOT A GOOD CODE PRACTICE. 

[ValidationLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L225-L245)

[ValidationLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L478-L492)




##

##   [L-4 ]    CONSTANTS SHOULD BE DEFINED RATHER THAN USING MAGIC NUMBERS

 ####  It is bad practice to use numbers directly in code without explanation

    File:  2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

          here length is checked with 2 without any explanations. Why checked the length is set 2 

            require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)


            2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol

        17:     bytes32 constant POOL_STORAGE_POSITION =
                  bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);

##

## [L-5]  EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

  ####   While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand. There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts. constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.


         File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

         70:     bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE"); 

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)


        2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol

        17:     bytes32 constant POOL_STORAGE_POSITION =
                  bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);


##






--------------------------------------------------------------------------------------------------------------------------------------------------------------

NC-1	Missing checks for address(0) when assigning values to address state variables	4
NC-2	Return values of approve() not checked	3
NC-3	Event is missing indexed fields	8
NC-4	Constants should be defined rather than using magic numbers	2
NC-5	Functions not used internally could be marked external	4


L-1	abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()	3
L-2	Do not use deprecated library functions	4
L-3	Unsafe ERC20 operation(s)
        

