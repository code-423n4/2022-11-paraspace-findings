# 1. comment error
The comment of `NFTFloorOracle.setMultiplePrices`, `/// @notice Allows owner ` should be `Allows updater` according to the role checker modifier:
```
    function setMultiplePrices(
        address[] calldata _assets,
        uint256[] calldata _twaps
    ) external onlyRole(UPDATER_ROLE) {
```