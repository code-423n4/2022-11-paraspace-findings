## `Cross-Chain Replay` attack

Storing the `block.chainid` is not safe. 

```
                    block.chainid,
```
* File: ValidationLogic.sol [(line 1140)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1140)



## Use of `block.number`

```
            (block.number - updatedAt) <= config.expirationPeriod,
```
* File: contracts/misc/NFTFloorOracle.sol [(line 244)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L244)

```
        assetPriceMapEntry.updatedAt = block.number;
```
* File: contracts/misc/NFTFloorOracle.sol [(line 379)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L379)


```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L394
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L403



## Use of `block.timestamp`

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
            startTime: block.timestamp
```
* File: libraries/MintableERC721Logic.sol [(line 382)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L382)

```
        assetPriceMapEntry.updatedTimestamp = block.timestamp;
```
* File: contracts/misc/NFTFloorOracle.sol [(line 380)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L380)
```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L69
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L285
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L778


## open `TODO` comments

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.

```
        // TODO: support PToken
```
* File: /logic/MarketplaceLogic.sol [(line 442)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442)

```
        // TODO using bit shifting for the 2^96
```
* File: misc/UniswapV3OracleWrapper.sol [(line 238)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238)
```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59


## Use `scientific notation` (e.g. `1e18`) rather than `exponentiation` (e.g. `10**18`)

```
                    ((oracleData.token0Price * (10**18)) /
```
>File: misc/UniswapV3OracleWrapper.sol [(line 245)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245)


## `TYPOS`

```
	//@audit `tsatr`
     * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
```
>File: libraries/logic/AuctionLogic.sol [(line 34)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34)
```
	//@audit `aggeregate`
	/// aggeregate prices which are not expired from different feeders, if number of valid/unexpired prices
	/// not enough, we do not aggeregate and just use previous price
```
>File: contracts/misc/NFTFloorOracle.sol [(line 53-54)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L53-L54)
```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412
//@audit `aggeregate`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L59
//@audit `undate`


## Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions

While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

```
	contract WPunkGateway is
    ReentrancyGuard,
    IWPunkGateway,
    IERC721Receiver,
    OwnableUpgradeable

```
>File: contracts/ui/WPunkGateway.sol [(line 19-23)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L23)

```
	contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {


```
>File: contracts/misc/NFTFloorOracle.sol [(line 55)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L55)
```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L40-L44
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L29-L36
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L45-L49
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L29
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L23
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L20
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L23-L27
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L46-L50


## `INITIALIZE` functions can be front-run

This function can be called by everyone. So an attacker may watch the mempool for this  function and may frontrun it by supplying more gas.This may result in unintended behaviour.


```
    function initialize(
        address _admin,
        address[] memory _feeders,
        address[] memory _assets
    ) public initializer {

```
>File: contracts/misc/NFTFloorOracle.sol [(line 97-101)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97-L101)
```
    function initialize(
        IPool initializingPool,
        address underlyingAsset,
        IRewardController incentivesController,
        string calldata nTokenName,
        string calldata nTokenSymbol,
        bytes calldata params
    ) public virtual override initializer {

```
>File: contracts/protocol/tokenization/NTokenApeStaking.sol [(line 36-43)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L36-L43)
 
```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L52-L59


##  Missing `initializer` modifier on constructor

OpenZeppelin recommends that the initializer modifier be applied to constructors in order to avoid potential griefs, social engineering, or exploits. Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.

```
	contract WPunkGateway is
    ReentrancyGuard,
    IWPunkGateway,
    IERC721Receiver,
    OwnableUpgradeable

```
>File: contracts/ui/WPunkGateway.sol [(line 19-23)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L23)

```
	contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {


```
>File: contracts/misc/NFTFloorOracle.sol [(line 55)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L55)
```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L40-L44
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L29-L36
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L45-L49
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L29
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L23
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L20
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L23-L27
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L46-L50


## `public` functions not called by the contract should be declared `external` instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public.

```
    function initialize(
        address _admin,
        address[] memory _feeders,
        address[] memory _assets
    ) public initializer {
```
* File: contracts/misc/NFTFloorOracle.sol [(line 97-101)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97-L101)


```
    function collateralizedBalanceOf(address account)
        public
```
* File: protocol/tokenization/base/MintableIncentivizedERC721.sol [(line 447-448)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L447-L448)


## Missing `zero address` check in `constructor`

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

```
        punk = _punk;
        wpunk = _wpunk;
        pool = _pool;
```

>File: contracts/ui/WPunkGateway.sol [(line 48-50)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L48-L50)
```
        transferOwnership(owner);

```

>File: protocol/configuration/PoolAddressesProvider.sol [(line 47)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L47)

```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L17
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L61
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L33
*https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L28-L30

## Set `garbage` value in `mapping` for deleting that

If there is a mapping data structure present inside struct, then deleting the struct doesn't delete the mapping. Instead one should use lock to lock that data structure from further use.


```
        delete reservesData[asset];
```
>File:contracts/protocol/libraries/logic/PoolLogic.sol [(line 152)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L152)
```
        delete assetPriceMap[_asset];
        delete assetFeederMap[_asset];
```
>File:contracts/misc/NFTFloorOracle.sol [(line 302-303)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L302-L303)


## `NatSpec` is incomplete

```
    /// @audit Missing: `@return`
        /**
     * @notice Validates the health factor of a user.
     * @param reservesData The state of all the reserves
     * @param reservesList The addresses of all the active reserves
     * @param userConfig The state of the user for the specific reserve
     * @param user The user to validate health factor of
     * @param reservesCount The number of available reserves
     * @param oracle The price oracle
     */
    function validateHealthFactor(

```
* File: libraries/logic/ValidationLogic.sol [(line 693-702)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L693-L702)

```
    /// @audit Missing: `@param`
    /**
     * @notice Validates a transfer action.
     * @param reserve The reserve object
     */
    function validateTransferERC721(

```
* File: libraries/logic/ValidationLogic.sol [(line 940-944)](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L940-L944)

```
      Other instances of this issue are :
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L105-L114
    /// @audit Missing: `@return`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L155-L165
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L356-L362
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L504-L512
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L183-L186
    /// @audit Missing: `param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L200-L203
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L174-L175
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L182-L183
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L196-L204
    /// @audit Missing: `@return`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L225-L231
    /// @audit Missing: `@return`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L75-L78
    /// @audit Missing: `@param`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L162-L165
    /// @audit Missing: `@param`& `return`
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L162-L165
    /// @audit Missing: `@param` & `return`