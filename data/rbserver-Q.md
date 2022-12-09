## [L-01] CHAINLINK AGGREGATOR IS NOT SUFFICIENTLY VALIDATED AND CAN RETURN STALE ANSWER
As shown below, calling the `getAssetPrice` function in the `ParaSpaceOracle` contract can execute `price = uint256(source.latestAnswer())`, where `source` is a Chainlink aggregator. If the answer returned by the Chainlink aggregator is 0, a fallback oracle can then be used for setting `price`. However, besides that Chainlink's `latestAnswer` function is deprecated, only verifying if the returned answer is positive is also not enough to guarantee that the returned answer is not stale. Using a stale answer as `price` can have unexpected consequences, such as mistakenly considering a user position unhealthy that allows liquidation.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L115-L136
```solidity
    function getAssetPrice(address asset)
        public
        view
        override
        returns (uint256)
    {
        if (asset == BASE_CURRENCY) {
            return BASE_CURRENCY_UNIT;
        }

        uint256 price = 0;
        IEACAggregatorProxy source = IEACAggregatorProxy(assetsSources[asset]);
        if (address(source) != address(0)) {
            price = uint256(source.latestAnswer());
        }
        if (price == 0 && address(_fallbackOracle) != address(0)) {
            price = _fallbackOracle.getAssetPrice(asset);
        }

        require(price != 0, Errors.ORACLE_PRICE_NOT_READY);
        return price;
    }
```

To avoid using a stale answer returned by the Chainlink aggregator, according to [Chainlink's documentation](https://docs.chain.link/docs/historical-price-data):
1. The `latestRoundData` function can be used instead of the deprecated `latestAnswer` function.
2. `roundId` and `answeredInRound` are also returned. "You can check `answeredInRound` against the current `roundId`. If `answeredInRound` is less than `roundId`, the answer is being carried over. If `answeredInRound` is equal to `roundId`, then the answer is fresh."
3. "A read can revert if the caller is requesting the details of a round that was invalid or has not yet been answered. If you are deriving a round ID without having observed it before, the round might not be complete. To check the round, validate that the timestamp on that round is not 0."

Hence, as a mitigation, https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L127-L129 can be updated to the following code.
```solidity
        if (address(source) != address(0)) {
            (uint80 roundId, int256 answer, , uint256 updatedAt, uint80 answeredInRound) = source.latestRoundData();
            require(answeredInRound >= roundId, "answer is stale");
            require(updatedAt > 0, "round is incomplete");
            require(answer > 0, "Invalid answer");

            price = uint256(answer);            
        }
```

## [L-02] UNSAFE `decimals()` CALL
The following `_getOracleData` function in the `UniswapV3OracleWrapper` contract calls `decimals()` to get the decimals of `positionData.token0` and `positionData.token1`. However, `decimals()` is optional according to https://eips.ethereum.org/EIPS/eip-20. When `positionData.token0` or `positionData.token1` does not implement `decimals()`, calling this `_getOracleData` function reverts. As a mitigation, helper functions like BoringCrypto's [`safeDecimals`](https://github.com/boringcrypto/BoringSolidity/blob/ccb743d4c3363ca37491b87c6c9b24b1f5fa25dc/contracts/libraries/BoringERC20.sol#L52-L55) can be used so such tokens can be accommodated.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L221-L280
```solidity
    function _getOracleData(UinswapV3PositionData memory positionData)
        internal
        view
        returns (PairOracleData memory)
    {
        ...

        oracleData.token0Decimal = IERC20Detailed(positionData.token0)
            .decimals();
        oracleData.token1Decimal = IERC20Detailed(positionData.token1)
            .decimals();

        ...
    }
```

## [L-03] INACCURATE `require` REASON
Inaccurate reasons for `require` statements can be misleading. The following `require` statement's condition is `vars.healthFactor > HEALTH_FACTOR_LIQUIDATION_THRESHOLD` but the reason misses the case where `vars.healthFactor` equals `HEALTH_FACTOR_LIQUIDATION_THRESHOLD`. Please consider using a reason that is more accurate and descriptive for this `require` statement.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L341-L344
```solidity
        require(
            vars.healthFactor > HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
            Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
        );
```

## [L-04] UNRESOLVED TODO COMMENTS
Comments regarding todos indicate that there are unresolved action items for implementation, which need to be addressed before the protocol deployment. Please review the todo comments in the following code.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238-L239
```solidity
        // TODO using bit shifting for the 2^96
        // positionData.sqrtPriceX96;
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L55-L62
```solidity
        consideration[0] = ConsiderationItem(
            itemType,
            token,
            0,
            makerAsk.price, // TODO: take minPercentageToAsk into account
            makerAsk.price,
            payable(takerBid.taker)
        );
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442-L443
```solidity
        // TODO: support PToken
        IPToken(vars.xTokenAddress).transferUnderlyingTo(to, vars.creditAmount);
```

## [N-01] MISSING EVENT EMISSION FOR `SupplyLogic.executeDecreaseUniswapV3Liquidity` FUNCTION
The `SupplyLogic` library's `executeDecreaseUniswapV3Liquidity` function (https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L396-L449) does not emit any events by itself, which is unlike the `SupplyLogic.executeWithdraw` and `SupplyLogic.executeWithdrawERC721` functions. Emitting events after important state changes enhances off-chain monitoring. Please consider emitting relevant events for this `SupplyLogic.executeDecreaseUniswapV3Liquidity` function.

## [N-02] CONSTANTS CAN BE USED INSTEAD OF MAGIC NUMBERS
To improve readability and maintainability, constants can be used instead of magic numbers. Please consider replacing the following magic numbers, such as `30`, with constants. Please also note that the following magic numbers are not found in https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba#NC-4.

```solidity
contracts\misc\UniswapV3OracleWrapper.sol
  245: ((oracleData.token0Price * (10**18)) /  
  247: ) * 2**96) / 1E9  
  259: ) * 2**96) / 1E9  
  271: ) * 2**96) /  
  273: (9 +  

contracts\protocol\tokenization\NTokenApeStaking.sol
  133: ApeStakingLogic.executeSetUnstakeApeIncentive(dataStorage, 30); 
```

## [N-03] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. NatSpec comments are missing for the following functions. Please consider adding them.

```solidity
contracts\misc\UniswapV3OracleWrapper.sol
  221: function _getOracleData(UinswapV3PositionData memory positionData)  
  282: function _getPendingFeeAmounts(UinswapV3PositionData memory positionData)   

contracts\protocol\libraries\logic\GenericLogic.sol
  399: function getLtvAndLTForUniswapV3(   
  450: function _getUserBalanceForUniswapV3(   
  535: function _getAssetPrice(address oracle, address currentReserveAddress)  
  543: function _getTokenPrice(    

contracts\protocol\libraries\logic\SupplyLogic.sol
  134: function executeSupplyERC721Base(   
  335: function executeWithdrawERC721( 
  396: function executeDecreaseUniswapV3Liquidity( 

contracts\protocol\libraries\logic\ValidationLogic.sol
  189: function validateWithdrawERC721(    
  1155: function validateForUniswapV3(  

contracts\protocol\pool\PoolCore.sol
  56: function getRevision() internal pure virtual override returns (uint256) {   
  237: function decreaseUniswapV3Liquidity(    

contracts\protocol\pool\PoolStorage.sol
  19: function poolStorage()  

contracts\protocol\tokenization\NToken.sol
  95: function _burn( 
  323: function getAtomicPricingConfig() external view returns (bool) {    

contracts\protocol\tokenization\NTokenApeStaking.sol
  36: function initialize(    
  125: function POOL_ID() internal pure virtual returns (uint256) {  
  130: function initializeStakingData() internal {  
  136: function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {  
  143: function apeStakingDataStorage()  

contracts\protocol\tokenization\NTokenMoonBirds.sol
  36: function getXTokenType() external pure override returns (XTokenType) {  
  40: function burn(  
  63: function onERC721Received(  

contracts\protocol\tokenization\NTokenUniswapV3.sol
  144: function _safeTransferETH(address to, uint256 value) internal { 

contracts\protocol\tokenization\base\MintableIncentivizedERC721.sol
  364: function _mintMultiple( 
  384: function _burnMultiple(address user, uint256[] calldata tokenIds) 

contracts\protocol\tokenization\libraries\MintableERC721Logic.sol
  187: function executeMintMultiple(   
  348: function _approve(  
  402: function _checkBalanceLimit(    
  416: function _exists(MintableERC721Data storage erc721Data, uint256 tokenId)    
  424: function isAuctioned(   

contracts\ui\WPunkGateway.sol
  57: function initialize() external initializer {  
```

## [N-04] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. The `@param` and/or `@return` comments are missing for the following functions. Please consider completing the NatSpec comments for them.

```solidity
contracts\misc\UniswapV3OracleWrapper.sol
  51: function getOnchainPositionData(uint256 tokenId)    
  144: function getLpFeeAmountFromPositionData(    
  156: function getTokenPrice(uint256 tokenId) public view returns (uint256) { 

contracts\protocol\libraries\logic\GenericLogic.sol
  74: function calculateUserAccountData(  
  362: function _getUserBalanceForERC721(  
  512: function _getUserBalanceForERC20(   

contracts\protocol\libraries\logic\ValidationLogic.sol
  63: function validateSupply(    

contracts\protocol\tokenization\NTokenApeStaking.sol
  64: function getBAKC() public view returns (IERC721) {  
  71: function getApeStaking() external view returns (ApeCoinStaking) {  
  78: function _transfer(  
  102: function burn(  
  182: function getUserApeStakingAmount(address user)  

contracts\protocol\tokenization\NTokenMoonBirds.sol
  96: function toggleNesting(uint256[] calldata tokenIds) external {  
  111: function nestingPeriod(uint256 tokenId)  
  127: function nestingOpen() external returns (bool) {  

contracts\protocol\tokenization\NTokenUniswapV3.sol
  51: function _decreaseLiquidity(    

contracts\protocol\tokenization\libraries\MintableERC721Logic.sol
  483: function _addTokenToOwnerEnumeration(   
  497: function _addTokenToAllTokensEnumeration(   
  514: function _removeTokenFromOwnerEnumeration(  
  544: function _removeTokenFromAllTokensEnumeration(  

contracts\ui\WPunkGateway.sol
  129: function acceptBidWithCredit(  
  167: function batchAcceptBidWithCredit(  
  228: function getWPunkAddress() external view returns (address) {  
```