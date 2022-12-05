## [Low-01]  ValidationLogic.validateLiquidateERC721: msg.value should be greater than actualLiquidationAmount not maxLiquidationAmount

### Impact

In validateLiquidateERC721, maxLiquidationAmount is the maximum amount of expenditure entered by the user, and actualLiquidationAmount is the maximum liquidation amount that the user can liquidate. When the asset is ETH, msg.value should be greater than actualLiquidationAmount, which is consistent with validateLiquidateERC20
```
         require(
             msg.value == 0 || msg.value >= params.actualLiquidationAmount,
             Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
         );

```

### Proof of Concept

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L672-L676

### Recommended Mitigation Steps
 
```diff
        require(
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&
-               (msg.value == 0 || msg.value >= params.maxLiquidationAmount),
+               (msg.value == 0 || msg.value >= params.actualLiquidationAmount),
            Errors.LIQUIDATION_AMOUNT_NOT_ENOUGH
        );
```


## [Low-02] Implementation does not match documentation

### Impact

https://parallelfinance.notion.site/Audit-Technical-Documentation-0a107270dabe45d2b66a076e0bdaa943
The docs say 
```
During ERC721 liquidation, we allow the liquidationAsset to be any ERC20 (not only the debtAsset of borrower), if the liquidation asset is not debt asset then basically there is no liquidation bonus. 
```
But in fact, only eth/weth is allowed to liquidate ERC721, and there is no cancellation of liquidation rewards by comparing liquidation assets and debt assets

### Proof of Concept
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L477-L478

### Recommended Mitigation Steps
Change the documentation description, or change the implementation

## [Low-03] NTokenMoonBirds may not be able to receive airdrops

### Impact
For most NToken, some airdrops that are actively minted to the holder's address can be withdrawn and later distributed by the PoolAdmin calling the rescueERC721 function.
```solidity
    function rescueERC721(
        address token,
        address to,
        uint256[] calldata ids
    ) external override onlyPoolAdmin {
        require(
            token != _underlyingAsset,
            Errors.UNDERLYING_ASSET_CAN_NOT_BE_TRANSFERRED
        );
        for (uint256 i = 0; i < ids.length; i++) {
            IERC721(token).safeTransferFrom(address(this), to, ids[i]);
        }
        emit RescueERC721(token, to, ids);
    }
```
However, in the onERC721Received function of the NTokenMoonBirds contract, due to the requirement that the sender can only be the MoonBird contract, when safemint()/safetransferfrom() is called to send the airdrop NFTs to the NTokenMoonBirds contract, the transaction will fail, thus preventing NTokenMoonBirds from receiving these airdrops.
```solidity
    function onERC721Received(
        address operator,
        address from,
        uint256 id,
        bytes memory
    ) external virtual override returns (bytes4) {
        // only accept MoonBird tokens
        require(msg.sender == _underlyingAsset, Errors.OPERATION_NOT_SUPPORTED);
```

For example, Moonbirds Oddities are actively minted to the holder's address.
https://etherscan.io/tx/0x3af5de8b6a8c55aac033d57e1b110e8340abf4dcd289ebda889a44f9f9dc613d


### Proof of Concept
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L136-L149
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L63-L77

### Recommended Mitigation Steps
Consider allowing the NTokenMoonBirds contract to receive NFTs from other addresses and only call POOL.supportERC721FromNToken when msg.sender == _underlyingAsset
```diff
    function onERC721Received(
        address operator,
        address from,
        uint256 id,
        bytes memory
    ) external virtual override returns (bytes4) {
        // only accept MoonBird tokens
-        require(msg.sender == _underlyingAsset, Errors.OPERATION_NOT_SUPPORTED);

        // if the operator is the pool, this means that the pool is transferring the token to this contract
        // which can happen during a normal supplyERC721 pool tx
        if (operator == address(POOL)) {
            return this.onERC721Received.selector;
        }
+     if(msg.sender == _underlyingAsset){       
        // supply the received token to the pool and set it as collateral
        DataTypes.ERC721SupplyParams[]
            memory tokenData = new DataTypes.ERC721SupplyParams[](1);

        tokenData[0] = DataTypes.ERC721SupplyParams({
            tokenId: id,
            useAsCollateral: true
        });

        POOL.supplyERC721FromNToken(_underlyingAsset, tokenData, from);
+ }
        return this.onERC721Received.selector;
    }
```
