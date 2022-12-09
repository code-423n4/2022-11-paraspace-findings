# GASOP Report

|         | Issue                                                                                                              | Instances |
| ------- | :----------------------------------------------------------------------------------------------------------------- | :-------: |
| [G-001] | Splitting require() statements that use `&&` saves gas                                                             |    11     |
| [G-002] | Don't use `_msgSender()` if not supporting EIP-2771                                                                |     3     |
| [G-003] | Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate |    12     |
| [G-004] | Using `calldata` instead of memory for read-only arguments in external functions saves gas                         |     8     |
| [G-005] | internal functions only called once can be inlined to save gas                                                     |     7     |
| [G-006] | Replace modifier with function                                                                                     |     5     |
| [G-007] | Use elementary types or a user-defined type instead of a struct that has only one member                           |     1     |
| [G-008] | Use named returns for local variables where it is possible.                                                        |     6     |

## [G-001] Splitting require() statements that use `&&` saves gas

### Impact

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by 3 gas.

### Findings

Total:11

[paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-175](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-175)

```solidity
171:    require(
172:                marketplaceIds.length == payloads.length &&
173:                    payloads.length == credits.length,
174:                Errors.INCONSISTENT_PARAMS_LENGTH
175:            );
```

[paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279-283](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279-283)

```solidity
279:    require(
280:                marketplaceIds.length == payloads.length &&
281:                    payloads.length == credits.length,
282:                Errors.INCONSISTENT_PARAMS_LENGTH
283:            );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-549](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-549)

```solidity
546:    require(
547:                vars.collateralReserveActive && vars.principalReserveActive,
548:                Errors.RESERVE_INACTIVE
549:            );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L636-639](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L636-639)

```solidity
636:    require(
637:                vars.collateralReserveActive && vars.principalReserveActive,
638:                Errors.RESERVE_INACTIVE
639:            );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L550-553](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L550-553)

```solidity
550:    require(
551:                !vars.collateralReservePaused && !vars.principalReservePaused,
552:                Errors.RESERVE_PAUSED
553:            );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L640-643](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L640-643)

```solidity
640:    require(
641:                !vars.collateralReservePaused && !vars.principalReservePaused,
642:                Errors.RESERVE_PAUSED
643:            );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L672-676](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L672-676)

```solidity
672:    require(
673:                params.maxLiquidationAmount >= params.actualLiquidationAmount &&
674:                    (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
675:                Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
676:            );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1196-1199](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1196-1199)

```solidity
1196:    require(
1197:                    vars.token0IsActive && vars.token1IsActive,
1198:                    Errors.RESERVE_INACTIVE
1199:                );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1202-1205](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1202-1205)

```solidity
1202:    require(
1203:                    !vars.token0IsPaused && !vars.token1IsPaused,
1204:                    Errors.RESERVE_PAUSED
1205:                );
```

[paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1208-1211](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1208-1211)

```solidity
1208:    require(
1209:                    !vars.token0IsFrozen && !vars.token1IsFrozen,
1210:                    Errors.RESERVE_FROZEN
1211:                );
```

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-220](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-220)

```solidity
216:    require(
217:                value > MIN_AUCTION_HEALTH_FACTOR &&
218:                    value <= MAX_AUCTION_HEALTH_FACTOR,
219:                Errors.INVALID_AMOUNT
220:            );
```

## [G-002] Don't use `_msgSender()` if not supporting EIP-2771

### Impact

The use of `_msgSender()` when there is no implementation of a meta transaction mechanism that uses it, such as EIP-2771, very slightly increases gas consumption.(**Review EIP-2771 in contract**)

### Findings

Total:3

[paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L99](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L99)

```solidity
99:    _isApprovedOrOwner(_msgSender(), tokenIds[index]),
```

[paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L260](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L260)

[paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L297](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L297)

```solidity
260:    _isApprovedOrOwner(_msgSender(), tokenId),
...
297:    _isApprovedOrOwner(_msgSender(), tokenId),
```

## [G-003] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

### Impact

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

### Findings

Total:12

[paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L25-28](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L25-28)

```solidity
25:        mapping(bytes32 => address) private _addresses;
26:
27:        // Map of marketplace contracts (id => address)
28:        mapping(bytes32 => DataTypes.Marketplace) internal _marketplaces;
```

[paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L43-44](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L43-44)

```solidity
43:            mapping(uint256 => address) storage reservesList,
44:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L103-104](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L103-104)

```solidity
103:            mapping(uint256 => address) storage reservesList,
104:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L163-164](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L163-164)

```solidity
163:            mapping(uint256 => address) storage reservesList,
164:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L288-289](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L288-289)

```solidity
288:            mapping(uint256 => address) storage reservesList,
289:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L488-489](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L488-489)

```solidity
488:            mapping(uint256 => address) storage reservesList,
489:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L464-465](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L464-465)

```solidity
464:            mapping(uint256 => address) storage reservesList,
465:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L525-526](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L525-526)

```solidity
525:            mapping(uint256 => address) storage reservesList,
526:            mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
```

[paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L24-26](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L24-26)

```solidity
24:        mapping(uint256 => address) owners;
25:        // Mapping from owner to list of owned token IDs
26:        mapping(address => mapping(uint256 => uint256)) ownedTokens;
```

[paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L32-34](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L32-34)

```solidity
32:        mapping(uint256 => uint256) allTokensIndex;
33:        // Map of users address and their state data (userAddress => userStateData)
34:        mapping(address => UserState) userState;
```

[paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L36-40](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L36-40)

```solidity
36:        mapping(uint256 => address) tokenApprovals;
37:        // Mapping from owner to operator approvals
38:        mapping(address => mapping(address => bool)) operatorApprovals;
39:        // Map of allowances (delegator => delegatee => allowanceAmount)
40:        mapping(address => mapping(address => uint256)) allowances;
```

[paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L205-206](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L205-206)

```solidity
205:            mapping(address => UserState) storage userState,
206:            mapping(address => mapping(uint256 => uint256)) storage ownedTokens,
```

## [G-004] Using `calldata` instead of memory for read-only arguments in external functions saves gas

### Impact

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a `for-loop` to copy each index of the `calldata` to the `memory` index. Each iteration of this `for-loop` costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having `memory` arguments, it's still valid for implementation contracs to use `calldata` arguments instead.  If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gass-efficient to use calldata when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one.

### Findings

Total:8

[paraspace-core/contracts/misc/NFTFloorOracle.sol#L97-100](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/NFTFloorOracle.sol#L97-100)

```solidity
97:    function initialize(
98:            address _admin,
99:            address[] memory _feeders,
100:            address[] memory _assets
```

[paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-98](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-98)

```solidity
97:    function withdrawBAKC(
98:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
```

[paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97-98](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97-98)

```solidity
97:    function withdrawBAKC(
98:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
```

[paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L38-41](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L38-41)

```solidity
38:    function withdrawBAKC(
39:            ApeCoinStaking _apeCoinStaking,
40:            uint256 poolId,
41:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
```

[paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L232-235](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L232-235)

```solidity
232:    function withdrawBAKC(
233:            ApeCoinStaking _apeCoinStaking,
234:            uint256 poolId,
235:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
```

[paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L60-62](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L60-62)

```solidity
60:    function withdrawBAKC(
61:            address nftAsset,
62:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs
```

[paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L120-122](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L120-122)

```solidity
120:    function withdrawBAKC(
121:            address nftAsset,
122:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs
```

[paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L188-190](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L188-190)

```solidity
188:    function withdrawBAKC(
189:            address nftAsset,
190:            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs
```

## [G-005] internal functions only called once can be inlined to save gas

### Impact

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

### Findings

Total:7

[paraspace-core/contracts/misc/NFTFloorOracle.sol#L265](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/NFTFloorOracle.sol#L265)

```solidity
265:    function _whenNotPaused(address _asset) internal view {
```

[paraspace-core/contracts/misc/NFTFloorOracle.sol#L388](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/NFTFloorOracle.sol#L388)

```solidity
388:    function _addRawValue(address _asset, uint256 _twap) internal {
```

[paraspace-core/contracts/misc/ParaSpaceOracle.sol#L218](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/ParaSpaceOracle.sol#L218)

```solidity
218:    function _onlyAssetListingOrPoolAdmins() internal view {
```

[paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L552](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L552)

```solidity
552:    function _checkAllowance(address token, address operator) internal {
```

[paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L144](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L144)

```solidity
144:    function _safeTransferETH(address to, uint256 value) internal {
```

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L69)

```solidity
69:    function _onlyPoolConfigurator() internal view virtual {
```

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L76](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L76)

```solidity
76:    function _onlyPoolAdmin() internal view virtual {
```

## [G-006] Replace modifier with function

### Impact

Modifiers make code more elegant, but cost more than normal functions.

### Findings

Total:5

[paraspace-core/contracts/misc/ParaSpaceOracle.sol#L34](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/ParaSpaceOracle.sol#L34)

```solidity
34:    modifier onlyAssetListingOrPoolAdmins() {
```

[paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L45](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L45)

```solidity
45:    modifier onlyPoolAdmin() {
```

[paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L59](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L59)

```solidity
59:    modifier onlyPool() {
```

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L56](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L56)

```solidity
56:    modifier onlyPoolConfigurator() {
```

[paraspace-core/contracts/protocol/pool/PoolParameters.sol#L64](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/pool/PoolParameters.sol#L64)

```solidity
64:    modifier onlyPoolAdmin() {
```

## [G-007] Use elementary types or a user-defined type instead of a struct that has only one member

### Impact

Use elementary types or a user-defined type instead of a struct that has only one member.

### Findings

Total:1

[paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L26-28](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L26-28)

```solidity
26:        struct APEStakingParameter {
27:            uint256 unstakeIncentive;
28:        }
```

## [G-008] Use named returns for local variables where it is possible.

### Impact

It is not necessary to have both a named return and a return statement.

### Findings

Total:6

[paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L203-L215](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L203-L215)

```solidity
    function getTokensPricesSum(uint256[] calldata tokenIds)
        external
        view
        returns (uint256)
    {
        uint256 sum = 0;

        for (uint256 index = 0; index < tokenIds.length; index++) {
            sum += getTokenPrice(tokenIds[index]);
        }

        return sum;
    }
```

[paraspace-core/contracts/misc/ParaSpaceOracle.sol#L115-L136](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/misc/ParaSpaceOracle.sol#L115-L136)

```solidity
    function getAssetPrice(address asset)
        public
        view
        override
        returns (uint256)
    {
        if (asset == BASE_CURRENCY) {
            return BASE_CURRENCY_UNIT;
        }

        uint256 price = 0;
        IEACAggregatorProxy source = IEACAggregatorProxy(assetsSources[asset]);
        if (address(source) != address(0)) {
            price = uint256(source.latestAnswer());
        }
        if (price == 0 && address(_fallbackOracle) != address(0)) {
            price = _fallbackOracle.getAssetPrice(asset);
        }

        require(price != 0, Errors.ORACLE_PRICE_NOT_READY);
        return price;
    }
```

[paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L305-L321](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L305-L321)

```solidity
    function calculateAvailableBorrows(
        uint256 totalCollateralInBaseCurrency,
        uint256 totalDebtInBaseCurrency,
        uint256 ltv
    ) internal pure returns (uint256) {
        uint256 availableBorrowsInBaseCurrency = totalCollateralInBaseCurrency
            .percentMul(ltv);

        if (availableBorrowsInBaseCurrency < totalDebtInBaseCurrency) {
            return 0;
        }

        availableBorrowsInBaseCurrency =
            availableBorrowsInBaseCurrency -
            totalDebtInBaseCurrency;
        return availableBorrowsInBaseCurrency;
    }
```

[paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L270-L333](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L270-L333)

```solidity
    function executeWithdraw(
        mapping(address => DataTypes.ReserveData) storage reservesData,
        mapping(uint256 => address) storage reservesList,
        DataTypes.UserConfigurationMap storage userConfig,
        DataTypes.ExecuteWithdrawParams memory params
    ) external returns (uint256) {
        ...
        uint256 amountToWithdraw = params.amount;
        ...
        return amountToWithdraw;
    }
```

[paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L353-L394](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L353-L394)

```solidity
    function executeWithdrawERC721(
        mapping(address => DataTypes.ReserveData) storage reservesData,
        mapping(uint256 => address) storage reservesList,
        DataTypes.UserConfigurationMap storage userConfig,
        DataTypes.ExecuteWithdrawERC721Params memory params
    ) external returns (uint256) {
        ...
        uint256 amountToWithdraw = params.tokenIds.length;
        ...
        return amountToWithdraw;
    }

```

[paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L127-L211](https://github.com/code-423n4/2022-11-paraspace/tree/main//paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L127-L211)

```solidity
    function executeRepay(
        mapping(address => DataTypes.ReserveData) storage reservesData,
        DataTypes.UserConfigurationMap storage userConfig,
        DataTypes.ExecuteRepayParams memory params
    ) external returns (uint256) {
        ...
        uint256 paybackAmount = variableDebt;
        ...
        return paybackAmount;
    }
```
