## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1] | Cache array length outside of loop | 12 |
| [GAS-2] | Use != 0 instead of > 0 for unsigned integer comparison | 5 |
| [GAS-3] | Don't initialize variables with default value | 4 |
| [GAS-4] | Using bools for storage incurs overhead | 2 |
| [GAS-5] | Loop can be more optimizable | 8 |
| [GAS-6] | Split require() statement that use && operator can help in save gas | 2 |
| [GAS-7] | Divide by 2 should be bit-shift. If possible try to use bit-shift in replace of multiplication and division | 2 |
| [GAS-8] | <x> += <y> costs more gas than <x> = <x> + <y> for state variables | 3 |
| [GAS-9] | Arithmetic operations that will not over or under flow should made uncheck   | 2 |


### [GAS-01] Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (12)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

191:    uint256[] memory prices = new uint256[](tokenIds.length);

193:    for (uint256 index = 0; index < tokenIds.length; index++) {
210:    for (uint256 index = 0; index < tokenIds.length; index++) {

```

```solidity
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol

92:   assets.length == sources.length,
196:  uint256[] memory prices = new uint256[](assets.length);

95:   for (uint256 i = 0; i < assets.length; i++) {
197:  for (uint256 i = 0; i < assets.length; i++) {
```

```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

225-228: require(
            _assets.length == _twaps.length,  
            "NFTOracle: Tokens and price length differ"
        );   
229:   for (uint256 i = 0; i < _assets.length; i++) {
291:   for (uint256 i = 0; i < _assets.length; i++) { 
321:   for (uint256 i = 0; i < _feeders.length; i++) {   
413:   for (uint256 i = 0; i < feederSize; i++) {

```


### [GAS-02] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (5)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

51:       require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

54-57:  require(
            orderInfo.consideration.length > 0,  // @audit
            Errors.INVALID_MARKETPLACE_ORDER
        );

84:       require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

87-90:  require(
            orderInfo.consideration.length > 0,  // @audit
            Errors.INVALID_MARKETPLACE_ORDER
        );

```

```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

356:    require(_twap > 0, "NFTOracle: price should be more than 0");

```


### [GAS-03] Don't initialize variables with default value

*Instances (4)*:
```solidity
File:  paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

208:    uint256 sum = 0;

```
```solidity
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol

125:   uint256 price = 0;
```

```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

411:   uint256 validNum = 0;
417:   if (priceInfo.updatedAt > 0) { 

```



### [GAS-04] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the  slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

*Instances (2)*:
```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

38:   bool paused;
45:   bool registered;

```


### [GAS-05] Loop can be more optimizable

. Should not initialize uint with default value i.e uint i=0 **TO** uint i;
. Should use ++i instead i++
. Should uncheck ++i


*Instances (8)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:    for (uint256 index = 0; index < tokenIds.length; index++) {
210:    for (uint256 index = 0; index < tokenIds.length; index++) {

```

```solidity
File:   paraspace-core/contracts/misc/ParaSpaceOracle.sol

95:   for (uint256 i = 0; i < assets.length; i++) {
197:  for (uint256 i = 0; i < assets.length; i++) {
```

```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

229:   for (uint256 i = 0; i < _assets.length; i++) {
291:   for (uint256 i = 0; i < _assets.length; i++) { 
321:   for (uint256 i = 0; i < _feeders.length; i++) {   
413:   for (uint256 i = 0; i < feederSize; i++) {
```

### [GAS-06] Split require() statement that use && operator can help in save gas

*Instances (2)*:
```solidity
File:   paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

41-45:  require(
            // NOT criteria based and must be basic order
            resolvers.length == 0 && isBasicOrder(advancedOrder),  
            Errors.INVALID_MARKETPLACE_ORDER
        );

71-77:  require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&  
                isBasicOrder(advancedOrders[0]) &&  
                isBasicOrder(advancedOrders[1]),
            Errors.INVALID_MARKETPLACE_ORDER
        );

```


### [GAS-07] Divide by 2 should be bit-shift. If possible try to use bit-shift in replace of multiplication and division

*Instances (2)*:
```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

429:   return (true, validPriceList[validNum / 2]);
440:   uint256 pivot = arr[uint256(left + (right - left) / 2)];

```



### [GAS-08] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

*Instances (3)*:
```solidity
File:   paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

149:   token0Amount += positionData.tokensOwed0;  
150:   token1Amount += positionData.tokensOwed1;
211:   sum += getTokenPrice(tokenIds[index]);

```

### [GAS-09] Arithmetic operations that will not over or under flow should made uncheck  

*Instances (2)*:
```solidity
File:   paraspace-core/contracts/misc/NFTFloorOracle.sol

442:   while (arr[uint256(i)] < pivot) i++;   
449:   i++;
```

