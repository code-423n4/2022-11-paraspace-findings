## Title
Redundant modifier onlyWhenFeederExisted Usage
## Affected Code
contracts/misc/NFTFloorOracle.sol 
```
/// @notice Allows owner to remove feeder.
/// @param _feeder feeder to remove
function removeFeeder(address _feeder)
    external
    onlyWhenFeederExisted(_feeder)
{
    _removeFeeder(_feeder);
}

function _removeFeeder(address _feeder)
    internal
    onlyWhenFeederExisted(_feeder)
{
    uint8 feederIndex = feederPositionMap[_feeder].index;
    if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
        feeders[feederIndex] = feeders[feeders.length - 1];
        feeders.pop();
    }
    delete feederPositionMap[_feeder];
    revokeRole(UPDATER_ROLE, _feeder);
    emit FeederRemoved(_feeder);
}
```
## Description
There is no need to call the onlyWhenFeederNotExisted modifier twice to judge whether the feeder exists which will waste gas.
## Advice
It is recommended to remove the modifier onlyWhenFeederExisted of function _removeFeeder.