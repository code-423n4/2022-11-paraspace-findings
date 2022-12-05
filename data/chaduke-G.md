G1: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L97
Since one can execute the line `` DataTypes.PoolStorage storage ps = poolStorage();`` in a library function, there is no need to execute it in the ``supply`` function and then pass members of the struct as arguments to the ``SupplyLogic.executeSupply`` function. These member variables of the struct can just as well be retrieved inside the ``SupplyLogic.executeSupply``., which can save gas as a result of eliminating some arguments for the function ``SupplyLogic.executeSupply``. This gas optimization can be used for other functions as well, including ``supplyERC721()``, ``supplyERC721FromNToken()``, etc. 

G2. https://github.com/para-space/paraspace-core/blob/d3a14348c91d0869f97bc438659fa65b6fd79a01/contracts/protocol/pool/PoolParameters.sol#L174
Cache ps._reserves[asset] as it is accessed twice in the function

G3. https://github.com/para-space/paraspace-core/blob/d3a14348c91d0869f97bc438659fa65b6fd79a01/contracts/protocol/pool/PoolApeStaking.sol#L80
Change this line to the following to avoid a SLOAD since ``xTokenAddress `` has already been cached in line 89.
```
 INTokenApeStaking(xTokenAddress).withdrawApeCoin(
            _nfts,
            msg.sender
        );
```

G4. https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L25
Change ``bytes memory parameter`` to ``bytes calldata paramter``. This will save gas when the function is called by a write function. 

G5: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L31
change ``bytes memory params`` to ``bytes calldata params``.

G6: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L403
There is no need to cache ``block.number``, which only introduces more gas.

G7. https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L35
Replacing line 35 by the implementation of ``_onlyAssetListingOrPoolAdmins()`` can save gas. There is 
no need to have another level of function call here. 

G8: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L342-L354
It can be simplified by eliminating all the unnecessary variables as follows:
```
function _getProxyImplementation(bytes32 id) internal returns (address) {
            return
                InitializableImmutableAdminUpgradeabilityProxy(
                    payable(_addresses[id])
                ).implementation();
}
```
G9: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L313
chanage the line to the following to avoid unnecessary type conversion. 
```
proxy = IParaProxy(new ParaProxy(address(this)));
```
G8. https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16

G9: https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16
The digest can be precomputed so that the constant evaluation will be more gas-saving. 

G10. https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23
The digest can be precomputed so that the constant evaluation will be more gas-saving. 


