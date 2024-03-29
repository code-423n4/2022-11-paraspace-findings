## Open TODOs
Open TODOs can point to architecture or programming issues that still need to be resolved. Consider resolving them before deploying.

Here are the instances entailed:

[File: LooksRareAdapter.sol#L59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59)

```
            makerAsk.price, // TODO: take minPercentageToAsk into accoun
```
[File: UniswapV3OracleWrapper.sol#L238](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238)

```
        // TODO using bit shifting for the 2^96
```
[File: MarketplaceLogic.sol#L442](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442)

```
        // TODO: support PToken
```
## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

Here are the contract instances lacking NatSpec mostly in its entirety:

[File: LooksRareAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol)

[File: SeaportAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

[File: X2Y2Adapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol)

Here are the contract instances partially lacking NatSpec:

[File: ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

[File: UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)

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
## Merging Locally Declared Variable
Separately declaring an uninitialized local variable is unnecessary considering it may directly be assigned its supposed value.

Here is the instance entailed:

[File: NFTFloorOracle.sol#L360-L365](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L360-L365)

```
360:        uint256 priceDeviation;
...
365:        priceDeviation = _twap > _priorTwap
```
The above two code lines may be merged into line 365 by getting `priceDeviation` directly assigned from the ternary block as follows:

```
365:        uint256 priceDeviation = _twap > _priorTwap
```
## revokeRole() for _removeFeeder()
In `NFTFloorOracle.sol`, the modifier, `onlyRole(DEFAULT_ADMIN_ROLE)`, has not been included as a function visibility for `removeFeeder()` because a similar check will be executed when externally calling `revokeRole()` in `_removeFeeder()`.

Consider moving [line 336](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L336), `revokeRole(UPDATER_ROLE, _feeder);`, to the beginning line of the function code block so that it would revert as early as possible whenever `removeFeeder()`, an external function, was invoked by a non-Admin.

## Unneeded State Variable
In `NFTFloorOracle.sol`, a public state array, `assets`, has been declared to house all NFT contracts. Unlike its counterpart, `feeders`, no array trimming is implemented like it is seen in [lines 332 - 333](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L332-L333). Simply deleting it on [line 301](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L301) turns the element to zero address by leaving a hole in the array, which would not be conducive if the ever growing `assets` were to be looked up. But since it is not practically referenced within or without the contract, e.g. in a for loop like `feeders` is, it may be deemed obsolete. If need be, the asset can be more meaningfully retrieved via the associated mappings, `assetPriceMap` and `assetFeederMap`.

In conjunction with this recommendation, the following associated code lines may also be removed from the contract:

```
35:    // index in asset list
36:    uint8 index;

283:        assets.push(_asset);
284:        assetFeederMap[_asset].index = uint8(assets.length - 1);

300:        uint8 assetIndex = assetFeederMap[_asset].index;
301:        delete assets[assetIndex];
```
## Events Associated With Setter Functions
Consider having events associated with setter functions emit both the new and old values instead of just the new value.

Here are the instances entailed:

[File: ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol)

```
109:     function _setFallbackOracle(address fallbackOracle) internal {
```
## Missing Zero Address and Zero Value Checks
Zero address and zero value checks should be implemented at the constructor to avoid human error(s) that could result in non-functional calls associated with them particularly when the incidents involve immutable variables that could lead to contract redeployment at its worst.

In the instances entailed below, the address arrays may have the sanity checks correspondingly carried out in the for loop of `_setAssetsSources()`:

[File: ParaSpaceOracle.sol#L50-L55](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L50-L55)

```
        IPoolAddressesProvider provider,
        address[] memory assets,
        address[] memory sources,
        address fallbackOracle,
        address baseCurrency,
        uint256 baseCurrencyUnit
```
All other instances entailed:

[File: UniswapV3OracleWrapper.sol#L24-L26](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L24-L26)

```
        address _factory,
        address _manager,
        address _addressProvider
```
## Two-step Transfer of Ownership
In `PoolAddressesProvider.sol`, the contract inherits from `Ownable.sol` where `transferOwnership()` is invoked at the constructor allowing the deployer to transfer ownership to the owner specified in the constructor parameter. This function checks the new owner is not the zero address and proceeds to write the new owner's address into the owner's state variable. If the nominated EOA account is not a valid account, it is entirely possible the contract deployer may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier. 

Consider implementing a two step process where the deployer nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to fully succeed.  This will ensure the new owner is going to be fully aware of the ownership assigned/transferred other than having the above mistake avoided.

## Typo Mistakes
[File: AuctionLogic.sol#L34](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34)

```
    @ tsatr
     * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
```
## Lines Too Long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens theses days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible. 

Here are the instances entailed:

[File: LiquidationLogic.sol#L603](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603)

## Use `delete` to Clear Variables
`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at x.

The delete key better conveys the intention and is also more idiomatic. 

Here are the instances entailed:

[File: MarketplaceLogic.sol#L409-L410](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L409-L410)

```
            price = 0;
            downpayment = 0;
```