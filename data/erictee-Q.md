### [L-01] ```approve()``` and ```safeApprove()``` should be replaced with ```safeIncreaseAllowance()``` / ```safeDecreaseAllowance()```


#### Impact
```approve()``` & ```safeApprove()``` are deprecated and subject to a known front-running attack. Consider using  ```safeIncreaseAllowance()``` & ```safeDecreaseAllowance()``` instead.


#### Findings:
```
contracts/protocol/libraries/logic/MarketplaceLogic.sol:L555                     IERC20(token).safeApprove(operator, type(uint256).max);

contracts/protocol/tokenization/NTokenApeStaking.sol:L45                 _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);

contracts/protocol/tokenization/NTokenApeStaking.sol:L46                 _apeCoin.approve(address(POOL), type(uint256).max);

```

### [L-02] ```decimals()``` not part of ERC20 standard.


#### Impact
```decimals()``` is not part of the official ERC20 standard and might fall for tokens that do not implement it. While in practice it is very unlikely, as usually most of the tokens implement it, this should still be considered as a potential issue.


#### Findings:
```
contracts/misc/UniswapV3OracleWrapper.sol:L234            .decimals();

contracts/misc/UniswapV3OracleWrapper.sol:L236            .decimals();

```

### [L-03] Use of deprecated functions


#### Impact
The contract uses deprecated function ```latestAnswer()```. Such functions might suddenly stop working if no longer supported.
Impact: Deprecated API stops working. Prices cannot be obtained. Protocol stops and contracts have to be redeployed.


#### Findings:
```
contracts/misc/ParaSpaceOracle.sol:L128            price = uint256(source.latestAnswer());

```

### [N-01] Open TODOs


#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.


#### Findings:
```
contracts/misc/UniswapV3OracleWrapper.sol:L238        // TODO using bit shifting for the 2^96

contracts/misc/marketplaces/LooksRareAdapter.sol:L59            makerAsk.price, // TODO: take minPercentageToAsk into account

contracts/protocol/libraries/logic/MarketplaceLogic.sol:L442        // TODO: support PToken

```



