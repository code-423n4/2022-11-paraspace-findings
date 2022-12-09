#INITIALIZE() FUNCTION CAN BE CALLED BY ANYBODY 
initialize() function can be called anybody when the contract is not initialized.  
that can change some role in smard contract 
there is no 0 address check in the address arguments of the initialize() function, which must be defined.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97 
# UNUSED RETURN VALUE 
The return value of an external call is not stored in a local or state variable that should be checking or using the return values of all external function calls

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L91
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L182