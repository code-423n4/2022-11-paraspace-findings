## [NAZ-G1] Unneeded Modifier
**Context**: [`NFTFloorOracle.sol#L169`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L169)

**Description**:
Currently the same modifier is applied to both the public and internal functions costing more gas then intended.

**Recommendation**: 
Consider removing the extra modifier.