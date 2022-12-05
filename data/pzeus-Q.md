## Issue
In general, missing a lot of dev and not only comments that will suit greatly to all auditors, future audits or even new developers that will be potentially onboarded
Although, this does not have any impact on the protocol itself, I think this will smoothen up a lot of things in the furure
## Examples
Some examples are `SeaportAdapter.sol` and `X2Y2Adapter.sol`
Also, in a lot of cases, the function behaviour and params explanation are placed inside the interface contracts of the contracts that are implementing those functionalities. This makes the whole process uncomfortable and annoying

There are also examples of no comments on complex math calculations
[DefaultReserveAuctionStrategy.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L90)
## Recommendation
Add missing useful comments where needed

## Issue
Unnecessary [`onlyWhenFeederExisted(_feeder)`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L169
) modifier used in the internal `_removeFeeder()` function. The same modifier is used inside the external `removeFeeder()` which calls the internal `_removeFeeder()` function
## Example
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L169
## Recommendation
Remove `onlyWhenFeederExisted(_feeder)` modifier from the internal `_removeFeeder()` function since it is only called by the external `removeFeeder()`

## Issue
Unnecessary [`onlyWhenAssetExisted(_asset)`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L298) modifier used in the internal `_removeAsset()` function. The same modifier is used inside the external `_removeAsset()` which calls the internal `_removeAsset()` function
## Example
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L298
## Recommendation
Remove `onlyWhenAssetExisted(_asset)` modifier from the internal `_removeAsset()` function since it is only called by the external `removeAsset()`

## Issue
There are getters which can be reused inside a contract
## Examples
`ParaSpaceOracle.sol` [1](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L214) & [2](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L204)
## Recommendation
Use `getSourceOfAsset()` on every `assetsSources[asset]` occurence inside `ParaSpaceOracle.sol` contract and `getFallbackOracle()` on every `address(_fallbackOracle)` occurrence

## Issue
There are a lot of places where an event should be emitted in order to point that some action was executed. Since using the events we can consider whether or not something should happen on the front-end or search for an information by event indexed values, there should be emitted events on important protocol actions
## Examples
`PoolParameters.sol` [1](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L110), [2](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139), [3](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166), [4](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181), [5](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L196) & [6](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206)
## Recommendation
Declare events and use them at the proper places

## Issue
There are missing access modifiers on `public/exernal` methods. In these cases, it can result in exposing customers data or some important protocol variables
## Examples
`PoolParameters.sol` [1](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L226) & [2](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L251)
## Recommendation
Use access modifiers in order to restrict who can call the functions