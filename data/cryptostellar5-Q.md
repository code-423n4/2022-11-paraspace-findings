
### 01 `safeIncreaseAllowance` AND `safeDecreaseAllowance` INSTEAD OF `safeApprove` OR  `approve`

*Number of Instances Identified: 3*

-   `safeApprove()` has been deprecated in favour of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`
-   using `approve()` might fail because some tokens (eg. USDT) don’t work when changing the allowance from an existing non-zero allowance value

### Recommended Mitigation Steps

Update instances of `approve()` and `safeApprove()` to `safeIncreaseAllowance()`.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
555:  IERC20(token).safeApprove(operator, type(uint256).max);
```


https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```
45: _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
46: _apeCoin.approve(address(POOL), type(uint256).max);
```


### 02 EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

*Number of Instances Identified: 1*

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
70:  bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
```


### 03 UPGRADEABLE CONTRACT IS MISSING A __GAP50 STORAGE VARIABLE TO ALLOW FOR NEW STORAGE VARIABLES IN LATER VERSIONS

*Number of Instances Identified: 1*

See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol

```
19-23: contract WPunkGateway is
    ReentrancyGuard,
    IWPunkGateway,
    IERC721Receiver,
    OwnableUpgradeable
```


### 04 ADDING A RETURN STATEMENT WHEN THE FUNCTION DEFINES A NAMED RETURN VARIABLE, IS REDUNDANT

*Number of Instances Identified: 2*

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
240: returns (uint256 price)
256: returns (uint256 timestamp)
```


### 05 TYPOS

*Number of Instances Identified: 10*

Uniswap should be written in place of uinswap

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
53: returns (UinswapV3PositionData memory)
77: UinswapV3PositionData
101: UinswapV3PositionData memory positionData = getOnchainPositionData
114: UinswapV3PositionData memory positionData
132: UinswapV3PositionData memory positionData = getOnchainPositionData
145: UinswapV3PositionData memory positionData
157: UinswapV3PositionData memory positionData = getOnchainPositionData
221: function _getOracleData(UinswapV3PositionData memory positionData)
282: function _getPendingFeeAmounts(UinswapV3PositionData memory positionData)
```


https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol

start should be written in place of tsatr

```
34: @notice Function to tsatr auction
```


### 06 OPEN TODOS

*Number of Instances Identified: 3*

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
238: // TODO using bit shifting for the 2^96
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

```
59: // TODO: take minPercentageToAsk into account
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
442: // TODO: support PToken
```


### 07 USE LATEST SOLIDITY VERSION

All the inscope contracts are using solidity version 0.8.10, should be upgraded to the latest version - 0.8.17

```
pragma solidity 0.8.10
```
