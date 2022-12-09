## USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS
When a function with a memory array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it’s still valid for implementation `contracs` to use `calldata` arguments instead.

[PoolCore.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L626)

## USING FUNCTIONS INSTEAD OF MODIFIERS SAVES GAS
if its possible use functions instead of modifiers
[NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L110-L115)
[NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L117-L125)
[NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L127-L130)
[NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L132-L135)
there are many instance of this in whole codebase 


## THE RESULT OF FUNCTION CALLS SHOULD BE CACHED RATHER THAN RE-CALLING THE FUNCTION
here `poolStorage()` function is called in every function instead of calling it in every function save its value in global state 
[PoolApeStaking.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L64)
[PoolApeStaking.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L97)
[PoolApeStaking.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L124)
[PoolApeStaking.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L192)
[PoolApeStaking.sol](
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L238)
there are many instances like this in different contracts

## USE UNCHECK BLOCK FOR OPERATION THAT CANNOT OVERFLOW/UNDERFLOW 
example use uncheck block for `++` operand in for loop to save gas because it not likely to overflow and solidity ^0.8.0 automatically check for overflow and underflow which cost gas to avoid that we use uncheck block
[PoolApeStaking](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)
[PoolApeStaking](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103)
[PoolApeStaking](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138)
[PoolApeStaking](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199)
[PoolApeStaking](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)

## FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
[PoolParameters](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139-L143)
[PoolParameters](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166-L169)
[PoolParameters](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181-L184)

## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES
Using the addition operator instead of plus-equals saves gas
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L257

## USAGE OF UINT SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
using uint smaller than uint256 cost more gas so its better to use uint256
[NFTFloorOracle](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9)
[NFTFloorOracle](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12)
[NFTFloorOracle](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14)
[NFTFloorOracle](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L65)
[NFTFloorOracle](
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L81)

