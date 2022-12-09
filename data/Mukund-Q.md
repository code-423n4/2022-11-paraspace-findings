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

