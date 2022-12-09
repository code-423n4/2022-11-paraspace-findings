
# \[LOW\] NFTFloorOracle#initialize should check ```_admin != address(0)```
Otherwise, contract would need to be redeployed

# \[Non-critical\] NFTFloorOracle#getPrice return value price is never assigned
The function return a value, but not in the indicated variable. This variable should be removed.

# \[Non-critical\] ParaSpaceOracle#constructor should check that parameters are not equal to address 0
This in order to avoid doing a mistaken deploy which would obligate a redeployment (giving that assignation is done to immutable variables). For this reason, the cosntructor should check that:
* ```provider!= address(0)```
* ```baseCurrency != address(0)```

# \[Non-critical\] ParaSpaceOracle#_setFallbackOracle should check fallbackOracle != _fallbackOracle to avoid emitting meaningless event
If ```_fallbackOracle == fallbackOracle``` then a meaningless **FallbackOracleUpdated** will be emitted. change current function to:
```diff
    function _setFallbackOracle(address fallbackOracle) internal {
+       if(address(_fallbackOracle) != fallbackOracle){
            _fallbackOracle = IPriceOracleGetter(fallbackOracle);
            emit FallbackOracleUpdated(fallbackOracle);
+       }
    
    }
```

# \[Non-critical\] UniswapV3OracleWrapper#constructor should check that parameters are not equal to address 0
This in order to avoid doing a mistaken deploy which would obligate a redeployment (giving that assignation is done to immutable variables). For this reason, the cosntructor should check that:
* ```_factory!= address(0)```
* ```_manager != address(0)```
* ```_addressProvider != address(0)```

# \[Non-critical\] PoolAddressesProvider#constructor owner parameter shadows owner state variable
It would be recommendable to change *owner* parameter name to *_owner* to avoid confusion due to shadowing.


# \[Non-critical\] PoolAddressesProvider#setPause can emit meaningless event
Giving this method does not check if ```assetFeederMap[_asset].paused``` is true, the method can emit meaningles event. It should be changed to:
```solidity
function setPause(address _asset, bool _flag)
    external
    onlyRole(DEFAULT_ADMIN_ROLE)
{
    bool _paused = assetFeederMap[_asset].paused;
    if (_paused != flag){
        assetFeederMap[_asset].paused = _flag;
        emit AssetPaused(_asset, _flag);
    }else{
        revert(ERROR_CANNOT_CHANGE_PAUSE_FLAG());
    } 
}
```

# \[Non-Critical\] Spelling mistake in code comments
[This comment](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34) should replace **tsatr** for **start**
