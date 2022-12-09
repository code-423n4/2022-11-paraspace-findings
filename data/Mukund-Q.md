## UNUSED/EMPTY RECEIVE()/FALLBACK() FUNCTION
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds 
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149

## Some important function should emit who called this functions with `msg.sender` even though its only callable by admin for future logs
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L127-L134
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L151-L166

## EVENTS IS MISSING INDEXED FIELD
Use at least 3 indexed field in events 
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/interfaces/IPoolConfigurator.sol#L19-L24
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/interfaces/IPoolConfigurator.sol#L40-L45
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/interfaces/IPoolConfigurator.sol#L80-L84

## EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/ACLManager.sol#L15-L23
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol#L25-L28
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol#L11-L14

## SOME OF THE FUNCTION IS MISSING ZERO ADDRESS CHECK
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70-L78
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182-L193
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol#L91-L97

## CONSTANT ARE REDEFINED ELSE WHERE
same constant are redefined in multiple contract it should be added in a library and used in different contracts without defining then again and again
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L53
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L56
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L49

