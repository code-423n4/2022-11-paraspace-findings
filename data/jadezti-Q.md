
|        | Issue                                                              | Instances |
| ------ |:------------------------------------------------------------------ |:---------:|
| [NC-1] | Require parameter verification in `NFTFloorOracle.setConfig` |     1     |


## [NC-1] Require parameter verification in `NFTFloorOracle.setConfig`

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L179

There is no verification when setting configurations in function `NFTFloorOracle.setConfig()`.
If incorrect parameters are set, the system may run into problems. For example, if `maxPriceDeviation` is set to less than `100`, NFT prices won't be updated any more since `setPrice()` function will always fail.
It is recommended to set appropriate data verifications when updating important configuration parameters.
