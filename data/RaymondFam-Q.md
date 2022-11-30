## Open TODOs
Open TODOs can point to architecture or programming issues that still need to be resolved. Consider resolving them before deploying.

Here is the instances entailed:

[File: LooksRareAdapter.sol#L59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59)

```
            makerAsk.price, // TODO: take minPercentageToAsk into accoun
```
## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

Here are the contract instances lacking NatSpec mostly in its entirety:

[File: LooksRareAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol)

[File: SeaportAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

[File: X2Y2Adapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol)

## No Storage Gap for Upgradeable Contracts
Consider adding a storage gap at the end of an upgradeable contract, just in case it would entail some child contracts in the future. This would ensure no shifting down of storage in the inheritance chain. 

Here are the contract instances entailed:

[File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol)

```
uint256[50] private __gap;
```
## Unneeded Conditional Check
The first conditional check in the if block below is always going to return true since `feederIndex` can be any unsigned integer from zero onward.

[File: NFTFloorOracle.sol#L331](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L331)

```
        if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
```
Consider removing it by having the code block refactored as follows:

```
        if (feeders[feederIndex] == _feeder) {
```
## Variable Assignment in Conditional Check
Making a variable assignment in a conditional statement deviates from the standard use and intention of the check and can easily lead to confusion.

Here is the instance entailed:

[File: NFTFloorOracle.sol#L360-L365](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L360-L365)

```
360:        uint256 priceDeviation;
...
365:        priceDeviation = _twap > _priorTwap
```
Consider executing the needed assignment before the conditional statement on line 360 by having the code block refactored as follows:

```
360:        uint256 priceDeviation = _twap;
...
365:        priceDeviation > _priorTwap
```