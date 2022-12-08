### [Gas]

1. **USING `ABI.ENCODE()` IS LESS EFFICIENT THAN `ABI.ENCODEPACKED()`**
    
    Instance: [ValidationLogic#L1136](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1136)
    
2. **`X = X + Y` IS MORE EFFICIENT THAN `X += Y`, LIKEWISE FOR `X = X-Y`**
    
    *29 instances*, here are some examples: 
    
    - [UniswapV3OracleWrapper#L149](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L149)
    - [UniswapV3OracleWrapper#L150](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L150)
    - [UniswapV3OracleWrapper#L211](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L211)
