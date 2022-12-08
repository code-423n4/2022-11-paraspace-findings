
#### 1. Tautology
`Uint >= 0` check is always true, so could be deleted to save Gas.

**Path:** ./NFTFloorOracle.sol : _removeFeeder()

#### 2. Redundant `dataValidity` initialization for admin
`dataValidity` variable is created at the start of the function, but it is actionable only for non admin `msg.senger`. By moving variable initialization after block:
```
if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) {
     _finalizePrice(_asset, _twap);
     return;
}
```
Gas for admin interaction with function would be reduced.

**Path:** ./NFTFloorOracle.sol : setPrice()