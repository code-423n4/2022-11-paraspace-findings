# [01] Lack of address(0) validation

If a variable gets configured with address zero, failure to immediately reset the value can result in unexpected behavior for the project.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23-L31

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L58-L59

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L148

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L237

# [02] Unbounded loops

While looping large collections, it's possible to run out of gas - causing a DOS condition. The contracts `GenericLogic.sol` and `ApeStakingLogic.sol` contain loops without an upper bound limit.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213

## Recommendation

Consider adding an startIndex and endIndex arguments on `getUserTotalStakingAmount` and `_getUserBalanceForERC721`, to enable a slice functionality where the loops don't need to traverse all the items. Frontends can be adjusted to call these functions in chunks if the total amount of items grows too large.

# [03] Use a more recent version of solidity

All the contracts in scope are using v0.8.10.

Consider using the latest stable version of solidity to ensure the compiler contains the latest security fixes.

# [04] Usage of outdated OpenZeppelin version

The project is using OpenZeppelin fixed version v.4.2.0.

Consider upgrading to the latest stable version v4.8.0.

# [05] Critical changes should use two-step procedure

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

Consider adding a two-steps pattern on critical changes to avoid mistakenly transferring ownership of roles or critial functionalities to the wrong address.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158-L162

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170-L174

# [06] Potantial issue regarding the amount of tokens minted

The current logic on `SupplyLogic` will take `params.amount` into account for minting PTokens. Some tokens change a fee while making transfers (e.g. safemoon) and it's possible for existing tokens to start to charge a fee in the future (e.g. USDT).

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L110-L115

## Recommendation

If possible, consider using the delta value between before and after the transfer to mint the PTokens.

# [07] Lack of payable keyword for address receiving ether

Replace `to.call` with `payable(to).call`.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L145

# [08] Missing storage gap for upgradeable contracts

Consider adding a `__gap[]` storage variable to allow for new storage variables in later versions.

See this [link](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) for a description of this storage variable issue. Please, refer to this [example](https://github.com/code-423n4/2022-08-foundation/blob/main/contracts/mixins/shared/Gap10000.sol) for an implementation reference.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L24

# [09] Avoid leaving upgradeable contract uninitialized

OpenZeppelin [recommends](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#initializing_the_implementation_contract) adding `_disableInitializers();` in the constructor when using upgradeable contracts.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L43-L68

# [10] Use replace and pop instead of the delete keyword to removing an item from an array

Instead of setting an value with zero (using the delete keyword), a better approach for [removing an asset](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L301) from `NFTFloorOracle` can be to replace the target item with the last element and pop the last element, similarly that what is done for [removing a feeder](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L332-L333).

# [11] Avoid stale values in setters

Consider adding a validation to ensure new values are different than the current values in setter functions. Otherwise, when passing the same value, the protocol will unnecessarily emit an event.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L148

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L237

# [12] Remove unnecessary solhint-disable

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L258

# [13] Remove unnecessary getter function

Public constants already contain a getter by default. If there's any special reason for `getRevision`, consider documenting it in the codebase.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L53

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L56-L58

# [14] Lack of spacing in comment

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L155

# [15] Avoid shadowing

Consider renaming the variable `isUsedAsCollateral` from [L242](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L242). It is being shadowed by [MintableIncetivizedERC721.isUsedAsCollateral](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L509).

# [16] Declaring `return named variables` and not using the variables in the function scope

Consider removing the `named variables` when they're not used inside the function.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L236-L248

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L252-L259

# [17] Missing NATSPEC

Consider adding NATSPEC on all external/public functions to improve documentation.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L335

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L135

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L187

# [18] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

The instance bellow shows an external function bellow an internal function.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L263-L267

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L271-L276

# [19] Avoid using the optimizer if possible, due to it's potential security bugs which can affect the contracts in scope

Optimizations are being actively developed and are not considered safe. Check the following [audit](https://github.com/trailofbits/publications/blob/master/reviews/SeaportProtocol.pdf) recommending agaist deployments with the optimizer enabled (item 3 in the audit document).

If possible, consider measuring and documenting the tradeoffs when enabling the optimizer.

# [20] Public functions not called by the contract should be declared external

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L261-L263

# [21] Open TODOs

TODOs should be resolved before deployment.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442

# [22] 2**<n> can be re-written as type(uint<n>).max + 1

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L247

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L259

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L271

# [23] Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

Scientific notation can be used for better code readability.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245
