## Remove empty constructor

### Where
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L23

https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L23

https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L24


# ./misc/NFTFloorOracle.sol

## Funcionality on initialize function can be moved to the constructo (since there is no proxies or upgradeability functions associated with this contract).

```solidity
    /// @notice Allow contract creator to set admin and updaters
    /// @param _admin The admin who can change roles
    /// @param _feeders The initial updaters
    /// @param _assets The initial nft assets
    function initialize(
        address _admin,
        address[] memory _feeders,
        address[] memory _assets
    ) public initializer {
        _addAssets(_assets);
        _addFeeders(_feeders);
        _setupRole(DEFAULT_ADMIN_ROLE, _admin);
        //still need to grant update_role to admin for emergency call
        _setupRole(UPDATER_ROLE, _admin);
        _setConfig(EXPIRATION_PERIOD, MAX_DEVIATION_RATE);
    }
```

change it to

```solidity
    /// @notice Allow contract creator to set admin and updaters
    /// @param _admin The admin who can change roles
    /// @param _feeders The initial updaters
    /// @param _assets The initial nft assets
    constructor(
        address _admin,
        address[] memory _feeders,
        address[] memory _assets
    ) {
        _addAssets(_assets);
        _addFeeders(_feeders);
        _setupRole(DEFAULT_ADMIN_ROLE, _admin);
        //still need to grant update_role to admin for emergency call
        _setupRole(UPDATER_ROLE, _admin);
        _setConfig(EXPIRATION_PERIOD, MAX_DEVIATION_RATE);
    }
```

and remove the Initializable.sol dependency

## Duplicated verification `onlyWhenFeederExisted()` on `removeFeeder()` call.

Modifier `onlyWhenFeederExisted()` is being called twice when `removeFeeder()` is executed. Once from the function itself and one more time in `_removeFeeder()` resulting in gas wasted.

Remove the validation from `removeFeeder()` function

```solidity
    /// @notice Allows owner to remove feeder.
    /// @param _feeder feeder to remove
    function removeFeeder(address _feeder)
        external
        // onlyWhenFeederExisted(_feeder) # remove this line
    {
        _removeFeeder(_feeder);
    }
```

## Duplicated verification `onlyWhenAssetExisted()` on `removeAsset()` call.

Modifier `onlyWhenAssetExisted()` is being called twice when `removeAsset()` is executed. Once from the function itself and one more time in `_removeAsset()` resulting in gas wasted.

Remove the validation from `removeAsset()` function

```solidity
    /// @notice Allows owner to remove asset.
    /// @param _asset asset to remove
    function removeAsset(address _asset)
        external
        onlyRole(DEFAULT_ADMIN_ROLE)
        // onlyWhenAssetExisted(_asset) # remove this line
    {
        _removeAsset(_asset);
    }
```
