## 1. Redundant Conditional Check

File: `NFTFloorOracle.sol` [Line 331](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L331)

```
        if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
```

Because [`feederIndex`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L330) is type `uint8`, it can never go below zero. As such, the check `feederIndex >= 0` is redundant

Instead, you can refactor to the following:

```
        if (feeders[feederIndex] == _feeder) {
```

## 2. Typo mistakes

File: `AuctionLogic.sol` [Line 34](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34)

tsatr -> start

```
     * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.
```

File: `ReserveLogic.sol` [Line 163](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol#L163), [Line 261](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol#L261)

Duplicated "reserve reserve"

```
     * @param reserve The reserve reserve to be updated
```

## 3. Use delete to clear variables instead of zero assignment

A better way to indicate that you are clearing a variable is to use the `delete` keyword.

For example:

File: `MarketplaceLogic.sol` [Line 409 - 410](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L409-L410)

```
            price = 0;
            downpayment = 0;
```

The above could be refactored to:

```
            delete price;
            delete downpayment;
```

## 4. Events are missing old value

Consider having events that update state variables emit both the old and new values.

Here are some instances of this issue:

File `ParaSpaceOracle.sol` [Line 62](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L62)

File: `ParaSpaceOracle.sol` [Line 111](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L111)

## 5. Missing Zero address/value checks

Zero address/value checks should be implemented for security. However, such checks are missing in many contracts.

Here are some instances:

File: `ParaSpaceOracle.sol` [Line 49-63](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L49-L63)

File: `UniswapV3OracleWrapper.sol` [Line 24-26](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L24-L26)

File: `DefaultReserveAuctionStrategy.sol` [Line 50-64](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L50-L64)

## 6. Avoid the use of `_setupRole()` as it is deprecated

As said in the [OpenZeppelin's docs](https://docs.openzeppelin.com/contracts/4.x/api/access#AccessControl-_setupRole-bytes32-address-), "This function is deprecated in favor of [`_grantRole`](https://docs.openzeppelin.com/contracts/4.x/api/access#AccessControl-_grantRole-bytes32-address-)."

There are 3 instances of this issue:

File: NFTFloorOracle.sol ([Line 104](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L104), [Line 106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L106), [Line 314](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L314))

```
104:         _setupRole(DEFAULT_ADMIN_ROLE, _admin);
...
106:        _setupRole(UPDATER_ROLE, _admin);
...
314:        _setupRole(UPDATER_ROLE, _feeder);
```

Consider replacing `_setupRole()` with `_grantRole()`:

```
104:         _grantRole(DEFAULT_ADMIN_ROLE, _admin);
...
106:        _grantRole(UPDATER_ROLE, _admin);
...
314:        _grantRole(UPDATER_ROLE, _feeder);
```

## 7. Unresolved TODOs

Consider resolving open TODOs before deployment.

Here are some examples of this issue:

File: `UniswapV3OracleWrapper.sol` [Line 238](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238)

File: `MarketplaceLogic.sol` [Line 442](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442)

File: `LooksRareAdapter.sol` [Line 59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59)

## 8. Limit line length

To comply with the [Solidity Style Guide](https://docs.soliditylang.org/en/develop/style-guide.html#maximum-line-length), lines should be limited to 120 characters max.

For example, the instance below is **243** characters past the recommended limit

File: `LiquidationLogic.sol` [Line 603](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603)
