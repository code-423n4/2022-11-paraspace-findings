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