#INITIALIZE() FUNCTION CAN BE CALLED BY ANYBODY 
initialize() function can be called anybody when the contract is not initialized.  
that can change some role in smard contract 
there is no 0 address check in the address arguments of the initialize() function, which must be defined.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97 
