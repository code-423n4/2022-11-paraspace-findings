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