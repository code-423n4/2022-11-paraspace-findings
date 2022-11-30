## USE CALLDATA INSTEAD OF MEMORY

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

When arguments are read-only on external functions, the data location should be `calldata`

Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol lines 99, 100, 290, 320
-paraspace-core\contracts\misc\UniswapV3OracleWrapper.sol lines 114, 145, 221, 282
-paraspace-core\contracts\misc\marketplaces\LooksRareAdapter.sol line 25
-paraspace-core\contracts\misc\marketplaces\SeaportAdapter.sol lines 25, 60
-paraspace-core\contracts\misc\marketplaces\X2Y2Adapter.sol line 31
-paraspace-core\contracts\protocol\configuration\PoolAddressesProvider.sol line 56
-paraspace-core\contracts\protocol\libraries\logic\BorrowLogic.sol lines 56, 130
-paraspace-core\contracts\protocol\libraries\logic\BorrowLogic.sol line 30
-paraspace-core\contracts\protocol\libraries\logic\GenericLogic.sol lines 77, 363, 364, 452, 453
-paraspace-core\contracts\protocol\libraries\logic\LiquidationLogic.sol lines 165, 290, 437, 438, 466, 467, 491, 492, 525, 526, 567, 568, 606, 607, 661, 662, 759, 760, 866, 867
-paraspace-core\contracts\protocol\libraries\logic\LiquidationLogic.sol lines 118, 335, 377, 378, 429, 430, 466, 467
-paraspace-core\contracts\protocol\libraries\logic\SupplyLogic.sol lines 84, 138, 170, 231, 274

## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

Reading array length at each iteration of the loop consumes more gas than necessary.
In the best case scenario (length read on a memory variable), caching the array length in the stack saves around 3 gas per iteration. In the worst case scenario (external calls at each iteration), the amount of gas wasted can be massive.

Consider storing the array’s length in a variable before the for-loop, and use this new variable instead

Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol lines 229, 291, 321
-paraspace-core\contracts\misc\ParaSpaceOracle.sol lines 95, 197
-paraspace-core\contracts\misc\UniswapV3OracleWrapper.sol lines 193, 210
-paraspace-core\contracts\protocol\libraries\logic\FlashClaimLogic.sol lines 38, 56
-paraspace-core\contracts\protocol\libraries\logic\MarketplaceLogic.sol lines 382, 470
-paraspace-core\contracts\protocol\libraries\logic\SupplyLogic.sol lines 184, 195
-paraspace-core\contracts\protocol\libraries\logic\ValidationLogic.sol lines 212, 465, 910, 1034
-paraspace-core\contracts\protocol\pool\PoolApeStaking.sol lines 72, 103, 138, 199, 215, 279, 289, 305
-paraspace-core\contracts\protocol\tokenization\NToken.sol lines 106, 145
-paraspace-core\contracts\protocol\tokenization\NTokenApeStaking.sol line 107
-paraspace-core\contracts\protocol\tokenization\NTokenMoonBirds.sol lines 51, 97
-paraspace-core\contracts\protocol\tokenization\base\MintableIncentivizedERC721.sol line 493
-paraspace-core\contracts\protocol\tokenization\libraries\MintableERC721Logic.sol lines 207, 280
-paraspace-core\contracts\ui\WPunkGateway.sol lines 82, 109, 113, 136, 174

## USING STORAGE INSTEAD OF MEMORY FOR STRUCTS/ARRAYS SAVES GAS

When fetching data from a storage location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another memory array/struct

Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol lines 357, 414
-paraspace-core\contracts\protocol\configuration\PoolAddressesProvider.sol line 212 
-paraspace-core\contracts\protocol\libraries\logic\GenericLogic.sol lines 421, 424


## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

Instances:
-paraspace-core\contracts\misc\UniswapV3OracleWrapper.sol lines 149, 150, 211
-paraspace-core\contracts\protocol\libraries\logic\GenericLogic.sol lines 169, 176, 178, 185, 189, 231, 233, 235, 237, 238, 380, 479, 496, 497
-paraspace-core\contracts\protocol\libraries\logic\MarketplaceLogic.sol line 397
-paraspace-core\contracts\protocol\pool\PoolApeStaking.sol 77, 166
-paraspace-core\contracts\protocol\tokenization\libraries\ApeStakingLogic.sol lines 214, 215 


## `++I` INSTEAD OF `I++` (OR USE ASSEMBLY WHEN APPLICABLE)

Use `++i` instead of ` i++`. This is especially useful in for loops but this optimization can be used anywhere in your code. 

Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol lines 229, 291, 321, 413, 421, 442, 449 
-paraspace-core\contracts\misc\ParaSpaceOracle.sol lines 95, 197
-paraspace-core\contracts\misc\UniswapV3OracleWrapper.sol lines 193, 210
-paraspace-core\contracts\protocol\libraries\logic\FlashClaimLogic.sol lines 38, 56
-paraspace-core\contracts\protocol\libraries\logic\MarketplaceLogic.sol lines 177,284, 382, 470
-paraspace-core\contracts\protocol\libraries\logic\SupplyLogic.sol lines 184, 195
-paraspace-core\contracts\protocol\libraries\logic\ValidationLogic.sol lines 131, 212, 465, 910, 1034
-paraspace-core\contracts\protocol\pool\PoolApeStaking.sol lines 72, 103, 138, 164, 172, 199, 215, 279, 289, 305
-paraspace-core\contracts\protocol\pool\PoolCore.sol lines 634, 638
-paraspace-core\contracts\protocol\pool\PoolParameters.sol
line 134
-paraspace-core\contracts\protocol\tokenization\NToken.sol lines 106, 145
-paraspace-core\contracts\protocol\tokenization\NTokenApeStaking.sol line 107
-paraspace-core\contracts\protocol\tokenization\NTokenMoonBirds.sol lines 51, 97
-paraspace-core\contracts\protocol\tokenization\base\MintableIncentivizedERC721.sol line 493
-paraspace-core\contracts\protocol\tokenization\libraries\MintableERC721Logic.sol lines 207, 234, 280, 304, 313
-paraspace-core\contracts\ui\WPunkGateway.sol lines 82, 109, 113, 136, 174


## USE MULTIPLE `REQUIRE()` STATMENTS INSTEAD OF `REQUIRE(EXPRESSION && EXPRESSION && ...)`

Instances:
-paraspace-core\contracts\protocol\pool\PoolParameters.sol lines 171, 279
-paraspace-core\contracts\protocol\libraries\logic\ValidationLogic.sol lines 546, 550, 636, 640, 1196, 1202, 1208, 

## IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {


Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol lines 229, 291, 321, 413
-paraspace-core\contracts\misc\ParaSpaceOracle.sol lines 95, 197
-paraspace-core\contracts\misc\UniswapV3OracleWrapper.sol lines 193, 210
-paraspace-core\contracts\protocol\libraries\logic\FlashClaimLogic.sol lines 38, 56
-paraspace-core\contracts\protocol\libraries\logic\GenericLogic.sol lines 371, 466
-paraspace-core\contracts\protocol\libraries\logic\MarketplaceLogic.sol lines 177, 284, 382, 470
-paraspace-core\contracts\protocol\libraries\logic\SupplyLogic.sol lines 184, 195
-paraspace-core\contracts\protocol\libraries\logic\ValidationLogic.sol lines 131, 212, 465, 910, 1034
-paraspace-core\contracts\protocol\pool\PoolApeStaking.sol lines 72, 103, 138, 172, 199, 215, 279, 289, 305
-paraspace-core\contracts\protocol\pool\PoolCore.sol line 634
-paraspace-core\contracts\protocol\tokenization\NToken.sol lines 106, 145
-paraspace-core\contracts\protocol\tokenization\NTokenApeStaking.sol line 107
-paraspace-core\contracts\protocol\tokenization\NTokenMoonBirds.sol lines 51, 97
-paraspace-core\contracts\protocol\tokenization\base\MintableIncentivizedERC721.sol line 493
-paraspace-core\contracts\protocol\tokenization\libraries\MintableERC721Logic.sol lines 207, 280
-paraspace-core\contracts\ui\WPunkGateway.sol lines 82, 109, 113, 136, 174


## `++I/I++` SHOULD BE `UNCHECKED{++I}`/`UNCHECKED{I++}` WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
`
   for (uint256 i = 0; i < orders.length; /** NOTE: Removed i++ **/ ) {
           // Do the thing
           // Unchecked pre-increment is cheapest
           unchecked { ++i; }   
}  `

Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol lines 229, 291, 321, 413
-paraspace-core\contracts\misc\ParaSpaceOracle.sol lines 95, 197
-paraspace-core\contracts\misc\UniswapV3OracleWrapper.sol lines 193, 210
-paraspace-core\contracts\protocol\libraries\logic\FlashClaimLogic.sol lines 38, 56
-paraspace-core\contracts\protocol\libraries\logic\GenericLogic.sol lines 371, 466
-paraspace-core\contracts\protocol\libraries\logic\MarketplaceLogic.sol lines 177, 284, 382, 470
-paraspace-core\contracts\protocol\libraries\logic\SupplyLogic.sol lines 184, 195
-paraspace-core\contracts\protocol\libraries\logic\ValidationLogic.sol lines 131, 212, 465, 910, 1034
-paraspace-core\contracts\protocol\pool\PoolApeStaking.sol lines 72, 103, 138, 172, 199, 215, 279, 289, 305
-paraspace-core\contracts\protocol\pool\PoolCore.sol line 634
-paraspace-core\contracts\protocol\tokenization\NToken.sol lines 106, 145
-paraspace-core\contracts\protocol\tokenization\NTokenApeStaking.sol line 107
-paraspace-core\contracts\protocol\tokenization\NTokenMoonBirds.sol lines 51, 97
-paraspace-core\contracts\protocol\tokenization\base\MintableIncentivizedERC721.sol line 493
-paraspace-core\contracts\protocol\tokenization\libraries\MintableERC721Logic.sol lines 207, 280
-paraspace-core\contracts\ui\WPunkGateway.sol lines 82, 109, 113, 136, 174


## USING `PRIVATE` RATHER THAN `PUBLIC` FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

Instances:
-paraspace-core\contracts\misc\NFTFloorOracle.sol line 70
-paraspace-core\contracts\protocol\libraries\logic\LiquidationLogic.sol lines 57, 64
-paraspace-core\contracts\protocol\libraries\logic\ValidationLogic.sol lines 45, 49, 56
-paraspace-core\contracts\protocol\pool\PoolCore.sol line 53
-paraspace-core\contracts\protocol\tokenization\NToken.sol line 32

## USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

## RESULTS OF CALLS TO _MSGSENDER() NOT CACHED

##STATE VARIABLES SHOULD BE CACHED IN STACK VARIABLES RATHER THAN RE-READING THEM FROM STORAGE
Check multiple storage accesses 

Instances:
-paraspace-core\contracts\protocol\tokenization\base\MintableIncentivizedERC721.sol line 196, 

## FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

Instances:
-paraspace-core\contracts\protocol\configuration\PoolAddressesProvider.sol lines 59, 73, 84, 109, 124, 154, 158, 170, 185, 227, 235, 248
-paraspace-core\contracts\ui\WPunkGateway.sol lines 206, 219
