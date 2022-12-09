## Summary	
### Low risk findings

| Issue | Description | Instances |
| -- | ----------- | -------- |
|1| Unused/empty `receive()` function|1|
|2| Open TODO re incentives|1|
|3| Outdated Open Zeppelin versions|1|
|| Total|3|

### Non-critical findings

| Issue | Description | Instances |
| -- | ----------- | -------- |
|1| Constant value definitions including call to `keccak256` should use `immutable`|1|
|2| `nonReentrant` modifier should appear before all other modifiers|25|
|3| Missing NatSpec for some `constructors`|7|
|4| Missing NatSpec for `event` |1|
|5| Partially missing NatSpec for some functions |24|
|6| Completely missing NatSpec for some functions |66|
|7| Long single-line comments  |17|
|8| Typos |9|
|9| Unclear comment |1|
|10|Inconsistent comment // spacing |multiple|
|11| Solidity version should be upgraded |1|
|12| TODOs and other open items |4|
|| Total|> 156|

### Low risk findings

| No. | Explanation + instances | 
|--| ----------- | 
|1| **Unused/empty `receive()` function`**|
|  | Leaving the `receive()` below without a `revert` could result in a loss of funds for a user who sends Ether to the contract (having no way to get anything back)|

[NTokenUniswapV3.sol: L149](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149)
```solidity
    receive() external payable {}
```
___
| No. | Explanation + instances | 
|--| ------------------------------------------------------------------------------------------- | 
|2| **Open TODO re incentives**|
||Open items re incentives such as the TODO below should be addressed before deployment |

[LooksRareAdapter.sol: L59](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59)
```solidity
            makerAsk.price, // TODO: take minPercentageToAsk into account
```
___
| No. | Explanation + instances | 
|--| ----------- | 
|3| **Outdated Open Zeppelin versions**|
||Outdated Open Zeppelin version are used (v4.7.0 and earlier)|
||The versions used have known vulnerabilities, including three with high severity. See [Security Advisories](https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories).||
___
___

### Non-critical findings

| No. | Description |
|--| ----------- | 
|1|Constant value definitions including call to `keccak256` should use `immutable`|
|  | Constant value definitions such as the call to `keccak256` below should use `immutable` instead of `constant`|

[NFTFloorOracle.sol: L70](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L70)
```solidity
    bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
```
___
| No. | Description |
|--| ----------- | 
|2|`nonReentrant` modifier should appear before all other modifiers|
|  |Adopting this rule protects against the possibilty of reentrancy in other modifiers|

[MintableIncentivizedERC721.sol: L458-462](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458-L462)
```solidity
    function setIsUsedAsCollateral(
        uint256 tokenId,
        bool useAsCollateral,
        address sender
    ) external virtual override onlyPool nonReentrant returns (bool) {
```
The `nonReentrant` modifier is similarly mispositioned in the following functions:

[MintableIncentivizedERC721.sol: L474-484](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L474-L484)

[MintableIncentivizedERC721.sol: L529-535](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L529-L535)

[MintableIncentivizedERC721.sol: L540-546](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L540-L546)

[NToken.sol: L79-82](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L79-L82)

[NToken.sol: L87-91](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L87-L91)

[NToken.sol: L119-123](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L119-L123)

[NToken.sol: L199-205](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L199-L205)

[NToken.sol: L214-220](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L214-L220)

[NTokenApeStaking.sol: L102-106](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102-L106)

[NTokenApeStaking.sol: L159-163](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L159-L163)

[NTokenBAYC.sol: L26-30](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L26-L30)

[NTokenBAYC.sol: L39-43](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L39-L43)

[NTokenBAYC.sol: L52-55](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L52-L55)

[NTokenBAYC.sol: L66-70](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L66-L70)

[NTokenBAYC.sol: L82-85](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L82-L85)

[NTokenBAYC.sol: L97-100](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97-L100)

[NTokenMAYC.sol: L26-30](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26-L30)

[NTokenMAYC.sol: L39-43](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L39-L43)

[NTokenMAYC.sol: L52-55](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L52-L55)

[NTokenMAYC.sol: L66-70](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L66-L70)

[NTokenMAYC.sol: L82-85](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L82-L85)

[NTokenMAYC.sol: L97-100](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-L100)

[NTokenMoonBirds.sol: L40-44](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40-L44)

[NTokenUniswapV3.sol: L123-130](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123-L130)
___

| No. | Description |
|--| ----------- | 
|3|Missing NatSpec for some `constructors`|
|  |NatSpec (consisting of an `@notice` or an `@dev` statement, as well as `@param` statements for any variables) is provided for most of the `constructors` in the contest. Below are instances of `constructors` with partially or completely missing NatSpec, which should be added to provide information and for consistency:|

[UniswapV3OracleWrapper.sol: L23-27](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23-L27)
```solidity
    constructor(
        address _factory,
        address _manager,
        address _addressProvider
    ) {
```
Missing: All NatSpec
___
[DefaultReserveAuctionStrategy.sol: L50-57](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L50-L57)
```solidity
    constructor(
        uint256 maxPriceMultiplier,
        uint256 minExpPriceMultiplier,
        uint256 minPriceMultiplier,
        uint256 stepLinear,
        uint256 stepExp,
        uint256 tickLength
    ) {
```
Missing: All NatSpec
___
[MintableIncentivizedERC721.sol: L77-88](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L77-L88)
```solidity
    /**
     * @dev Constructor.
     * @param pool The reference to the main Pool contract
     * @param name_ The name of the token
     * @param symbol_ The symbol of the token
     */
    constructor(
        IPool pool,
        string memory name_,
        string memory symbol_,
        bool atomic_pricing
    ) {
```
Missing: `@param atomic_pricing`
___
[NToken.sol: L39-49](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L39-L49)
```solidity
    /**
     * @dev Constructor.
     * @param pool The address of the Pool contract
     */
    constructor(IPool pool, bool atomic_pricing)
        MintableIncentivizedERC721(
            pool,
            "NTOKEN_IMPL",
            "NTOKEN_IMPL",
            atomic_pricing
        )
```
Missing: `@param atomic_pricing`
___
[NTokenApeStaking.sol: L28-34](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L28-L34)
```solidity
    /**
     * @dev Constructor.
     * @param pool The address of the Pool contract
     */
    constructor(IPool pool, address apeCoinStaking) NToken(pool, false) {
        _apeCoinStaking = ApeCoinStaking(apeCoinStaking);
    }
```
Missing: `@param apeCoinStaking`
___
[NTokenBAYC.sol: L10-17](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L10-L17)
```solidity
/**
 * @title BAYC NToken
 *
 * @notice Implementation of the NToken for the ParaSpace protocol
 */
contract NTokenBAYC is NTokenApeStaking {
    constructor(IPool pool, address apeCoinStaking)
        NTokenApeStaking(pool, apeCoinStaking)
```
Missing: `@param` statements
 ___ 
[NTokenMAYC.sol: L10-17](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L10-L17)
```solidity
/**
 * @title MAYC NToken
 *
 * @notice Implementation of the NToken for the ParaSpace protocol
 */
contract NTokenMAYC is NTokenApeStaking {
    constructor(IPool pool, address apeCoinStaking)
        NTokenApeStaking(pool, apeCoinStaking)
```
Missing: `@param` statements
 ___ 
 ___ 

| No. | Description |
|--| ----------- | 
|4|Missing NatSpec for `event`|
|  | Explanations (often consisting of an `@notice` or an `@dev` statement) are provided for almost all of the `events` in the contest. Below are two instances of `events` without such NatSpec:|

[PoolApeStaking.sol: L37-45](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L37-L45)
```solidity
    event ReserveUsedAsCollateralEnabled(
        address indexed reserve,
        address indexed user
    );

    event ReserveUsedAsCollateralDisabled(
        address indexed reserve,
        address indexed user
    );
```
___
___

| No. | Description |
|--| ----------- | 
|5|Partially missing NatSpec for some functions|
|  | NatSpec is partially missing for the following `external` or `public` `functions`. Note that, while it may be informative, NatSpec is not required for `internal` or `private` `functions`. I'm also ignoring `functions` preceded by references such as `@inheritdoc IPriceOracleGetter` |

[NFTFloorOracle.sol: L174-177](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L174-L177)
```solidity
    /// @notice Allows owner to update oracle configs
    function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
```
Missing: `@param expirationPeriod` and `@param maxPriceDeviation`
___
[NFTFloorOracle.sol: L182-185](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L182-L185)
```solidity
    /// @notice Allows owner to pause asset
    function setPause(address _asset, bool _flag)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
```
Missing: `@param _asset` and `@param _flag` 
___
[NFTFloorOracle.sol: L218-224](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L218-L224)
```solidity
    /// @notice Allows owner to set new price on PriceInformation and updates the
    /// internal Median cumulativePrice.
    /// @param _assets The nft contract to set a floor price for
    function setMultiplePrices(
        address[] calldata _assets,
        uint256[] calldata _twaps
    ) external onlyRole(UPDATER_ROLE) {
```
Missing: `@param _twaps`
___
[UniswapV3OracleWrapper.sol: L48-54](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L48-L54)
```solidity
    /**
     * @notice get onchain position data from uniswap for the specified tokenId.
     */
    function getOnchainPositionData(uint256 tokenId)
        public
        view
        returns (UinswapV3PositionData memory)
```
Missing: `@param tokenId` and `@return` 
___
[UniswapV3OracleWrapper.sol: L93-99](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L93-L99)
```solidity
    /**
     * @notice get onchain liquidity amount for the specified tokenId.
     */
    function getLiquidityAmount(uint256 tokenId)
        external
        view
        returns (uint256 token0Amount, uint256 token1Amount)
```
Missing: `@param tokenId` and two `@return` statements 
___
[UniswapV3OracleWrapper.sol: L109-115](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L109-L115)
```solidity
    /**
     * @notice calculate liquidity amount for the position data.
     * @param positionData The specified position data
     */
    function getLiquidityAmountFromPositionData(
        UinswapV3PositionData memory positionData
    ) public pure returns (uint256 token0Amount, uint256 token1Amount) {
```
Missing: Two `@return` statements
___
[UniswapV3OracleWrapper.sol: L124-130](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L124-L130)
```solidity
    /**
     * @notice get liquidity provider fee amount for the specified tokenId.
     */
    function getLpFeeAmount(uint256 tokenId)
        external
        view
        returns (uint256 token0Amount, uint256 token1Amount)
```
Missing: `@param tokenId` and two `@return` statements 
___
[UniswapV3OracleWrapper.sol: L140-146](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L140-L146)
```solidity
    /**
     * @notice calculate liquidity provider fee amount for the position data.
     * @param positionData The specified position data
     */
    function getLpFeeAmountFromPositionData(
        UinswapV3PositionData memory positionData
    ) public view returns (uint256 token0Amount, uint256 token1Amount) {
```
Missing: Two `@return` statements
___
[UniswapV3OracleWrapper.sol: L153-156](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L153-L156)
```solidity
    /**
     * @notice Returns the price for the specified tokenId.
     */
    function getTokenPrice(uint256 tokenId) public view returns (uint256) {
```
Missing: `@param tokenId` and `@return`
___
[UniswapV3OracleWrapper.sol: L183-189](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L183-L189)
```solidity
    /**
     * @notice Returns the price for the specified tokenId array.
     */
    function getTokensPrices(uint256[] calldata tokenIds)
        external
        view
        returns (uint256[] memory)
```
Missing: `@param tokenIds` and `@return`
___
[UniswapV3OracleWrapper.sol: L200-206](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L200-L206)
```solidity
    /**
     * @notice Returns the total price for the specified tokenId array.
     */
    function getTokensPricesSum(uint256[] calldata tokenIds)
        external
        view
        returns (uint256)
```
Missing: `@param tokenIds` and `@return`
___
[PoolLogic.sol: L155-180](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L155-L180)
```solidity
    /**
     * @notice Returns the user account data across all the reserves
     * @return totalCollateralBase The total collateral of the user in the base currency used by the price feed
     * @return totalDebtBase The total debt of the user in the base currency used by the price feed
     * @return availableBorrowsBase The borrowing power left of the user in the base currency used by the price feed
     * @return currentLiquidationThreshold The liquidation threshold of the user
     * @return ltv The loan to value of The user
     * @return healthFactor The current health factor of the user
     * @return erc721HealthFactor The current erc721 health factor of the user
     **/
    function executeGetUserAccountData(
        address user,
        DataTypes.PoolStorage storage ps,
        address oracle
    )
        external
        view
        returns (
            uint256 totalCollateralBase,
            uint256 totalDebtBase,
            uint256 availableBorrowsBase,
            uint256 currentLiquidationThreshold,
            uint256 ltv,
            uint256 healthFactor,
            uint256 erc721HealthFactor
        )
```
Missing: `@param user`, `@param ps` and `@param oracle`
___
[NTokenApeStaking.sol: L61-65](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L61-L65)
```solidity
    /**
     * @notice Returns the address of BAKC contract address.
     **/
    function getBAKC() public view returns (IERC721) {
        return _apeCoinStaking.nftContracts(ApeStakingLogic.BAKC_POOL_ID);
```
Missing: `@return`
___
[NTokenApeStaking.sol: L68-72](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L68-L72)
```solidity
    /**
     * @notice Returns the address of ApeCoinStaking contract address.
     **/
    function getApeStaking() external view returns (ApeCoinStaking) {
        return _apeCoinStaking;
```
Missing: `@return`
___
[NTokenApeStaking.sol: L99-106](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L99-L106)
```solidity
    /**
     * @notice Overrides the burn from NToken to withdraw all staked and pending rewards before burning the NToken on liquidation/withdraw
     */
    function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```
Missing: `@param from`, `@param receiverOfUnderlying`, `@param tokenIds` and two `@return` statements
___
[NTokenApeStaking.sol: L178-185](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L178-L185)
```solidity
    /**
     * @notice get user total ape staking position
     * @param user user address
     */
    function getUserApeStakingAmount(address user)
        external
        view
        returns (uint256)
```
Missing: `@return`
___
[NTokenMoonBirds.sol: L92-96](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L92-L96)
```solidity
    /**
        @dev an additional function that is custom to MoonBirds reserve.
        This function allows NToken holders to toggle on/off the nesting the status for the underlying tokens
    */
    function toggleNesting(uint256[] calldata tokenIds) external {
```
Missing: `@param tokenIds`
___
[NTokenMoonBirds.sol: L107-118](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L107-L118)
```solidity
    /**
        @dev an additional function that is custom to MoonBirds reserve.
        This function allows NToken holders to get nesting the state for the underlying tokens
    */
    function nestingPeriod(uint256 tokenId)
        external
        view
        returns (
            bool nesting,
            uint256 current,
            uint256 total
        )
```
Missing: `@param tokenId` and three `@return` statements
___
[NTokenMoonBirds.sol: L123-128](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L123-L128)
```solidity
    /**
        @dev an additional function that is custom to MoonBirds reserve.
        This function check if nesting is open for the underlying tokens
    */
    function nestingOpen() external returns (bool) {
        return IMoonBird(_underlyingAsset).nestingOpen();
```
Missing: `@return`
___
[ApeStakingLogic.sol: L196-210](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L196-L210)
```solidity
    /**
     * @notice get user total ape staking position
     * @param userState The user state of nToken
     * @param ownedTokens The ownership mapping state of nNtoken
     * @param user User address
     * @param poolId identify whether BAYC pool or MAYC pool
     * @param _apeCoinStaking ApeCoinStaking contract address
     */
    function getUserTotalStakingAmount(
        mapping(address => UserState) storage userState,
        mapping(address => mapping(uint256 => uint256)) storage ownedTokens,
        address user,
        uint256 poolId,
        ApeCoinStaking _apeCoinStaking
    ) external view returns (uint256) {
```
Missing: `@return`
___
[ApeStakingLogic.sol: L225-235](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L225-L235)
```solidity
    /**
     * @notice get ape staking position for a tokenId
     * @param poolId identify whether BAYC pool or MAYC pool
     * @param _apeCoinStaking ApeCoinStaking contract address
     * @param tokenId specified the tokenId for the position
     */
    function getTokenIdStakingAmount(
        uint256 poolId,
        ApeCoinStaking _apeCoinStaking,
        uint256 tokenId
    ) public view returns (uint256) {
```
Missing: `@return`
___
[WPunkGateway.sol: L119-135](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L119-L135)
```solidity
    /**
     * @notice Implements the acceptBidWithCredit feature. AcceptBidWithCredit allows users to
     * accept a leveraged bid on ParaSpace NFT marketplace. Users can submit leveraged bid and pay
     * at most (1 - LTV) * $NFT
     * @dev The nft receiver just needs to do the downpayment
     * @param marketplaceId The marketplace identifier
     * @param payload The encoded parameters to be passed to marketplace contract (selector eliminated)
     * @param credit The credit that user would like to use for this purchase
     * @param referralCode The referral code used
     */
    function acceptBidWithCredit(
        bytes32 marketplaceId,
        bytes calldata payload,
        DataTypes.Credit calldata credit,
        uint256[] calldata punkIndexes,
        uint16 referralCode
    ) external nonReentrant {
```
Missing: `@param punkIndexes`
___
[WPunkGateway.sol: L157-173](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L157-L173)
```solidity
    /**
     * @notice Implements the batchAcceptBidWithCredit feature. AcceptBidWithCredit allows users to
     * accept a leveraged bid on ParaSpace NFT marketplace. Users can submit leveraged bid and pay
     * at most (1 - LTV) * $NFT
     * @dev The nft receiver just needs to do the downpayment
     * @param marketplaceIds The marketplace identifiers
     * @param payloads The encoded parameters to be passed to marketplace contract (selector eliminated)
     * @param credits The credits that the makers have approved to use for this purchase
     * @param referralCode The referral code used
     */
    function batchAcceptBidWithCredit(
        bytes32[] calldata marketplaceIds,
        bytes[] calldata payloads,
        DataTypes.Credit[] calldata credits,
        uint256[] calldata punkIndexes,
        uint16 referralCode
    ) external nonReentrant {
```
Missing: `@param punkIndexes`
___
[WPunkGateway.sol: L225-228](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L225-L228)
```solidity
    /**
     * @dev Get WPunk address used by WPunkGateway
     */
    function getWPunkAddress() external view returns (address) {
```
Missing: `@return`
___
___

| No. | Description |
|--| ----------- | 
|6|Completely missing NatSpec for some `functions`|
|  | NatSpec is completely or almost completely missing for the following `external` or `public` `functions`:|

_Example:_

[LooksRareAdapter.sol: L67-69](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L67-L69)
```solidity
    function getBidOrderInfo(
        bytes memory /*params*/
    ) external pure override returns (DataTypes.OrderInfo memory) {
```

Similarly for the following `functions`:

**LooksRareAdapter.sol**

[L25-29](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L25-L29), [L73-77](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L73-L77), [L96-100](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L96-L100)

**SeaportAdapter.sol**

[L25-29](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L25-L29), [L60-64](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L60-L64), [L93-97](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L93-L97), [L109-112](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L109-L112)

**X2Y2Adapter.sol**

[L31-35](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L31-L35), [L79-83](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L79-L83), [L88-92](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L88-L92), [L104-108](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L104-L108)

**NFTFloorOracle.sol**

[L261-262](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L261-L262)

**ParaSpaceOracle.sol**

[L138-142](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L138-L142), [L155-159](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L155-L159), [L172-176](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L172-L176)

**UniswapV3OracleWrapper.sol**

[L217-218](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L217-L218)

**MarketplaceLogic.sol**

[L63-70](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L63-L70), [L160-167](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L160-L167), [L229-237](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L229-L237), [L267-275](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L267-L275)

**PoolLogic.sol**

[L214-218](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L214-L218)

**SupplyLogic.sol**

[L335-340](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L335-L340), [L396-401](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L396-L401)

**DefaultReserveAuctionStrategy.sol**

[L66-67](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L66-L67), [L70-71](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L70-L71), [L74-75](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L74-L75), [L78-79](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L78-L79)

[L82-83](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L82-L83), [L86-87](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L86-L87), [L90-93](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L90-L93)

**PoolCore.sol**

[L237-244](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L237-L244), [L397-401](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L397-L401), [L792-798](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L792-L798)

**PoolParameters.sol**

[L251-256](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L251-L256)

**MintableIncentivizedERC721.sol**

[L96-97](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L96-L97), [L100-101](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L100-L101), [L104-109](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L104-L109)

**NToken.sol**

[L52-59](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L52-L59), [L127-131](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L127-L131), [L136-140](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L136-L140), [L151-157](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L151-L157)

[L168-171](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L168-L171), [L271-276](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L271-L276), [L280-286](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L280-L286), [L295-301](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L295-L301)

[L323-324](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L323-L324), [L327-332](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L327-L332)

**NTokenApeStaking.sol**

[L36-43](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L36-L43), [L136](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L136)

**NTokenBAYC.sol**

[L113](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L113)

**NTokenMAYC.sol**
[L113](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L113)

**NTokenMoonBirds.sol**

[L36](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L36), [L40-44](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40-L44), [L63-68](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L63-L68)

**NTokenUniswapV3.sol**

[L37](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L37)

**MintableERC721Logic.sol**

[L80-87](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L80-L87), [L135-142](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L135-L142), [L187-197](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L187-L197), [L260-270](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L260-L270), [L340-344](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L340-L344)

[L357-362](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L357-L362), [L368-372](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L368-L372), [L386-390](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L386-L390), [L424-428](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L424-L428)

**WPunkGateway.sol**

[L57](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L57), [L232-237](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L232-L237)
___
___

| No. | Description |
|--| ----------- | 
|7|Long single-line comments|
|  | In theory, comments over 80 characters should wrap using multi-line comment syntax. Even if longer comments (up to 120 characters) are becoming |
| |acceptable and a scroll bar is provided, very long comments can interfere with readability. Most of the long comments in the contest do wrap.|
| |However, below are instances of extra-long comments (over 120 characters) whose readability could be improved|
| |by wrapping, as shown. Note that a small indentation is used in each continuation line (which flags for the reader|
| |that the comment is ongoing), along with a period at the close (to signal the end of the comment). In addition,|
| |when an extra-long line includes both code and comments, it makes sense to put the comment in the line above.|

[LiquidationLogic.sol: L603](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603)
```solidity
     * @return The actual debt that is getting liquidated. If liquidation amount passed in by the liquidator is greater then the total user debt, then use the user total debt as the actual debt getting liquidated. If the user total debt is greater than the liquidation amount getting passed in by the liquidator, then use the liquidation amount the user is passing in.
```
Suggestion:
```solidity
     * @return The actual debt that is getting liquidated. If liquidation amount passed in by the 
     *   liquidator is greater than the total user debt, then use the user total debt as the actual
     *   debt getting liquidated. If the user total debt is greater than the liquidation amount getting
     *   passed in by the liquidator, then use the liquidation amount the user is passing in.
```
___
[ValidationLogic.sol: L1137](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1137)
```solidity
                    0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
```
Suggestion:
```solidity
                    // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
                    0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f
```
___
[AuctionLogic.sol: L94](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L94)
```solidity
     * @notice Function to end auction on an ERC721 of a position if its ERC721 Health Factor increases back to above `AUCTION_RECOVERY_HEALTH_FACTOR`.
```
Suggestion:
```solidity
     * @notice Function to end auction on an ERC721 of a position if its ERC721
     *   Health Factor increases back to above `AUCTION_RECOVERY_HEALTH_FACTOR`.
```
___
[NTokenApeStaking.sol: L100](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L100)
```solidity
     * @notice Overrides the burn from NToken to withdraw all staked and pending rewards before burning the NToken on liquidation/withdraw
```
Suggestion:
```solidity
     * @notice Overrides the burn from NToken to withdraw all staked and 
     *   pending rewards before burning the NToken on liquidation/withdraw.
```
___
[MintableERC721Logic.sol: L530](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L530)
```solidity
            erc721Data.ownedTokens[from][tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
```
Suggestion:
```solidity
            // Move the last token to the slot of the to-delete token
            erc721Data.ownedTokens[from][tokenIndex] = lastTokenId;
```
___
Similarly for the following extra-long (over 120 characters) comments:

[PoolAddressesProvider.sol: L259](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L259)

[PoolAddressesProvider.sol: L338](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L338)

[LiquidationLogic.sol: L655](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L655)

[WPunkGateway.sol: L71](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L71)

[BorrowLogic.sol: L157](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L157)

[NTokenBAYC.sol: L48](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L48)

[NTokenBAYC.sol: L93](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L93)

[NTokenBAYC.sol: L95](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L95)

[NTokenMAYC.sol: L48](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L48)

[NTokenMAYC.sol: L93](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L93)

[NTokenMAYC.sol: L95](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L95)

[SupplyLogic.sol: L260](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L260)
___
___

| No. | Description |
|--| ----------- | 
|8|Typos|

[ApeStakingLogic.sol: L59](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L59)
```solidity
     * @notice undate incentive percentage for unstakePositionAndRepay
```
Change `undate` to `update` if that was intended
___
[AuctionLogic.sol: L34](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34)
```solidity
     * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
```
Change `tsatr` to `start` if that was intended
___
[LiquidationLogic.sol: L603](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603)
```solidity
     * @return The actual debt that is getting liquidated. If liquidation amount passed in by the liquidator is greater then the total user debt, then use the user total debt as the actual debt getting liquidated. If the user total debt is greater than the liquidation amount getting passed in by the liquidator, then use the liquidation amount the user is passing in.
```
Change the first `then` to `than`
___
[MarketplaceLogic.sol: L195](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L195)
```solidity
            // eg. The following example image that the `taker` owns only ETH and wants to
```
Change `image` to `imagines` if that was intended
___
[NFTFloorOracle.sol: L11](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L11)
```solidity
//we do not accept price lags behind to much
```
Change `to` to `too`
___
The same typo (`aggeregate`) occurs in all three lines referenced below:

[NFTFloorOracle.sol: L53](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L53)

[NFTFloorOracle.sol: L54](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L54)

[NFTFloorOracle.sol: L412](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412)
```solidity
/// aggeregate prices which are not expired from different feeders, if number of valid/unexpired prices
```
Change `aggeregate` to `aggregate`
___
[PoolAddressesProvider.sol: L336](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L336)
```solidity
     * @notice Returns the the implementation contract of the proxy contract by its identifier.
```
Remove repeated word `the`
___
___

| No. | Description |
|--| ----------- | 
|9|Unclear comment|
|  |Comments should communicate clearly, immediately and without ambiguity. The readability of the comment below could be improved:|

[AuctionLogic.sol: L34](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34)
```solidity
     * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
```
This `@notice` is challenging to understand even after correcting `tsatr` to `start`
___
___

| No. | Description |
|--| ----------- | 
|10|Inconsistent comment // spacing|
|  |Some lines use //x (///x) whereas others use // x (/// x). While the choice of spacing is up to the developers, it should be made consistent (normally // x or /// x is used). |

[NFTFloorOracle.sol: L8-21](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L8-L21)
```solidity
//we need to deploy 3 oracles at least
uint8 constant MIN_ORACLES_NUM = 3;
//expirationPeriod at least the interval of client to feed data(currently 6h=21600s/12=1800 in mainnet)
//we do not accept price lags behind to much
uint128 constant EXPIRATION_PERIOD = 1800;
//reject when price increase/decrease 1.5 times more than original value
uint128 constant MAX_DEVIATION_RATE = 150;

struct OracleConfig {
    // Expiration Period for each feed price
    uint128 expirationPeriod;
    // Maximum deviation allowed between two consecutive oracle prices
    uint128 maxPriceDeviation;
}
```
___
___

| No. | Description |
|--| ----------- | 
|11|Solidity version should be upgraded|
|  |The current solidity version in contracts is `0.8.10`, compared to the latest version of `0.8.17`. Only the latest version receives security fixes. |
|  |On the other hand, the latest version often has bugs, so it's safer to use releases a few versions older at first, as has been done here. |
|  |Having said that, version 0.8.12 or later is needed for `string.concat` to be used instead of `abi.encodePacked` and 0.8.13 is required to get the ability to use `using for` with a list of free functions |
___
___

| No. | Description |
|--| ----------- | 
|12|TODOs and other open items|
|  |Comments that refer to open items should be addressed and modified or removed |

[UniswapV3OracleWrapper.sol: L238](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238)
```solidity
        // TODO using bit shifting for the 2^96
```
___
[MarketplaceLogic.sol: L442](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442)
```solidity
        // TODO: support PToken
```
___
[NToken.sol: L220-222](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L220-L222)
```solidity
    {
        // Intentionally left blank
    }
```
___
[NTokenMoonBirds.sol: L32-34](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L32-L34)
```solidity
    constructor(IPool pool) NToken(pool, false) {
        // Intentionally left blank
    }
```
___
___

