## Unnecessay safeTransferFrom with WETH

Since we know the implementation of WETH, the extra check with SafeERC20 is not required.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L584-L588

```solidity
            IERC20(vars.weth).safeTransferFrom(
                address(this),
                msg.sender,
                vars.ethLeft
            );
```

## Spliting && require to save gas

e.g.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L71-L77

```solidity
        require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&
                isBasicOrder(advancedOrders[0]) &&
                isBasicOrder(advancedOrders[1]),
            Errors.INVALID_MARKETPLACE_ORDER
        );
```

to

```solidity
        // NOT criteria based and must be basic order
        require(advancedOrders.length == 2, Errors.INVALID_MARKETPLACE_ORDER);
        require(isBasicOrder(advancedOrders[0]), Errors.INVALID_MARKETPLACE_ORDER);
        require(isBasicOrder(advancedOrders[1]), Errors.INVALID_MARKETPLACE_ORDER);
```

## Use calldata instead of memory

Use calldata instead of memory for function parameters saves gas if the function argument is only read.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/interfaces/IMarketplace.sol#L16

```solidity
    function getAskOrderInfo(bytes memory data, address WETH)
```

to

```solidity
    function getAskOrderInfo(bytes calldata data, address WETH)
```

and also changing the adapter implementations

## Update Solidity Version

The project is mostly using 0.8.10 but the latest version is 0.8.17