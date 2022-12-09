
## 01 SAFEAPPROVE() IS DEPRECATED

Deprecated in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead

_There is 1 instance of this issue:_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
File: contracts/protocol/libraries/logic/MarketplaceLogic.sol

555: IERC20(token).safeApprove(operator, type(uint256).max);//@
```

--------------

## 02 RACE CONDITION IN APPROVE()

_There are 2 instances of this issue:_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```
File: contracts/protocol/tokenization/NTokenApeStaking.sol

45: _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
46: _apeCoin.approve(address(POOL), type(uint256).max);
```

**Recommendation:**

Add increaseAllowance and decreaseAllowance methods in ERC20 contract

-------
## 03 TYPO - UINSWAP SHOULD BE UNISWAP

_There are 10 instances of this issue:_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
File: contracts/misc/UniswapV3OracleWrapper.sol

15: import {UinswapV3PositionData} from "../interfaces/IUniswapV3PositionInfoProvider.sol";
54: returns (UinswapV3PositionData memory)
77: UinswapV3PositionData({
101: UinswapV3PositionData memory positionData = getOnchainPositionData(
114: UinswapV3PositionData memory positionData
132: UinswapV3PositionData memory positionData = getOnchainPositionData(
145: UinswapV3PositionData memory positionData
157: UinswapV3PositionData memory positionData = getOnchainPositionData(
221: function _getOracleData(UinswapV3PositionData memory positionData)
282: function _getPendingFeeAmounts(UinswapV3PositionData memory positionData)
```

-----
## 04 OPEN TODOS

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

_There are 3 instances of this issue:_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```
File: contracts/misc/UniswapV3OracleWrapper.sol

238: // TODO using bit shifting for the 2^96
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

```
File: contracts/misc/UniswapV3OracleWrapper.sol

59: makerAsk.price, // TODO: take minPercentageToAsk into account
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```
File: contracts/protocol/libraries/logic/MarketplaceLogic.sol

442: // TODO: support PToken
```

----------

## 05 TRANSFEROWNERSHIP SHOULD BE TWO STEP

The owner is the authorized user in the solidity contracts. Usually, an owner can be updated with transferOwnership function. However, the process is only completed with single transaction. If the address is updated incorrectly, an owner functionality will be lost forever.

_There is 1 instance of this issue:_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

```
File: contracts/protocol/configuration/PoolAddressesProvider.sol

47: transferOwnership(owner);
```

----------

## 06 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

_There are 2 instances of this issue_

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```
File: contracts/misc/NFTFloorOracle.sol

236-240: function getPrice(address _asset)
        external
        view
        override
        returns (uint256 price)
252-256: function getLastUpdateTime(address _asset)
        external
        view
        override
        returns (uint256 timestamp)
```

-------

## 07 USE A MORE RECENT VERSION OF SOLIDITY

It's a best practice to use the latest compiler version.

The specified minimum compiler version is quite old. Older compilers might be susceptible to some bugs. We recommend changing the solidity version pragma to the latest version to enforce the use of an up to date compiler.

List of known compiler bugs and their severity can be found here: [https://etherscan.io/solcbuginfo](https://etherscan.io/solcbuginfo)

_This issue exists in the following In-scope contracts_

-----------