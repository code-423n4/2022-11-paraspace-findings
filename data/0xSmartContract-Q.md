## Summary
### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]| Avoid _shadowing_ `inherited state variables`| 1 |
|[L-02]| Use `Ownable2StepUpgradeable` instead of ` OwnableUpgradeable ` contract | 1 |
|[L-03]| Owner can renounce Ownership| 1 |
|[L-04]| Critical Address Changes Should Use Two-step Procedure| 7 |
|[L-05]| Initial value check is missing in Set Functions| 7 |
|[L-06]| Using Vulnerable Version of Openzeppelin |  |
|[L-07]|initialize() function can be called by anybody | 1 |
|[L-08]| The `nonReentrant` modifier should occur before all other modifiers| 18 |

Total 8 issues


### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [N-01]|Not using the named return variables anywhere in the function is confusing|2|
| [N-02] |Same Constant redefined elsewhere  |4|
| [N-03] |`MintableIncentivizedERC721.tokenURI`  violates EIP-721| 1 |
| [N-04] | NatSpec is missing | 16 |
| [N-05] |Constant values such as a call to keccak256(), should used to immutable rather than constant | 3|
| [N-06] |For functions, follow Solidity standard naming conventions  | 15 |
| [N-07] |Use a single file for all system-wide constants| 28 |
| [N-08] |`Function writing` that does not comply with the `Solidity Style Guide`  | All contracts |
| [N-09] |Lack of event emission after critical `initialize()` functions | 2 |
| [N-10] |Add a timelock to critical functions| 18  |
| [N-11] |Use a more recent version of Solidity| All contracts |
| [N-12] |Solidity compiler optimizations can be problematic |  |
| [N-13] |Open TODOs | 4 |
| [N-14] |For modern and more readable code; update import usages| 30 |
| [N-15] |`Empty blocks` should be _removed_ or _Emit_ something | 1 |
| [N-16] |NatSpec comments should be increased in contracts | All |
| [N-17] |`WETH` address definition can be use _directly_| 1 |
| [N-18] |Avoid variable names that can shade | 1 |
| [N-19] |Use Underscores for Number Literals | 5 |

Total 19 issues


### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Make the Test Context with Solidity |
| [S-02] |Generate perfect code headers every time |

Total 2 suggestions

## [L-01] Avoid _shadowing_ `inherited state variables`

**Context:**
[PoolAddressesProvider.sol#L47](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L47)

**Description:**
In ```PoolAddressesProvider.sol#L47``` , there is a local variable named ```owner```, but there is a state varible named ```owner``` in the inherited ```Owned.sol``` with the same name. This use causes compilers to issue warnings, negatively affecting checking and code readability. 

```solidity

paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:
  44       */
  45:     constructor(string memory marketId, address owner) {
  46:         _setMarketId(marketId);
  47:         transferOwnership(owner);
  48:     }


paraspace-core/contracts/dependencies/openzeppelin/contracts/Ownable.sol:
  39:     function owner() public view returns (address) {
```

**Recommendation:**
Avoid using variables with the same name, including inherited in the same contract, if used, it must be specified in the NatSpec comments.

### [L-02] Use `Ownable2StepUpgradeable` instead of ` OwnableUpgradeable ` contract

**Context:**
[PoolAddressesProvider.sol#L20](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L20)

**Description:**
```transferOwnership``` function is used to change Ownership from ```OwnableUpgradeable.sol```.

There is another Openzeppelin Ownable contract (Ownable2StepUpgradeable.sol) has  ` transferOwnership` function ,  use it is more secure due to 2-stage ownership transfer.

[Ownable2StepUpgradeable.sol](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/access/Ownable2StepUpgradeable.sol)


### [L-03] Owner can renounce Ownership

**Context:**
[PoolAddressesProvider.sol#L20](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L20)


**Description:**
Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

The Openzeppelin’s Ownable used in this project contract implements renounceOwnership. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

`onlyOwner` functions;
```js
paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:

    56:     function setMarketId(string memory newMarketId) external override onlyOwner {
    70:     function setAddress(bytes32 id, address newAddress) external override onlyOwner
    81:     function setAddressAsProxy(bytes32 id, address newImplementationAddress) external override onlyOwner
   105:     function updatePoolImpl( IParaProxy.ProxyImplementation[] calldata implementationParams, address _init, bytes calldata _calldata) external override onlyOwner {
   121:     function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) external override onlyOwner {
   142:     function setPriceOracle(address newPriceOracle) external override onlyOwner {
   158:     function setACLManager(address newAclManager) external override onlyOwner {
   170:     function setACLAdmin(address newAclAdmin) external override onlyOwner {

```

**Recommendation:**
We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.


### [L-04] Critical Address Changes Should Use Two-step Procedure

The critical procedures should be two step process.

**Context:**
7 results 1 files

```solidity
paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:
    70:     function setAddress(bytes32 id, address newAddress) external override onlyOwner
    81:     function setAddressAsProxy(bytes32 id, address newImplementationAddress) external override onlyOwner
  105:     function updatePoolImpl( IParaProxy.ProxyImplementation[] calldata implementationParams, address _init, bytes calldata _calldata) external override onlyOwner {
  121:     function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) external override onlyOwner {
  142:     function setPriceOracle(address newPriceOracle) external override onlyOwner {
  158:     function setACLManager(address newAclManager) external override onlyOwner {
  170:     function setACLAdmin(address newAclAdmin) external override onlyOwner {

```

Recommended Mitigation Steps
Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

### [L-05] Initial value check is missing in Set Functions

**Context:**
7 results 1 files

```solidity
paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:
    70:     function setAddress(bytes32 id, address newAddress) external override onlyOwner
    81:     function setAddressAsProxy(bytes32 id, address newImplementationAddress) external override onlyOwner
  105:     function updatePoolImpl( IParaProxy.ProxyImplementation[] calldata implementationParams, address _init, bytes calldata _calldata) external override onlyOwner {
  121:     function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) external override onlyOwner {
  142:     function setPriceOracle(address newPriceOracle) external override onlyOwner {
  158:     function setACLManager(address newAclManager) external override onlyOwner {
  170:     function setACLAdmin(address newAclAdmin) external override onlyOwner {

```
Checking whether the current value and the new value are the same should be added

### [L-06]  Using Vulnerable Version of Openzeppelin
The package.json configuration file says that the project is using 4.6.0 of OpenZeppelin which has a vulnerability in initializers that call external contracts, 

openzeppelin/contracts vulnerabilities:
https://security.snyk.io/package/npm/@openzeppelin%2Fcontracts/


```solidity
paraspace-core/package.json:
  31:     "@openzeppelin/contracts": "4.2.0",
  32:     "@openzeppelin/contracts-upgradeable": "4.2.0",
```

**Recommendation:**
Use patched versions

### [L-07] initialize() function can be called by anybody

`initialize()` function can be called anybody when the contract is not initialized.

More importantly, if someone else runs this function, they will have full authority because of the `__Ownable_init()` function.

Here is a definition of `initialize()` function.

```solidity

paraspace-core/contracts/ui/WPunkGateway.sol:
  56  
  57:     function initialize() external initializer {
  58:         __Ownable_init();
  59: 
  60:         // create new WPunk Proxy for PunkGateway contract
  61:         WPunk.registerProxy();
  62: 
  63:         // address(this) = WPunkGatewayProxy
  64:         // proxy of PunkGateway contract is the new Proxy created above
  65:         proxy = WPunk.proxyInfo(address(this));
  66: 
  67:         WPunk.setApprovalForAll(pool, true);
  68:     }

```


### Recommended Mitigation Steps:

Add a control that makes `initialize()` only call the Deployer Contract or EOA;

```solidity
if (msg.sender != DEPLOYER_ADDRESS) {
            revert NotDeployer();
        }
```

### [L-08] The `nonReentrant` modifier should occur before all other modifiers


```solidity
5 results 18 files

paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol:
   28          onlyPool
   29:         nonReentrant
   30      {

   41          onlyPool
   42:         nonReentrant
   43      {

   55:     ) external onlyPool nonReentrant {

   68          onlyPool
   69:         nonReentrant
   70      {

   85:     ) external onlyPool nonReentrant {

  100:     ) external onlyPool nonReentrant {

paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol:
   28          onlyPool
   29:         nonReentrant
   30      {

   41          onlyPool
   42:         nonReentrant
   43      {

   54          address _recipient
   55:     ) external onlyPool nonReentrant {
   56          _apeCoinStaking.withdrawMAYC(_nfts, _recipient);

   68          onlyPool
   69:         nonReentrant
   70      {

   84          address _recipient
   85:     ) external onlyPool nonReentrant {

   99          address _apeRecipient
  100:     ) external onlyPool nonReentrant {

paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol:
  44:     ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol:
  130:     ) external onlyPool nonReentrant {

paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:
  462:     ) external virtual override onlyPool nonReentrant returns (bool) {
  463          return

  482          onlyPool
  483:         nonReentrant
  484          returns (

  533          onlyPool
  534:         nonReentrant
  535      {

  544          onlyPool
  545:         nonReentrant
  546      {

```

This is a best-practice to protect against reentrancy in other modifiers



### [NC-01] Not using the named return variables anywhere in the function is confusing


Within the scope of the project, it is observed that this is not followed in general, the following codes can be given as an example

```solidity
2 results 1 files

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:
  192      )
  193:         external
  194:         returns (
  195:             uint64 oldCollateralizedBalance,
  196:             uint64 newCollateralizedBalance


  265      )
  266:         external
  267:         returns (
  268:             uint64 oldCollateralizedBalance,
  269:             uint64 newCollateralizedBalance
  270:         )

```

**Recommendation:**
Consider adopting a consistent approach to return values throughout the codebase by removing all named return variables, explicitly declaring them as local variables, and adding the necessary return statements where appropriate. This would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

### [NC-02] Same Constant redefined elsewhere

Keeping the same constants in different files may cause some problems or errors, reading constants from a single file is preferable. This should also be preferred in gas optimization


```solidity
4 results 4 files:

paraspace-core/contracts/protocol/pool/PoolApeStaking.sol:
  35:     uint256 internal constant POOL_REVISION = 1;

paraspace-core/contracts/protocol/pool/PoolCore.sol:
  53:     uint256 public constant POOL_REVISION = 1;

paraspace-core/contracts/protocol/pool/PoolMarketplace.sol:
  56:     uint256 internal constant POOL_REVISION = 1;

paraspace-core/contracts/protocol/pool/PoolParameters.sol:
  49:     uint256 internal constant POOL_REVISION = 1;

```
### [NC-03] `MintableIncentivizedERC721.tokenURI`  violates EIP-721

According to EIP-721, tokenURI "Throws if _tokenId is not a valid NFT." This is not the case for MintableIncentivizedERC721. In such situations, an empty string is returned.


```solidity
 /**
     * @dev See {IERC721Metadata-tokenURI}.
     */
    function tokenURI(uint256)
        external
        view
        virtual
        override
        returns (string memory)
    {
        return "";
    }
```

Recommended Mitigation Steps:
Throw if the token ID does not exist

### [NC-04] NatSpec is missing 

**Description:**
NatSpec is missing for the following functions , constructor and modifier:

**Context:**

```solidity
16 results 3 files:

paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:
   96:     function name() public view override returns (string memory) {
  100:     function symbol() external view override returns (string memory) {
  104:     function balanceOf(address account)
  364:     function _mintMultiple(
  384:     function _burnMultiple(address user, uint256[] calldata tokenIds)

paraspace-core/contracts/protocol/tokenization/NToken.sol:
  52:     function initialize(

paraspace-core/contracts/protocol/tokenization/NToken.sol:
  119:     function transferOnLiquidation(
  127:     function rescueERC20(
  136:     function rescueERC721(
  151:     function rescueERC1155(
  168:     function executeAirdrop(
  271:     function onERC721Received(
  280:     function onERC1155Received(
  295:     function onERC1155BatchReceived(
  323:     function getAtomicPricingConfig() external view returns (bool) {
  327:     function getXTokenType()

```

### [NC-05] Constant values such as a call to keccak256(), should used to immutable rather than constant

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand.

Constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.


```js
3 results 2 files:

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  70:     bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");

paraspace-core/contracts/mocks/tokens/Moonbirds.sol:
  2681:     bytes32 public constant DEFAULT_ADMIN_ROLE = 0x00;
  3202:     bytes32 public constant EXPULSION_ROLE = keccak256("EXPULSION_ROLE");
```
### [N-06 ] For functions, follow Solidity standard naming conventions

**Context:**
[MintableERC721Logic.sol#L153](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L153), [NTokenMAYC.sol#L109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L109), [NTokenBAYC.sol#L109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L109), [NTokenApeStaking.sol#L125](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L125), [NTokenApeStaking.sol#L130](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L130), [NTokenApeStaking.sol#L143](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L143), [PoolParameters.sol#L49](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L49), [PoolParameters.sol#L49](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L50), [PoolParameters.sol#L49](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L51), [PoolParameters.sol#L93](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L93), [PoolCore.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L56), [PoolApeStaking.sol#L55](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L55), [PoolApeStaking.sol#L413](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413), [PoolApeStaking.sol#L428](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L428), [PoolApeStaking.sol#L443](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L443)

**Description:**
The above codes don't follow Solidity's standard naming convention,

internal and private functions : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)

### [NC-07] Use a single file for all system-wide constants

There are many addresses and constants used in the system. It is recommended to put the most used ones in one file (for example constants.sol, use inheritance to access these values)

  This will help with readability and easier maintenance for future changes. This also helps with any issues, as some of these hard-coded values are admin addresses.

constants.left
Use and import this file in contracts that require access to these values. This is just a suggestion, in some use cases this may result in higher gas usage in the distribution.

```solidity
28 results 14 files:

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  70:     bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");

paraspace-core/contracts/protocol/configuration/ACLManager.sol:
  15:     bytes32 public constant override POOL_ADMIN_ROLE = keccak256("POOL_ADMIN");
  16:     bytes32 public constant override EMERGENCY_ADMIN_ROLE =
  18:     bytes32 public constant override RISK_ADMIN_ROLE = keccak256("RISK_ADMIN");
  19:     bytes32 public constant override FLASH_BORROWER_ROLE =
  21:     bytes32 public constant override BRIDGE_ROLE = keccak256("BRIDGE");
  22:     bytes32 public constant override ASSET_LISTING_ADMIN_ROLE =

paraspace-core/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:
  58:     uint16 public constant MAX_RESERVES_COUNT = 128;

paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol:
  57:     uint256 public constant MAX_LIQUIDATION_CLOSE_FACTOR = 1e4;
  64:     uint256 public constant CLOSE_FACTOR_HF_THRESHOLD = 0.95e18;

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:
  45:     uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;
  49:     uint256 public constant MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD =
  56:     uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;

paraspace-core/contracts/protocol/libraries/types/DataTypes.sol:
  12:     address public constant SApeAddress = address(0x1);
  13:     uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;

paraspace-core/contracts/protocol/pool/PoolConfigurator.sol:
  69:     uint256 public constant CONFIGURATOR_REVISION = 0x1;

paraspace-core/contracts/protocol/pool/PoolCore.sol:
  53:     uint256 public constant POOL_REVISION = 1;
  54      IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;

paraspace-core/contracts/protocol/tokenization/NToken.sol:
  32:     uint256 public constant NTOKEN_REVISION = 0x1;

paraspace-core/contracts/protocol/tokenization/PToken.sol:
  34:     bytes32 public constant PERMIT_TYPEHASH =
  39:     uint256 public constant PTOKEN_REVISION = 0x1;

paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol:
  32:     uint256 public constant DEBT_TOKEN_REVISION = 0x1;

paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol:
  25:     bytes32 public constant DELEGATION_WITH_SIG_TYPEHASH =

paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol:
  10:     bytes public constant EIP712_REVISION = bytes("1");
  11      bytes32 internal constant EIP712_DOMAIN =

paraspace-core/contracts/ui/UiPoolDataProvider.sol:
  37:     uint256 public constant ETH_CURRENCY_UNIT = 1 ether;
  38:     address public constant MKR_ADDRESS =
  40:     address public constant SAPE_ADDRESS = address(0x1);


```

### [NC-08] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
All Contracts

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

 constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
within a grouping, place the view and pure functions last


### [NC-09] Lack of event emission after critical `initialize()` functions

To record the init parameters for off-chain monitoring and transparency reasons, please consider emitting an event after the `initialize()` functions:

```solidity
2 results 2 files:

paraspace-core/contracts/ui/WPunkGateway.sol:
  56  
  57:     function initialize() external initializer {
  58:         __Ownable_init();
  59: 
  60:         // create new WPunk Proxy for PunkGateway contract
  61:         WPunk.registerProxy();
  62: 
  63:         // address(this) = WPunkGatewayProxy
  64:         // proxy of PunkGateway contract is the new Proxy created above
  65:         proxy = WPunk.proxyInfo(address(this));
  66: 
  67:         WPunk.setApprovalForAll(pool, true);
  68:     }

paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol:
  35  
  36:     function initialize(
  37:         IPool initializingPool,
  38:         address underlyingAsset,
  39:         IRewardController incentivesController,
  40:         string calldata nTokenName,
  41:         string calldata nTokenSymbol,
  42:         bytes calldata params
  43:     ) public virtual override initializer {
  44:         IERC20 _apeCoin = _apeCoinStaking.apeCoin();
  45:         _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
  46:         _apeCoin.approve(address(POOL), type(uint256).max);
  47:         getBAKC().setApprovalForAll(address(POOL), true);
  48: 
  49:         super.initialize(
  50:             initializingPool,
  51:             underlyingAsset,
  52:             incentivesController,
  53:             nTokenName,
  54:             nTokenSymbol,
  55:             params
  56:         );
  57: 
  58:         initializeStakingData();
  59:     }

```


### [NC-10] Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).
Consider adding a timelock to:

```solidity
18 results 5 files

paraspace-core/contracts/misc/ERC721OracleWrapper.sol:
  44:     function setOracle(address _oracleAddress)

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  175:     function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
  183:     function setPause(address _asset, bool _flag)
  195:     function setPrice(address _asset, uint256 _twap)
  221:     function setMultiplePrices(

paraspace-core/contracts/misc/ParaSpaceOracle.sol:
  66:     function setAssetSources(
  74:     function setFallbackOracle(address fallbackOracle)

paraspace-core/contracts/protocol/configuration/ACLManager.sol:
  39      /// @inheritdoc IACLManager
  40:     function setRoleAdmin(bytes32 role, bytes32 adminRole)
  41          external

paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:
   56:     function setMarketId(string memory newMarketId)
   70:     function setAddress(bytes32 id, address newAddress)
   81:     function setAddressAsProxy(bytes32 id, address newImplementationAddress)
  121:     function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
  142:     function setPriceOracle(address newPriceOracle)
  158:     function setACLManager(address newAclManager) external override onlyOwner {
  170:     function setACLAdmin(address newAclAdmin) external override onlyOwner {
  182:     function setPriceOracleSentinel(address newPriceOracleSentinel)
  224:     function setProtocolDataProvider(address newDataProvider)
  242:     function setMarketplace(
```
### [NC-11] Use a more recent version of Solidity

**Context:**
All contracts

**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used , newer version can be used `(0.8.17)` 

### [NC-12] Solidity compiler optimizations can be problematic

```js

paraspace-core/hardhat.config.ts:
  71:     compilers: [
  72:       {
  73:         version: "0.8.10",
  74:         settings: {
  75:           optimizer: {
  76:             enabled: true,
  77:             runs: 4000,
  78            },

```

**Description:**
Protocol has enabled optional compiler optimizations in Solidity.
There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. 

Therefore, it is unclear how well they are being tested and exercised.
High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. 

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported.
A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe.
It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario
A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.


**Recommendation:**
Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.
Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.



### [NC-13] Open TODOs

**Context:**
```solidity
4 results 4 files:

paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:
  238:         // TODO using bit shifting for the 2^96

paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol:
  59:             makerAsk.price, // TODO: take minPercentageToAsk into account

paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol:
  442:         // TODO: support PToken

paraspace-core/contracts/ui/UiIncentiveDataProvider.sol:
  68:             // TODO: check that this is deployed correctly on contract and remove casting

```

**Recommendation:**
Use temporary TODOs as you work on a feature, but make sure to treat them before merging. Either add a link to a proper issue in your TODO, or remove it from the code.



### [N-14] For modern and more readable code; update import usages

**Context:**

```solidity
30 results 8 files:

paraspace-core/contracts/misc/NFTFloorOracle.sol:
  4: import "../dependencies/openzeppelin/contracts/AccessControl.sol";
  5: import "../dependencies/openzeppelin/upgradeability/Initializable.sol";
  6: import "./interfaces/INFTFloorOracle.sol";
    

paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol:
   4: import "../interfaces/IFlashClaimReceiver.sol";
   5: import "../../dependencies/openzeppelin/contracts/Address.sol";
   6: import "../../dependencies/openzeppelin/contracts/Ownable.sol";
   7: import "../../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";
   8: import "../../dependencies/openzeppelin/contracts/IERC20.sol";
   9: import "../../dependencies/openzeppelin/contracts/IERC721.sol";
  10: import "../../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
  11: import "../../dependencies/openzeppelin/contracts/ERC721Holder.sol";
  12: import "../../dependencies/openzeppelin/contracts/IERC1155.sol";
  13: import "../../dependencies/openzeppelin/contracts/ERC1155Holder.sol";

paraspace-core/contracts/misc/flashclaim/UserFlashclaimRegistry.sol:
  4: import "./AirdropFlashClaimReceiver.sol";

paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol:
  30: import "../../../interfaces/INTokenApeStaking.sol";

paraspace-core/contracts/protocol/pool/PoolApeStaking.sol:
   4: import "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
   5: import "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
   7: import "../../interfaces/IPoolApeStaking.sol";
   8: import "../../interfaces/IPToken.sol";
   9: import "../../dependencies/yoga-labs/ApeCoinStaking.sol";
  10: import "../../interfaces/IXTokenType.sol";
  11: import "../../interfaces/INTokenApeStaking.sol";
  20: import "../libraries/logic/BorrowLogic.sol";
  21: import "../libraries/logic/SupplyLogic.sol";

paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol:
  12: import "../../interfaces/INTokenApeStaking.sol";

paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:
   7: import "../../../interfaces/IPool.sol";
  11: import "./MintableERC721Logic.sol";

paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:
  7: import "../../../interfaces/IRewardController.sol";
  8: import "../../libraries/types/DataTypes.sol";
  9: import "../../../interfaces/IPool.sol";
```


**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;
```js
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```

### [NC-15] `Empty blocks` should be _removed_ or _Emit_ something

**Description:**
Code contains empty block

```solidity
1 result 1 file:

paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol:
  148  
  149:     receive() external payable {}
  150  }

```

**Recommendation:**
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

### [NC-16] NatSpec comments should be increased in contracts

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
NatSpec comments should be increased in contracts

### [NC-17] `WETH` address definition can be use _directly_

**Context:**
```solidity
1 result 1 file:

paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol:
   92:         address weth = _addressesProvider.getWETH();

  108:        uint256 balanceWeth = IERC20(weth).balanceOf(address(this));

```

**Description:**
WETH is a wrap Ether contract with a specific address in the Etherum network, giving the option to define it may cause false recognition, it is healthier to define it directly.

Advantages of defining a specific contract directly:
- It saves gas,
- Prevents incorrect argument definition, 
- Prevents execution on a different chain and re-signature issues,

WETH Address : 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2


**Recommendation:**
```js
address private constant WETH = 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2;
```

### [NC-18] Avoid variable names that can shade

With global variable names in the form of  `call{value: value }` , argument name similarities can shade and negatively affect code readability.


```solidity

    function _safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, "ETH_TRANSFER_FAILED");
    }

```

### [NC-19] Use Underscores for Number Literals


```solidity
5 results 2 files:

paraspace-core/contracts/mocks/tokens/MAYC.sol:
  1435:     uint16 private constant MAX_MEGA_MUTATION_ID = 30007;
  1436:     uint256 public constant SERUM_MUTATION_OFFSET = 10000;
  1440:     uint256 public constant PS_MAX_MUTANTS = 10000;
  1442:     uint256 public constant PS_MUTANT_ENDING_PRICE = 10000000000000000;

paraspace-core/contracts/mocks/tokens/BAYC.sol:
  2228:     uint256 public constant apePrice = 80000000000000000; //0.08 ETH

```

There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

Recommendation:
Consider using underscores for number literals to improve its readability


### [S-01] Make the Test Context with Solidity

It's crucial to write tests with possibly 100% coverage for smart contract systems.

It is recommended to write appropriate tests for all possible code streams and especially for extreme cases.

But the other important point is the test context, tests written with solidity are safer, it is recommended to focus on tests with Foundry


### [S-02] Generate perfect code headers every time

**Description:**
I recommend using header for Solidity code layout and readability

https://github.com/transmissions11/headers

```js
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```
