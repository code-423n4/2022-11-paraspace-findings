 # UNUSED STATE VARIABLE 
One or more state variables are never used in the codebase. 
Variable STABLE_BORROWING_MASK and RESERVE_FACTOR_MASK   is never used in smart contarct.

 
# REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS 
Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas.
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L118
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L123
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L128
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L133
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L207
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L225
# ADD UNCHECKED {} FOR SUBTRACTIONS WHERE THE OPERANDS CANNOT UNDERFLOW BECAUSE OF A PREVIOUS REQUIRE() OR IF STATEMENT
This will stop the check for overflow and underflow so it will save gas
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L284
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L332