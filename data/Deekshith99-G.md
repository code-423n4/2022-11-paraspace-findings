## [G -1] fixed bytes[x] can save some gas than bytes32
       for 4 to 8 characters of word also we are using bytes32 
       depending up on the word size we can choose the bytes[x] 
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L31-L38 

## [G - 2] Its recommended to update the solidity version to the latest version (secured one)
         for  more optimization of the contracts