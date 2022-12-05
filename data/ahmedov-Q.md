# [01] DOUBLE CHECK IN ```removeFeeder``` FOR ```feeder``` EXISTANCE

paraspace-core/contracts/misc/NFTFloorOracle.sol: [169](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L169)
paraspace-core/contracts/misc/NFTFloorOracle.sol: [328](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L328)

## Suggest 

```diff
     function removeFeeder(address _feeder)
         external
-        onlyWhenFeederExisted(_feeder)
     {
         _removeFeeder(_feeder);
     }
```