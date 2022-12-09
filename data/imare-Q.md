### [L-01] missing cast to uint40 in getNormalizedIncome and getNormalizedDebt

because of this line
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol#L304

The next two lines to work correctly need to fist cast the block.timestamp like this ``uint40(block.timestamp)``

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol#L48-L51

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol#L77-L80

### [L-02] PToken permit needs to approve 0 first for some token to work correctly

A token like this is USDT

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/PToken.sol#L243-L245

### [L-03] no need to call onlyWhenFeederExisted twice in a row when removing feeder address

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167-L172

### [L-04] The function call ``executeTransferCollateralizable`` for ``MintableIncentivizedERC721`` contract does not transfer collateral

The called function does remove the from address from ``isUsedAsCollateral`` mapping but it dosnt move/set it to the ``to`` address.

```solidity
    if (from != to && isUsedAsCollateral_) {
        erc721Data.userState[from].collateralizedBalance -= 1; // TODO: M? where is  erc721Data.userState[to].collateralizedBalance =+1
        delete erc721Data.isUsedAsCollateral[tokenId];
    }
```

### [L-05] ``setGracePeriod`` call has no validatio for ``newGracePerion`` leading to a possible temporary DOS of ``PriceOracleSentinel``

if ``_gracePerion`` state variable is set too low

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol#L100-L106

The check in the next function will always return false

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol#L83-L88

If it happens then ``isBorrowAllowed()`` and ``isLiquidationAllowed()`` both will also always return false. Rendering the borrow/liquidate service unavailable.

### [L-06] ``maxPriceDeviation`` and ``expirationDate`` are not validated in NFTFloorOracle`` contract can lead to DOSING of NFT oracle

Both variable are missing validation if they are **set too low** and in such case they will return false on the following lines:

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L370-L372

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L425-L427

Rendering the NFT oracle unavailable. ``getPrice`` will revert with "NFTOracle: asset price expired"

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L236-L248

### [L-07] centralization risks

The owner address of ``PoolAddressesProvider`` contract has control over many dangerous functions. Such as:

* ``setAddressAsProxy``, ``setPoolConfiguratorImpl`` and ``updatePoolImpl`` which are responsable for setting the pool diamon proxy implementation

* ``setPriceOracle`` ``setPriceOracleSentinel``: for settings the used oracle for the protocol prices

* ``setACLAdmin``: for setting the access control administration

All mentioned functions should have a time lock for the users to have more confidence in the protocol.