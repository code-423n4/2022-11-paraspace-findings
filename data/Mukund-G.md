## 1. USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS
When a function with a memory array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it’s still valid for implementation `contracs` to use `calldata` arguments instead.

[PoolCore.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L626)

## 2.USING FUNCTIONS INSTEAD OF MODIFIERS SAVES GAS
if its possible use functions instead of modifiers
[NFTFloorOracle.sol#L110-L115](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L110-L115)
[NFTFloorOracle.sol#L117-L125](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L117-L125)
[NFTFloorOracle.sol#L127-L130](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L127-L130)
[NFTFloorOracle.sol#L132-L135](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L132-L135)
there are many instance of this in whole codebase 


## 3.THE RESULT OF FUNCTION CALLS SHOULD BE CACHED RATHER THAN RE-CALLING THE FUNCTION
here `poolStorage()` function is called in every function instead of calling it in every function save its value in global state 
[PoolApeStaking.sol#L64](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L64)
[PoolApeStaking.sol#L97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L97)
[PoolApeStaking.sol#L124](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L124)
[PoolApeStaking.sol#L192](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L192)
[PoolApeStaking.sol#L238](
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L238)
there are many instances like this in different contracts


## 4.USE UNCHECK BLOCK FOR OPERATION THAT CANNOT OVERFLOW/UNDERFLOW 
example use uncheck block for `++` operand in for loop to save gas because it not likely to overflow and solidity ^0.8.0 automatically check for overflow and underflow which cost gas to avoid that we use uncheck block
[PoolApeStaking#L72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)
[PoolApeStaking#L103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103)
[PoolApeStaking#L138](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138)
[PoolApeStaking#L199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199)
[PoolApeStaking#L72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)


## 5.FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
[PoolParameters#L139-L143](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139-L143)
[PoolParameters#L166-L169](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166-L169)
[PoolParameters#L181-L184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181-L184)


## 6.<X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES
Using the addition operator instead of plus-equals saves gas
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L257


## 7.USAGE OF UINT SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
[NFTFloorOracle.sol#L9](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9)
[NFTFloorOracle.sol#L12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12)
[NFTFloorOracle.sol#L14](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14)
[IncentivizedERC20.sol#L65](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L65)
[IncentivizedERC20.sol#L81](
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L81)


## 8.EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT
[DebtTokenBase.sol#L25-L28](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol#L25-L28)
[EIP712Base.sol#L11-L14](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol#L11-L14)

## 9. use a more recent version of solidity
solidity 8.17 is available now.

## 10. `abi.encode()` is less efficient than `abi.encodepacked()`
[ValidationLogic.sol#L1124](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1124)

## 11. Using private rather than public for constant saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table
[LiquidationLogic.sol#L57](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L57)
[LiquidationLogic.sol#L64](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L64)
[ValidationLogic.sol#L45](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45)
[ValidationLogic.sol#L49](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L49)
[ValidationLogic.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L56)

## NOTE I HAVE NOT MENTIONED ALL INSTANCE OF ABOVE ISSUES BECAUSE REPORT WAS BECOMING MESSY