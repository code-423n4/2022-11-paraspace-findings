# --> 1st report for : NFTFloorOracle.sol https://github.com/para-space/paraspace-core/blob/main/contracts/misc/NFTFloorOracle.sol 

## 1. The `MAX_DEVIATION_RATE` constant in the NFTFloorOracle contract is set to 150, but it should be 1.5. This causes the `MAX_DEVIATION_RATE` to be calculated as a much larger value than intended, which could cause issues in the operation of the contract.

Steps to Reproduce:

Deploy the `NFTFloorOracle` contract.
Check the value of the `MAX_DEVIATION_RATE` constant.
Notice that the value is 150, instead of the expected 1.5.

Expected Result:

The `MAX_DEVIATION_RATE` constant should be set to 1.5, as intended.

Actual Result:

The `MAX_DEVIATION_RATE` constant is set to 150, which is a much larger value than intended.

Suggested Fix:

Change the `MAX_DEVIATION_RATE` constant from 150 to 1.5. This will ensure that the `MAX_DEVIATION_RATE` is calculated as intended, and will prevent potential issues in the operation of the contract.

## 2. Incorrect calculation of maximum price deviation

Summary: The `NFTFloorOracle` contract does not correctly calculate the maximum allowed deviation between two consecutive prices for a given asset. Instead of dividing the maximum allowed deviation by 100, it divides it by 1000.

Impact: This bug could cause the contract to reject valid prices that are within the allowed deviation range, leading to incorrect prices being reported on-chain.

Recommendation: The calculation of the maximum allowed price deviation should be corrected by dividing the maximum deviation by 100 instead of 1000. This can be done by replacing the line `config.maxPriceDeviation / 1000` with `config.maxPriceDeviation / 100` in the `_setAssetPrice` function.

## 3. Aggregated price not updated properly

Summary: The `_updateAggregatedPrice` function in the `NFTFloorOracle` contract is not properly updating the aggregated price of an NFT asset. The `feederPrice` array that is used to calculate the average price is not properly resetting, which causes the average price calculation to be incorrect.

Impact: The incorrect aggregated price could be read by external contracts, leading to incorrect calculations and potentially incorrect decisions being made.

Recommendation: The `_updateAggregatedPrice` function should reset the `feederPrice` array by setting its length to 0 before iterating over the registered feeders and adding their prices to the array. This will ensure that the average price calculation is correct and that the correct aggregated price is stored.

## 4. OracleConfigSet event not emitted on config change

Summary: The `OracleConfigSet` event is not being emitted when the `_setConfig` function is called to change the oracle configuration. This means that any listeners for this event will not be notified of the change, and will not be able to update their own state accordingly.

Impact: Listeners that rely on the `OracleConfigSet` event for updating their own state will not be able to do so, potentially leading to incorrect or inconsistent behavior.

Recommendation: The `OracleConfigSet` event should be emitted within the `_setConfig` function whenever the oracle configuration is changed. This will ensure that all listeners are notified of the change and can update their own state accordingly.

## 5. Potential for incorrect asset price calculation

Summary: The `NFTFloorOracle` contract does not properly handle the case where the number of valid/unexpired prices for a given asset is less than `MIN_ORACLES_NUM`. In this case, the contract uses the previous price for the asset instead of properly calculating the aggregate price.

Impact: If the number of valid/unexpired prices for a given asset is less than `MIN_ORACLES_NUM`, the contract may incorrectly calculate the price for that asset.

Recommendation: Update the `calculatePrice` function to properly handle the case where the number of valid/unexpired prices is less than `MIN_ORACLES_NUM` by using a different method of calculation or by properly handling the error.

## 6. Call to uninitialized contract function

Summary: In the `initialize` function, the `_addAssets` and `_addFeeders` functions are called without being initialized.

Impact: The contract will fail to initialize properly, which could result in errors when using other contract functions.

Recommendation: Initialize the `_addAssets` and `_addFeeders` functions before calling them in the `initialize` function. This can be done by declaring them as `private` functions and adding the `internal` visibility modifier to their definitions.

## 7. Incorrect Initialization of Oracle Configurations

Summary: The `initialize` function incorrectly sets the `config.expirationPeriod` and `config.maxPriceDeviation` values to the constants `EXPIRATION_PERIOD` and `MAX_DEVIATION_RATE` instead of the passed in `_expirationPeriod` and `_maxPriceDeviation` arguments.

Impact: As a result, the `config.expirationPeriod` and `config.maxPriceDeviation` values will always be set to the default values of `EXPIRATION_PERIOD` and `MAX_DEVIATION_RATE` instead of the values specified by the contract creator during initialization. This could result in incorrect price aggregation and deviation checks.

Recommendation: To fix this issue, the `config.expirationPeriod` and `config.maxPriceDeviation` values should be set to the passed in `_expirationPeriod` and `_maxPriceDeviation` arguments instead of the constants `EXPIRATION_PERIOD` and `MAX_DEVIATION_RATE`. The relevant code should be updated as follows:
```
function initialize(
    address _admin,
    address[] memory _feeders,
    address[] memory _assets,
    uint128 _expirationPeriod,
    uint128 _maxPriceDeviation
) public initializer {
    _addAssets(_assets);
    _addFeeders(_feeders);
    _setupRole(DEFAULT_ADMIN
```

## 8. Oracle Configuration Error

Summary: The `NFTFloorOracle` contract has an error in its configuration. The `EXPIRATION_PERIOD` constant is incorrectly set to `1800` instead of `21600`.

Impact: This error will cause prices in the contract to expire too soon, potentially leading to incorrect price aggregation and inaccurate information being provided to users.

Recommendation: Update the value of the `EXPIRATION_PERIOD` constant to `21600` to fix the error. This will ensure that prices do not expire too soon and that accurate price information is provided to users.

## 9. NFTFloorOracle not checking if feeders is empty before updating

Summary: The `NFTFloorOracle` contract does not check if feeders is empty before updating `feederPrice` in the `updatePrice` function.

Impact: This bug can cause the contract to update `feederPrice` with undefined values, resulting in incorrect data being stored in the contract.

Recommendation: Add a check to verify that `feeders` is not empty before updating `feederPrice`. This can be done by adding an if statement that checks the length of `feeders` and only updates `feederPrice` if the length is greater than 0.

## 10. Invalid memory allocation for `FeederRegistrar` in `NFTFloorOracle` contract

Summary: The `NFTFloorOracle` contract is initializing the `FeederRegistrar` struct with a fixed-length array of size `1`, but is not allocating any memory for the array. This can cause memory access errors when the contract attempts to access elements of the array.

Impact: Depending on the context in which the contract is being used, this bug could cause the contract to crash or become unresponsive. It could also potentially result in lost funds or other unintended consequences.

Recommendation: The `FeederRegistrar` struct should be initialized with a dynamically-allocated array, rather than a fixed-length array of size `1`. This will ensure that the contract has sufficient memory allocated for the array, and will prevent memory access errors.

## 11. Update price lags behind in NFTFloorOracle contract

Summary: The `NFTFloorOracle` contract does not properly handle the case where the price update lags behind. If the update lags behind, the contract could be at risk of not providing an accurate representation of the current price.

Impact: If the price update lags behind, it could result in incorrect prices being reported to users of the contract. This could have a negative impact on the users who rely on the contract to provide accurate pricing information.

Recommendation: To fix this issue, the contract should include additional checks to ensure that the reported prices are not too far behind the current time. This could be done by adding a `timestamp` field to the `PriceInformation` struct, and then using that field to check if the reported price is still within the acceptable time frame. If the price is too old, the contract could either reject the update or use the previous price until a new, valid update is provided.

## 12. Incorrect feeder map storage in NFTFloorOracle contract

Summary:
The `NFTFloorOracle` contract incorrectly uses a `mapping(address => FeederPosition)` to store the position of each feeder in the `feeders` array. This causes the contract to behave incorrectly when the number of feeders exceeds the maximum size of a `mapping` (2^256-1).

Impact:
This bug can cause the contract to behave unpredictably when the number of feeders exceeds the maximum size of a `mapping`. This could potentially result in incorrect prices being reported for assets on the contract.

Recommendation:
To fix this issue, the contract should use a `mapping(uint256 => address)` to store the position of each feeder in the `feeders` array. This will allow the contract to store an unlimited number of feeders without exceeding the maximum size of a `mapping`. Additionally, the contract should check for the maximum number of feeders allowed before adding a new feeder to ensure that it does not exceed the maximum size of a `mapping`.

## 13. Violation of the minimum number of oracles in NFTFloorOracle contract

Summary: The NFTFloorOracle contract does not enforce a minimum number of oracles for price aggregation, which could potentially result in incorrect or unreliable prices.

Impact: If the number of oracles is less than the minimum required, the price aggregation mechanism in the contract may not function correctly and could result in incorrect or unreliable prices. This could lead to incorrect valuation of assets and potentially impact the financial health of users and organizations that rely on the contract.

Recommendation: The contract should enforce a minimum number of oracles for price aggregation by modifying the `_aggregatePrice` function to check that the number of oracles is greater than or equal to the minimum required. The contract should also revert the transaction and throw an error if the minimum number of oracles is not met.

## 14. Incorrect implementation of asset removal function in NFTFloorOracle contract

Summary: The function `removeAsset` in the `NFTFloorOracle` contract does not correctly remove the given asset from the `assets` array and the `assetFeederMap` mapping.

Impact: If an asset is removed from the contract using the `removeAsset` function, it will still appear in the `assets` array and the `assetFeederMap` mapping, potentially leading to confusion and incorrect price aggregation.

Recommendation: The `removeAsset` function should be updated to properly remove the given asset from the `assets` array and the `assetFeederMap` mapping. This can be done by setting the value at the given asset's index in the `assets` array to the zero address and deleting the entry in the `assetFeederMap` mapping for the given asset.

# --> 2nd report for : ParaSpaceOracle.sol https://github.com/para-space/paraspace-core/blob/main/contracts/misc/ParaSpaceOracle.sol

## 1. ParaSpaceOracle Bug

Summary: The contract `ParaSpaceOracle` has a bug that allows unauthorized users to change the price sources for assets and the fallback oracle.

Impact: This bug allows any user to change the price sources for assets and the fallback oracle, potentially leading to incorrect or malicious price data being used in the contract.

Recommendation: The `setAssetSources` and `setFallbackOracle` functions should be marked with the `onlyAssetListingOrPoolAdmins` modifier to restrict access to only authorized users.

## 2.  Inconsistent number of asset and source addresses in ParaSpaceOracle constructor

Summary: The `ParaSpaceOracle` contract contains a bug in its constructor where it checks that the number of assets and sources provided match, but does not check that each asset has a corresponding source. This can lead to incorrect data being stored in the `assetsSources` mapping.

Impact: If an attacker calls the `ParaSpaceOracle` contract with an inconsistent number of assets and sources, they can potentially manipulate the stored data in the `assetsSources` mapping, leading to incorrect price data being returned to users. This can also result in the fallback oracle being called incorrectly.

Recommendation: The check in the constructor of the `ParaSpaceOracle` contract should be updated to ensure that each asset has a corresponding source. Additionally, checks should be put in place to ensure that the data in the `assetsSources` mapping is always consistent and correct.

## 3. ParaSpaceOracle contract - Inconsistent parameters length error

Summary: The _setAssetsSources() function in the ParaSpaceOracle contract contains a bug that causes it to revert if the assets and sources arrays have different lengths.

Impact: This bug can cause the contract to fail and prevent users from updating the sources for each asset.

Recommendation: The condition in the require statement on line 64 should be updated to check that the length of the assets and sources arrays are equal. This can be done by adding an additional condition to the existing require statement, or by using a different comparison operator, such as == or ===.

## 4. Improper use of require() in ParaSpaceOracle contract

Summary: The ParaSpaceOracle contract uses the require() function in a way that can result in a potential loss of funds. In the `_setAssetsSources()` function, there is a require statement that checks whether the length of the `assets` and `sources` arrays are equal. However, if the lengths are not equal, the function does not revert the transaction. Instead, it continues execution and tries to assign a value to `assetsSources[assets[i]]`, which can result in a loss of funds if the `assets` array is longer than the `sources` array.

Impact: If the `assets` array is longer than the `sources` array, the contract will continue execution and try to assign a value to `assetsSources[assets[i]]` for each index in the `assets` array, potentially resulting in a loss of funds.

Recommendation: The require statement in the `_setAssetsSources()` function should be changed to include a revert() call to prevent the potential loss of funds. The updated code should look like this:
```
function _setAssetsSources(
    address[] memory assets,
    address[] memory sources
) internal {
    require(
        assets.length == sources.length,
        Errors.INCONSISTENT_PARAMS_LENGTH
    );
    for (uint256 i = 0; i < assets.length; i++) {
        require(
            assets[i] != BASE_CURRENCY,
            Errors.SET_ORACLE_SOURCE_NOT_ALLOWED
        );
        assetsSources[assets[i]] = sources[i];
    }
}
```
## 5. ParaSpaceOracle contract does not check for fallback oracle's contract type

Summary: The ParaSpaceOracle contract does not check if the fallback oracle address provided to the `setFallbackOracle` function is a contract address. As a result, it is possible to set the fallback oracle to a non-contract address.

Impact: If the fallback oracle is set to a non-contract address, calls to the `getPrice` function will revert, leading to potential loss of funds for users of the contract.

Recommendation: Add a check in the `setFallbackOracle` function to verify that the provided fallback oracle address is a contract address. This can be done by calling the `isContract` function provided by the OpenZeppelin `Address` library.

# --> 3rd report for : UniswapV3OracleWrapper.sol https://github.com/para-space/paraspace-core/blob/main/contracts/misc/UniswapV3OracleWrapper.sol 

## 1. UniswapV3OracleWrapper contract is susceptible to rounding errors

Summary: The `getOnchainPositionData` function in the UniswapV3OracleWrapper contract calculates the price of an NFT token using the `currentPrice` value from a UniswapV3 pool contract. However, this calculation is susceptible to rounding errors due to the use of floating point numbers.

Impact: This issue can cause the contract to return incorrect price information for NFT tokens, leading to potential financial losses for users who rely on the contract for accurate pricing data.

Recommendation: Avoid using floating point numbers in the `getOnchainPositionData` function. Instead, use fixed point numbers with a sufficiently large number of decimal places to ensure that the calculation is accurate.

## 2. Potential security vulnerability in UniswapV3OracleWrapper contract

Summary: The UniswapV3OracleWrapper contract contains a potentially security vulnerability in the `getOnchainPositionData` function. The function does not check if `token0` or `token1` in `UNISWAP_V3_POSITION_MANAGER.positions(tokenId)` are `null` before calling `UNISWAP_V3_FACTORY.getPool(token0, token1, fee)`. If either of these values are `null`, it can cause an error when calling `UNISWAP_V3_FACTORY.getPool`.

Impact: If exploited, this vulnerability could allow an attacker to potentially cause an error in the contract and disrupt its normal functioning.

Recommendation: The `getOnchainPositionData` function should check if `token0` and `token1` are `null` before calling `UNISWAP_V3_FACTORY.getPool(token0, token1, fee)`. This can be done by adding an `require` statement to check if `token0` and `token1` are not `null` before calling `UNISWAP_V3_FACTORY.getPool`.
```
function getOnchainPositionData(uint256 tokenId)
    public
    view
    returns (UinswapV3PositionData memory)
{
    (
        ,
        ,
        address token0,
        address token1,
        uint24 fee,
        int24 tickLower,
        int24 tickUpper,
        uint128 liquidity,
        uint256 feeGrowthInside0LastX128,
        uint256 feeGrowthInside1LastX128,
        uint256 tokensOwed0,
        uint256 tokensOwed1
    ) = UNISWAP_V3_POSITION_MANAGER.positions(tokenId);
    require(token0 != address(0) && token1 != address(0), "Token0 and token1 cannot be null");

    IUniswapV3PoolState pool = IUniswapV3PoolState(
        UNISWAP_V3_FACTORY.getPool(token0, token1, fee)
    );
    (uint160 currentPrice, int24 currentTick, , , , , ) = pool.slot0();

    return
        UinswapV3PositionData({
            token0: token0,
            token1: token1,
            fee: fee,
            tickLower: tickLower,
            tickUpper: tickUpper,
            liquidity: liquidity,
            feeGrowthInside0LastX128: feeGrowthInside0LastX128,
            feeGrowthInside1LastX128: feeGrowthInside1LastX128,
            tokensOwed0: tokensOwed0,
            tokensOwed1: tokensOwed1,
            currentPrice: currentPrice,
            currentTick: currentTick
        });
}
```

## 3. Incorrect Fee Calculation in UniswapV3OracleWrapper contract

Summary: The `getOnchainPositionData` function in the UniswapV3OracleWrapper contract calculates the fee incorrectly. It uses the `tickLower` and `tickUpper` values from the `UNISWAP_V3_POSITION_MANAGER.positions()` call to calculate the fee, but these values do not correspond to the fee. Instead, the `fee` value returned by the same call should be used.

Impact: This bug can cause the contract to return incorrect fee data for a given Uniswap V3 pool. This may lead to users making incorrect decisions based on the faulty fee data.

Recommendation: The `getOnchainPositionData` function should be updated to use the `fee` value from the `UNISWAP_V3_POSITION_MANAGER.positions()` call instead of the `tickLower` and `tickUpper` values. This will ensure that the correct fee data is returned for a given Uniswap V3 pool.

## 4. Error in setFallbackOracle() function

Summary: The setFallbackOracle() function allows the caller to change the fallbackOracle address without any checks. This allows any address to be set as the fallbackOracle, which could potentially be an attacker's address.

Impact: An attacker could potentially set their own address as the fallbackOracle and use it to perform malicious actions, such as manipulating the price of assets or stealing funds.

Recommendation: Implement a check to ensure that only the contract owner or an authorized address can change the fallbackOracle address. This can be done by adding a require statement that checks if the caller's address is the same as the contract owner or an authorized address. For example:
```
require(msg.sender == contractOwner || msg.sender == authorizedAddress, "Only the contract owner or authorized address can change the fallbackOracle address.");
```

## 5. Possible Integer Overflow Vulnerability in UniswapV3OracleWrapper Contract

Summary:
The `getOnchainPositionData` function in the UniswapV3OracleWrapper contract multiplies the `feeGrowthInside1LastX128` value by `Q128` (which is `2^128`) in lines 92-93, which may result in an integer overflow if the `feeGrowthInside1LastX128` value is large enough.

Impact:
If an integer overflow occurs, the contract may not behave as expected, potentially leading to incorrect calculations or vulnerabilities that can be exploited by attackers.

Recommendation:
To avoid this issue, consider using the `SafeMath` library or other safe math operations to handle these calculations. Alternatively, you can check the `feeGrowthInside1LastX128` value to ensure that it does not exceed the maximum value that can be safely multiplied by `Q128` without causing an integer overflow.

## 6. Incorrect use of `int24` causing underflow in UniswapV3OracleWrapper contract

Summary: The `int24` type is being used incorrectly in the `UniswapV3OracleWrapper` contract, potentially causing an underflow that could result in incorrect calculations.

Impact: If the `int24` type underflows, the contract could provide incorrect price data for Uniswap pools. This could cause financial losses for users relying on this data.

Recommendation: Replace the `int24` type with a signed integer type that supports the necessary range of values, such as `int256`. Alternatively, consider using the `SafeMath` library to prevent underflow and overflow in calculations involving the `int24` type.

## 7. UniswapV3OracleWrapper contract has a bug

Summary: In the `getOnchainPositionData` function in the UniswapV3OracleWrapper contract, there is a bug that causes the `pairOracleData` local variable to be returned instead of the `onchainData` local variable.

Impact: This bug causes the returned value to be incorrect, as `pairOracleData` contains data about the Uniswap pair and not the on-chain position data of the specified token.

Recommendation: Modify the `getOnchainPositionData` function to return `onchainData` instead of `pairOracleData`.

## 8. Inconsistent state in UniswapV3OracleWrapper

Summary: In the `getOnchainPositionData()` function of the `UniswapV3OracleWrapper` contract, the `pool` variable is assigned to the result of calling `IUniswapV3Factory.getPool()`, but this function may return an empty address. If `pool` is an empty address, then calling `pool.slot0()` on the next line will result in a runtime error.

Impact: If this bug is exploited, it could cause the contract to become stuck in an inconsistent state, potentially leading to loss of funds.

Recommendation: Add a check to ensure that `pool` is not an empty address before calling `pool.slot0()`. Alternatively, use the `require()` function to check the return value of `IUniswapV3Factory.getPool()` and revert the transaction if it returns an empty address.

## 9. Incorrect calculation of feeGrowthOutside1X128Lower in UniswapV3OracleWrapper contract

Summary: The calculation of the `feeGrowthOutside1X128Lower` variable in the `getOnchainFeeParams` function of the UniswapV3OracleWrapper contract is incorrect. The `feeGrowthInside1LastX128` variable is divided by `Q128` instead of multiplied by it, which means the value of `feeGrowthOutside1X128Lower` is always zero.

Impact: This bug causes the `feeGrowthOutside1X128Lower` variable to always be zero, which may result in incorrect calculations in other parts of the contract or any contract that uses this variable.

Recommendation: Multiply `feeGrowthInside1LastX128` by `Q128` instead of dividing it to correctly calculate the value of `feeGrowthOutside1X128Lower`.

## 10. Incorrect fee calculation for UniswapV3OracleWrapper contract

Summary: The `_getFeeParams` function in the `UniswapV3OracleWrapper` contract incorrectly calculates the fees for a given liquidity amount.

Impact: This can result in incorrect fees being calculated and used for token swaps on the Uniswap V3 pool. This could lead to incorrect amounts of tokens being received in exchange for other tokens.

Recommendation: The `_getFeeParams` function should be updated to correctly calculate the fees for a given liquidity amount. This would involve correctly handling the case where the liquidity amount is greater than or equal to `0x100000000000000000000000000000000`.

## 11. Lack of checks in _getLiquidityAmounts

Summary: The `_getLiquidityAmounts` function does not check if the provided pool exists or not, which could result in a call to a non-existent contract.

Impact: This could result in a call to a non-existent contract, which would cause a contract execution to halt and the transaction to fail.

Recommendation: Add checks to verify that the provided pool exists before calling it. This can be done by calling the `getPool` function on the Uniswap V3 factory contract, and checking if the returned address is not the zero address.

## 12. Access Control Vulnerability in UniswapV3OracleWrapper Contract

Summary:
The UniswapV3OracleWrapper contract does not properly enforce access controls for the `_updatePositionData` function. This function allows an attacker to set the values of `_token0Price`, `_token1Price`, and `_sqrtPriceX96` for any ERC-20 token pair. This can be exploited by an attacker to manipulate the price of any ERC-20 token pair.

Impact:
An attacker could manipulate the price of any ERC-20 token pair by calling the `_updatePositionData` function and setting the values of `_token0Price`, `_token1Price`, and `_sqrtPriceX96` to arbitrary values. This could lead to financial loss for users of the contract who rely on the accuracy of the price data.

Recommendation:
Restrict access to the `_updatePositionData` function by adding a modifier that checks the caller's permissions before allowing them to execute the function. This will prevent unauthorized users from being able to manipulate the price data.

## 13. Incorrect Calculation of UniswapV3OracleWrapper.getOnchainPositionData()

Summary: The `getOnchainPositionData()` function in the `UniswapV3OracleWrapper` contract calculates the `sqrtPriceX96` field of the returned `PairOracleData` struct incorrectly. It should be calculated using the `sqrt()` function from the `SqrtLib` library, but instead it is calculated using the `sqrtPriceX96()` function from the `FullMath` library.

Impact: This issue can cause the `sqrtPriceX96` field of the returned `PairOracleData` struct to be incorrect, which can affect the calculation of other fields in the struct and any contracts or functions that use the output of this function.

Recommendation: The `sqrtPriceX96` field should be calculated using the `sqrt()` function from the `SqrtLib` library, instead of using the `sqrtPriceX96()` function from the `FullMath` library. This can be fixed by replacing the following line of code in the `getOnchainPositionData()` function:
```
uint160 sqrtPriceX96 = FullMath.sqrtPriceX96(currentPrice);
```

with this line of code:

```
uint160 sqrtPriceX96 = SqrtLib.sqrt(currentPrice);
```

## 14. Out-of-bounds array access in `UniswapV3OracleWrapper` contract

Summary: The `getOnchainPositionData` function in the `UniswapV3OracleWrapper` contract does not check the bounds of the `tokenId` parameter when accessing the `UNISWAP_V3_POSITION_MANAGER`'s `positions` array, potentially allowing an attacker to read data from unintended storage locations.

Impact: If an attacker is able to exploit this vulnerability, they could potentially read data from unintended storage locations, potentially revealing sensitive information or allowing them to manipulate the contract's state.

Recommendation: The `getOnchainPositionData` function should be updated to check the bounds of the `tokenId` parameter before accessing the `UNISWAP_V3_POSITION_MANAGER`'s `positions` array. This could be done by adding a check to ensure that `tokenId` is less than the length of the `positions` array before accessing the array.

# --> 4th report for : PoolAddressesProvider.sol  https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/configuration/PoolAddressesProvider.sol 

## 1. Critical Vulnerability in `PoolAddressesProvider` Contract

Summary: The `PoolAddressesProvider` contract is owned by the ParaSpace Governance, which is controlled by a single address. This means that the owner of this address has complete control over the contract and can change the addresses of all the registered contracts, including the `ACL_MANAGER` and `ACL_ADMIN` contracts, which manage the permissions of all the other contracts in the system.

Impact: An attacker who controls the owner address of the `PoolAddressesProvider` contract can change the addresses of all the registered contracts, including the `ACL_MANAGER` and `ACL_ADMIN` contracts, effectively gaining control over the entire system.

Recommendation: The `PoolAddressesProvider` contract should be owned by a multisig wallet or a contract that has a more decentralized ownership model, such as a contract that is controlled by a committee of multiple addresses. This would make it more difficult for a single attacker to gain control over the contract and the entire system.

## 2. PoolAddressesProvider.setAddress() Integer Overflow Vulnerability

Summary
The `setAddress()` function in the `PoolAddressesProvider` contract does not check for an integer overflow when updating the `_addresses` mapping. This can allow an attacker to insert an invalid address into the mapping by providing a large `id` value.

Impact
An attacker could insert an invalid address into the `_addresses` mapping, potentially leading to incorrect contract behavior or vulnerabilities in other parts of the contract.

Recommendation
The `setAddress()` function should be modified to check for an integer overflow when updating the `_addresses` mapping. This can be done by checking that the sum of the `id` value and the length of the `_addresses` mapping is less than or equal to the maximum value that can be stored in a `bytes32` variable. For example:
```
function setAddress(bytes32 id, address newAddress)
    external
    override
    onlyOwner
{
    require(id + _addresses.length <= 2**256 - 1, "Overflow error");
    address oldAddress = _addresses[id];
    _addresses[id] = newAddress;
    emit AddressSet(id, oldAddress, newAddress);
}
```
Alternatively, the `_addresses` mapping could be changed to use a `bytes32` variable as the key instead of an integer. This would prevent an integer overflow from occurring, but would require changes to other parts of the contract that use the `_addresses` mapping.

## 3. ParaProxy contract has a vulnerability that allows an attacker to steal funds from the contract

Summary: The ParaProxy contract contains a vulnerability that allows an attacker to steal funds from the contract by calling the `initialize()` function. This function is intended to be called only by the contract owner, but it can be called by anyone. The vulnerability is caused by the fact that the contract does not check the caller of the `initialize()` function.

Impact: An attacker can exploit this vulnerability to steal funds from the ParaProxy contract.

Recommendation: The `initialize()` function should be marked as `internal` and the contract should be updated to check the caller of the function to ensure that only the contract owner is allowed to call it. This will prevent an attacker from being able to exploit the vulnerability.

## 4. Changing the POOL contract

Summary: The `POOL` contract can be changed by the contract owner, allowing them to change it to a malicious contract.

Impact: If the contract owner changes the `POOL` contract to a malicious contract, they can steal assets from the contract.

Recommendation: The `POOL` contract should not be able to be changed by the contract owner. Instead, the `POOL` contract should be set to a trusted contract that cannot be changed. Alternatively, a governance mechanism could be put in place that allows a group of trusted parties to change the `POOL` contract.

## 5. Incorrect Use of the `onlyOwner` modifier in the `PoolAddressesProvider` contract.

Summary:
The `PoolAddressesProvider` contract uses the `onlyOwner` modifier in the `setAddressAsProxy` function, but this function can be called by anyone since it is marked as `external`. This means that anyone can call this function and replace the implementation of the proxy without being the owner of the contract.

Impact:
The incorrect use of the `onlyOwner` modifier in the `setAddressAsProxy` function can be exploited by an attacker to replace the implementation of a proxy contract without being the owner of the `PoolAddressesProvider` contract. This can lead to the attacker gaining unauthorized access to the functions of the proxy contract.

Recommendation:
The `setAddressAsProxy` function should be marked as `internal` or `private` instead of `external` to prevent it from being called by anyone other than the contract itself. This will ensure that the `onlyOwner` modifier is correctly applied and the function can only be called by the contract owner.

## 6. Unchecked Proxy Implementation Update

Summary: The `setAddressAsProxy` function in the `PoolAddressesProvider` contract allows the owner to update the implementation of a proxy contract without checking the provided `newImplementationAddress` for correctness. This can potentially allow the owner to execute arbitrary code on the proxy contract.

Impact: This vulnerability allows the owner of the contract to execute arbitrary code on the proxy contract, potentially leading to loss of funds or other malicious actions.

Recommendation: The `setAddressAsProxy` function should check the provided `newImplementationAddress` for correctness before updating the proxy's implementation. This can be done by calling `assertIsContract(newImplementationAddress)` or by using a similar check to verify that the provided address is a contract.

## 7. DoS Vulnerability in "PoolAddressesProvider" contract

Summary: The `PoolAddressesProvider` contract allows anyone to add any number of marketplaces to the protocol by calling the `addMarketplace` function. This function does not have a gas limit, allowing an attacker to cause a DoS attack by calling this function repeatedly with different marketplace addresses.

Impact: An attacker can cause a DoS attack on the protocol by calling the `addMarketplace` function repeatedly with different marketplace addresses. This will cause the contract to consume an increasing amount of gas, potentially reaching the block gas limit and causing the Ethereum network to stop processing transactions.

Recommendation: Implement a gas limit on the `addMarketplace` function to prevent an attacker from causing a DoS attack. This gas limit should be set to a reasonable amount that allows users to add a small number of marketplaces, but prevents an attacker from adding an excessive number of marketplaces.

# --> 5th report for : AuctionLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/AuctionLogic.sol

## 1. Unbounded Memory Usage in `executeEndAuction` Function

Summary:
The `executeEndAuction` function in the `AuctionLogic` library does not properly bound the number of elements that can be stored in the `DataTypes.ExecuteAuctionParams` memory struct. This can result in a contract that uses the `AuctionLogic` library to run out of gas and be unable to operate properly.

Impact:
If an attacker is able to exploit this vulnerability, they could cause a contract that uses the `AuctionLogic` library to run out of gas and be unable to function properly. This could lead to a denial of service attack against the contract and any users who rely on it.

Recommendation:
The `executeEndAuction` function should properly bound the number of elements that can be stored in the `DataTypes.ExecuteAuctionParams` memory struct to prevent unbounded memory usage. This can be done by adding a `memory` keyword to the `params` parameter in the function signature, like so:
```
function executeEndAuction(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
    DataTypes.ExecuteAuctionParams memory params
)
```
This will limit the amount of memory that can be used when storing the `params` struct, and prevent the contract from running out of gas.

## 2. Improper Authorization for `executeEndAuction` Function

Summary: The `executeEndAuction` function in the `AuctionLogic` library can be called by any address, allowing any address to end an auction on an ERC721 token. This can allow an attacker to end auctions on ERC721 tokens that they do not own and potentially manipulate the market.

Impact: This vulnerability allows an attacker to manipulate the market by ending auctions on ERC721 tokens that they do not own. This can potentially result in financial losses for users of the market.

Recommendation: The `executeEndAuction` function should be restricted to only be called by authorized addresses, such as the owner of the contract or a contract with the appropriate permission. This can be achieved by adding a require statement at the beginning of the function to check that the caller has the necessary authorization.

## 3. `executeStartAuction` function can call a zero address

Summary: The `executeStartAuction` function in the `AuctionLogic` contract does not check if the `collateralXToken` is not zero address before calling the `startAuction` function on it. This can cause the call to fail if the `collateralXToken` is the zero address.

Impact: The `executeStartAuction` function can fail if the `collateralXToken` is the zero address. This can cause the auction to fail to start, leading to a potential loss of funds for the user.

Recommendation: The `executeStartAuction` function should check if the `collateralXToken` is not the zero address before calling the `startAuction` function on it. This will ensure that the `startAuction` function is called only on valid addresses and avoid potential losses for the user.

## 4. Arbitrary Contract Execution in `executeEndAuction`

Summary: The `executeEndAuction` function in the `AuctionLogic` contract accepts a parameter `strategy` of type `IReserveAuctionStrategy` which is used without any checks for the address or contract type, allowing an attacker to supply an arbitrary contract that implements the `IReserveAuctionStrategy` interface and execute arbitrary code.

Impact: An attacker can execute arbitrary code and gain control of the contract by supplying a malicious contract as the `strategy` parameter. This can allow the attacker to steal funds from the contract or perform other malicious actions.

Recommendation: The `executeEndAuction` function should include checks for the address and contract type of the `strategy` parameter before using it. This will prevent an attacker from supplying a malicious contract as the `strategy` parameter.

## 5. Reentrancy Attack on executeEndAuction()

Summary: The `executeEndAuction()` function in the `AuctionLogic` library is vulnerable to a reentrancy attack. If an attacker calls `executeEndAuction()` with a malicious `IReserveAuctionStrategy` contract, the attacker can repeatedly call `executeEndAuction()` until the `collateralXToken` contract is drained of its funds.

Impact: An attacker can potentially drain the `collateralXToken` contract of its funds. This could lead to a loss of funds for the users of the `collateralXToken` contract and could potentially disrupt the functioning of the protocol.

Recommendation: The `executeEndAuction()` function should be modified to include a `require()` statement that checks for reentrancy. For example, the function could check if the current contract is calling itself before calling the `endAuction()` function in the `collateralXToken` contract.

# --> 6th report for : BorrowLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/BorrowLogic.sol

## 1. Unvalidated Address in BorrowLogic.executeBorrow

Summary: The `executeBorrow` function in the BorrowLogic contract uses an unvalidated user-supplied address to mint a token. This can be exploited by a malicious user to mint tokens on an arbitrary address, potentially leading to loss of funds.

Impact: A malicious user can exploit this vulnerability to mint tokens on an arbitrary address. This can lead to the loss of funds and can potentially be used to attack other smart contracts or users in the network.

Recommendation: It is recommended to validate the user-supplied address before using it to mint tokens. This can be done by checking that the address is the expected contract address and that it is not the zero address. Additionally, measures such as rate limiting and access control can be implemented to mitigate the risk of such attacks.

## 2. Improper Validation in `executeBorrow()` Function

Summary:
The `executeBorrow()` function in the `BorrowLogic` contract does not properly validate input parameters. In particular, the `amount `parameter is not checked for overflows before it is used to calculate `minHeldAmount` and `borrowBalanceAmount`. This can be exploited by an attacker to cause an overflow and potentially manipulate the `minHeldAmount` and `borrowBalanceAmount` variables to arbitrary values.

Impact:
If exploited, this vulnerability can allow an attacker to manipulate the `minHeldAmount` and `borrowBalanceAmount` variables to arbitrary values, potentially allowing them to borrow more than they are entitled to.

Recommendation:
The `executeBorrow()` function should properly check the `amount` parameter for overflows before using it in any calculations. This can be done by checking that `amount + userConfig.borrowBalanceAmount[reserve.id]` does not overflow.

## 3. Unrestricted Write Access in `executeBorrow` Function

Summary
The `executeBorrow` function in the `BorrowLogic` library allows a caller to borrow assets from a reserve. This function contains an unrestricted write access vulnerability because it does not check the caller's authorization before modifying the `reserveData` and `userConfig` storage variables. As a result, any caller can potentially borrow assets from a reserve without the necessary authorization.

Impact
This vulnerability allows any caller to borrow assets from a reserve without the necessary authorization. This can lead to unauthorized access to funds and can potentially result in financial losses for the reserve and its users.

Recommendation
The `executeBorrow` function should be modified to check the caller's authorization before modifying the `reserveData` and `userConfig` storage variables. This can be done by adding an `onlyReserveManager` or `onlyOwner` modifier to the function, or by adding an explicit check for the caller's authorization in the function body. Additionally, it is recommended to review the contract code for similar instances of unrestricted write access and apply the necessary fixes.

## 4. Integer Overflow/Underflow in BorrowLogic Library

Summary:

The `executeBorrow()` function in the `BorrowLogic` library is vulnerable to integer overflows and underflows. This can occur when calculating the `nextScaledVariableDebt` variable, which is calculated by multiplying the `amount` parameter by `variableDebtScalingFactor` and then dividing it by `variableDebtScalingFactor`. If the `amount` parameter is set to the maximum or minimum possible value, the multiplication and division will cancel each other out, resulting in an overflow or underflow. This can be exploited by a malicious actor to cause unexpected behavior or security issues in the contract.

Impact:

If exploited, this vulnerability could allow a malicious actor to cause unexpected behavior or security issues in the contract.

Recommendation:

To fix this vulnerability, the `executeBorrow()` function should be modified to check for overflows and underflows before calculating the `nextScaledVariableDebt` variable. This can be done by using the `SafeMath` library provided by OpenZeppelin. Alternatively, the `nextScaledVariableDebt` variable can be calculated using a different formula that does not have the potential for overflows and underflows.

## 5. Use of Uninitialized Memory in `BorrowLogic.executeBorrow()`

Summary:

The `BorrowLogic.executeBorrow()` function contains a bug that can cause the contract to operate on uninitialized memory. This can potentially lead to incorrect or unintended behavior in the contract, and may result in security vulnerabilities.

Impact:

The impact of this bug is that the contract may behave in unintended ways, and may be vulnerable to attack. This could potentially result in loss of funds for users of the contract.

Recommendation:

The issue can be fixed by initializing the `reserveCache` variable before using it in the `ValidationLogic.validateBorrow()` function call. This will ensure that the `reserveCache` variable is properly initialized, and the contract will operate correctly.

## 6. Lack of Access Control in BorrowLogic

Summary: In the `BorrowLogic.executeBorrow()` function, there is no check to ensure that only authorized users can execute the borrow function. This allows any user to execute the borrow function and potentially manipulate the state of the contract.

Impact: An attacker could exploit this vulnerability by calling the `executeBorrow()` function without authorization and manipulate the contract state for their own benefit. This could result in financial loss for the users of the contract.

Recommendation: Implement an access control mechanism in the `executeBorrow()` function to ensure that only authorized users can execute the borrow function. This can be done by adding a require statement that checks the caller's address against a list of authorized addresses.

## 7. Lack of input validation in `executeBorrow` function

Summary: The `executeBorrow` function in the `BorrowLogic` contract does not properly validate user input in the `params.amount` parameter. An attacker can specify a large `amount` value and bypass the collateralization check in the `ValidationLogic.validateBorrow` function. This allows the attacker to borrow an arbitrary amount of tokens without providing sufficient collateral.

Impact: An attacker can borrow an arbitrary amount of tokens without providing sufficient collateral. This can lead to a liquidation of the attacker's position and a loss of funds for other users of the protocol.

Recommendation: The `executeBorrow` function should properly validate the `params.amount` parameter to ensure that it does not exceed the maximum allowed amount based on the user's collateralization ratio.

## 8. BorrowLogic Library Contains Vulnerable Mapping

Summary:
The BorrowLogic library contains a mapping variable that is not declared with the "public" or "internal" keyword. As a result, the mapping variable is accessible from any contract that uses the BorrowLogic library, potentially allowing an attacker to manipulate the mapping data.

Impact:
An attacker may be able to manipulate the mapping data in the BorrowLogic library, potentially leading to incorrect calculations or other unpredictable behavior in contracts that use the BorrowLogic library.

Recommendation:
To prevent this vulnerability, the mapping variable in the BorrowLogic library should be declared with the "public" or "internal" keyword, limiting its accessibility to only authorized contracts.

# --> 7th report for : FlashClaimLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/FlashClaimLogic.sol

## 1. FlashClaimLogic Library Contains Unchecked External Call

Summary:
The FlashClaimLogic library contains an external call to the `claim` function of the `FlashReserve` contract without checking the return value of the call. As a result, the call may fail without being detected, potentially leading to incorrect behavior in contracts that use the FlashClaimLogic library.

Impact:
An attacker may be able to cause the `claim` function of the `FlashReserve` contract to fail, potentially leading to incorrect calculations or other unpredictable behavior in contracts that use the FlashClaimLogic library.

Recommendation:
To prevent this vulnerability, the external call to the `claim` function in the `FlashClaimLogic` library should be checked for success before continuing with any other operations. This can be done by calling the `require` or `assert` functions after the external call, and providing an error message if the call fails.

## 2. FlashClaimLogic Library Contains Vulnerable Mapping

Summary:
The FlashClaimLogic library contains a mapping variable that is not declared with the "public" or "internal" keyword. As a result, the mapping variable is accessible from any contract that uses the FlashClaimLogic library, potentially allowing an attacker to manipulate the mapping data.

Impact:
An attacker may be able to manipulate the mapping data in the FlashClaimLogic library, potentially leading to incorrect calculations or other unpredictable behavior in contracts that use the FlashClaimLogic library.

Recommendation:
To prevent this vulnerability, the mapping variable in the FlashClaimLogic library should be declared with the "public" or "internal" keyword, limiting its accessibility to only authorized contracts.

## 3. FlashClaimLogic Library Contains Unchecked External Call

Summary:
The FlashClaimLogic library contains an external call to the `executeOperation` function of the `IFlashClaimReceiver` contract without checking the return value of the call. As a result, the call may fail without being detected, potentially leading to incorrect behavior in contracts that use the FlashClaimLogic library.

Impact:
An attacker may be able to cause the `executeOperation` function of the `IFlashClaimReceiver` contract to fail, potentially leading to incorrect calculations or other unpredictable behavior in contracts that use the FlashClaimLogic library.

Recommendation:
To prevent this vulnerability, the external call to the `executeOperation` function in the FlashClaimLogic library should be checked for success before continuing with any other operations. This can be done by calling the `require` or `assert` functions after the external call, and providing an error message if the call fails.

# --> 8th report for : GenericLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/GenericLogic.sol

## 1. GenericLogic Library Contains Unchecked External Calls

Summary:
The GenericLogic library contains multiple external calls to other contracts without checking the return value of the calls. As a result, the calls may fail without being detected, potentially leading to incorrect behavior in contracts that use the GenericLogic library.

Impact:
An attacker may be able to cause the external calls in the GenericLogic library to fail, potentially leading to incorrect calculations or other unpredictable behavior in contracts that use the GenericLogic library.

Recommendation:
To prevent this vulnerability, the external calls in the GenericLogic library should be checked for success before continuing with any other operations. This can be done by calling the `require` or `assert` functions after each external call, and providing an error message if the call fails.

# --> 9th report for : LiquidationLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/LiquidationLogic.sol

## 1. Integer Overflow in Liquidation Logic

Summary: The `executeLiquidation` function in the `LiquidationLogic` library contains a vulnerability that allows an attacker to cause an integer overflow.

Impact: An attacker can exploit this vulnerability to cause the contract to calculate incorrect values and potentially execute malicious actions.

Recommendation: Update the `executeLiquidation` function to check for and prevent integer overflow. This can be done by using `SafeMath` functions or by manually checking for overflow before performing arithmetic operations.

## 2.  Potential Overflow in `calculateUserAccountData` function

Summary: The `calculateUserAccountData` function in the GenericLogic library has the potential to overflow when computing the `totalCollateralInBaseCurrency` and `totalDebtInBaseCurrency` variables. This is because it multiplies `totalERC721CollateralInBaseCurrency` by `userBalanceInBaseCurrency` and `payableDebtByERC20Assets` by `userBalanceInBaseCurrency` respectively, without checking if the multiplication will overflow.

Impact: If an attacker is able to provide large values for `totalERC721CollateralInBaseCurrency` and `payableDebtByERC20Assets`, the resulting overflow could be used to manipulate the state of the contract and potentially cause loss of funds for users.

Recommendation: To prevent the potential overflow, the SafeMath library should be used for the multiplication operations in the `calculateUserAccountData` function. This will ensure that the multiplication will not result in an overflow and will revert if it would have otherwise.

## 3. Type Mistmatch in LiquidationLogic contract

Summary:

In the LiquidationLogic contract, the `DEFAULT_LIQUIDATION_CLOSE_FACTOR` and `MAX_LIQUIDATION_CLOSE_FACTOR` constants are declared as uint256, but their values are specified as percentages (with a value of 0.5e4 and 1e4 respectively). This causes a type mismatch, because Solidity will interpret the numbers as their raw value, not as percentages.

Impact:

This bug may cause unexpected behavior when calculating liquidations in the contract, because the wrong values will be used. This could result in incorrect liquidation amounts being calculated, potentially leading to losses for users of the contract.

Recommendation:

To fix this issue, the values of `DEFAULT_LIQUIDATION_CLOSE_FACTOR` and `MAX_LIQUIDATION_CLOSE_FACTOR` should be specified as a fraction (not as a percentage), so that they are correctly interpreted as a uint256 value by Solidity. For example, the value of `DEFAULT_LIQUIDATION_CLOSE_FACTOR` could be changed to `0.005` (0.5%), and the value of `MAX_LIQUIDATION_CLOSE_FACTOR` could be changed to `0.01` (1.0%).

## 4. Integer Overflow in Liquidation Logic Library

Summary:
The LiquidationLogic library contains a potential integer overflow vulnerability in the `liquidate` function. The `closeFactor` value is calculated by dividing the `DEFAULT_LIQUIDATION_CLOSE_FACTOR` by the `pendingLiquidation` value and then multiplying by the `lotSize` value. This calculation can cause an integer overflow if the `pendingLiquidation` value is small and the `lotSize` value is large.

Impact:
An attacker could exploit this vulnerability to cause an integer overflow and potentially manipulate the `closeFactor` value to their advantage. This could result in the attacker being able to liquidate a larger amount of collateral than intended.

Recommendation:
The calculation of the `closeFactor` value should be modified to use SafeMath functions to prevent integer overflows. The `pendingLiquidation` value should also be checked to ensure it is not zero before performing the calculation.

## 5. Missing require statement in liquidateBorrower function

Summary: The liquidateBorrower function in the LiquidationLogic library is missing a "require" statement that would ensure that only the auctioneer contract can call it.

Impact: This vulnerability could allow anyone to call the liquidateBorrower function, which is intended to be called only by the auctioneer contract. This could result in unauthorized liquidations of borrowers' debts and collateral, potentially leading to loss of funds.

Recommendation: Add a "require" statement to the liquidateBorrower function to ensure that only the auctioneer contract can call it. This could be done by adding the following line before the existing code in line 124: require(msg.sender == auctioneer, "Only the auctioneer contract can call this function");

## 6. Incorrect Liquidation Close Factor

Summary: The code contains a vulnerability where the liquidation close factor is not properly defined. This could result in a scenario where a user with a health factor below the `CLOSE_FACTOR_HF_THRESHOLD` is liquidated for more than 100% of their debt.

Impact: This vulnerability could result in users being liquidated for more than 100% of their debt, leading to potential losses for the users and the protocol.

Recommendation: The `MAX_LIQUIDATION_CLOSE_FACTOR` should be properly defined and set to a value that is less than or equal to the `DEFAULT_LIQUIDATION_CLOSE_FACTOR` to prevent users from being liquidated for more than 100% of their debt.

## 7. Reentrancy Vulnerability in LiquidationLogic Library

Summary:
The `LiquidationLogic` library contains a reentrancy vulnerability in the `liquidate` function. When the `liquidate` function calls `transfer` on the `_collateral` token contract, it does not check for the return value of the `transfer` call before continuing execution. This allows an attacker to reenter the `liquidate` function through a malicious `_collateral` token contract and potentially steal collateral from the contract.

Impact:
An attacker can potentially steal collateral from the contract through a malicious `_collateral` token contract.

Recommendation:
The `liquidate` function should check the return value of the `transfer` call before continuing execution to prevent reentrancy attacks. This can be done by using the `require` statement to ensure that the `transfer` call returns `true`.

## 8. Hard-coded default liquidation close factor

Summary: 
The DEFAULT_LIQUIDATION_CLOSE_FACTOR constant in the LiquidationLogic library has a hard-coded value of 0.5e4, representing 50% of the borrower's debt to be repaid in a liquidation. This could result in financial losses for the protocol.

Impact: 
If a borrower defaults on their loan and the liquidation is triggered, only 50% of their debt will be repaid, potentially leading to financial losses for the protocol. In a worst-case scenario, if multiple borrowers default on their loans and the liquidation process is triggered, the financial losses for the protocol could be significant.

Recommendation: 
The DEFAULT_LIQUIDATION_CLOSE_FACTOR constant should be configurable, either through a separate contract or through a parameter in the constructor of the LiquidationLogic library. This would allow for the default liquidation close factor to be adjusted based on market conditions and risk assessments. This would reduce the potential for financial losses in the event of multiple defaults and liquidations.

To mitigate the impact of this vulnerability, it is recommended to implement a mechanism for adjusting the DEFAULT_LIQUIDATION_CLOSE_FACTOR constant based on market conditions and risk assessments. This could be done through a separate contract that is responsible for setting the value of the constant, or through a parameter in the constructor of the LiquidationLogic library that can be updated by the protocol's administrators. This would allow for the default liquidation close factor to be adjusted as needed, reducing the potential for financial losses in the event of multiple defaults and liquidations.

# --> 10th report for : MarketplaceLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/MarketplaceLogic.sol

## 1. Lack of input validation in `executeBuyWithCredit` function

Summary: The `executeBuyWithCredit` function in the MarketplaceLogic library lacks input validation on the `payload` parameter, potentially allowing attackers to provide malicious input and cause failures in the function.

Impact: If an attacker is able to provide malicious input to the `executeBuyWithCredit` function, it could cause a failure in the function and potentially result in financial losses for the protocol and its users. In a worst-case scenario, this could also lead to market instability and loss of trust in the protocol.

Recommendation: It is recommended to add input validation on the `payload` parameter in the `executeBuyWithCredit` function. This would ensure that only valid input is accepted, reducing the potential for failures and financial losses in the protocol. Additionally, it is recommended to implement error handling and recovery mechanisms in case of failures in the function.

## 2. Uninitialized variable used in function call

Summary: The `executeBuyWithCredit` function in the `MarketplaceLogic` contract contains a vulnerability where it does not validate the `marketplace` variable before calling its `adapter` property. This can result in a call to a potentially uninitialized `adapter` contract and can potentially cause the contract to be exploited by attackers.

Impact: If an attacker is able to provide a `marketplace` variable that is not properly initialized, they can potentially call the uninitialized `adapter` contract and cause the contract to fail or execute unintended behavior. This can result in loss of funds or other adverse effects.

Recommendation: The `executeBuyWithCredit` function should validate the `marketplace` variable before calling its `adapter` property to ensure that it has been properly initialized. This can be done by adding a check for the `marketplace` variable to be non-null before calling its `adapter` property. 
For example:

```
if (marketplace == address(0)) {
    // Throw an error or return from the function
}
```

## 3. Misuse of the `msg.value` variable

Summary: The `msg.value` variable is used to store the amount of ETH received by the contract in the `executeBuyWithCredit` function. However, this variable is not guaranteed to contain the correct value if the contract is called with a different method, leading to potential vulnerabilities.

Impact: This vulnerability could potentially be exploited by a malicious attacker to manipulate the internal state of the contract and cause unexpected behavior. This could lead to loss of funds or other security issues.

Recommendation: The `msg.value` variable should not be relied upon to store the amount of ETH received by the contract. Instead, the contract should explicitly require the amount of ETH to be passed as a parameter in the `executeBuyWithCredit` function.

## 4. Missing Authorization in MarketplaceLogic Library

Summary: The `executeBuyWithCredit()` function in the `MarketplaceLogic` library allows any user to execute a buy with credit operation, without checking if the user is authorized to perform this action.

Impact: An unauthorized user could execute a buy with credit operation and transfer a NFT to their own address, potentially leading to loss of funds for the original NFT owner.

Recommendation: Add proper authorization checks to the `executeBuyWithCredit()` function to ensure only authorized users can perform this action. This can be done by adding a call to an `isAuthorized()` function within the contract, which checks if the caller has the necessary permissions to execute this action.

## 5. Unrestricted Data Access in MarketplaceLogic Library

Summary: The MarketplaceLogic library allows unrestricted access to the user data stored in the UserConfiguration contract. This can potentially lead to unauthorized access to sensitive information, such as user addresses, referral codes, and credit values.

Impact: An attacker can potentially access sensitive user data and use it for malicious purposes, such as phishing attacks, identity theft, or unauthorized transactions.

Recommendation: To prevent unauthorized access to user data, the code should be updated to implement proper access control mechanisms, such as role-based or attribute-based access control, to limit access to the user data only to authorized users. Additionally, the user data should be encrypted to ensure its confidentiality and integrity.

## 6. Unchecked return value in executeBuyWithCredit() function

Summary:
The `executeBuyWithCredit()` function in the MarketplaceLogic library does not check the return value of the `IMarketplace.placeBid()` call. As a result, an attacker could place a successful bid on an NFT without actually paying for it.

Impact:
An attacker could place a bid on an NFT and successfully obtain it without actually paying for it. This could result in a loss of funds for the marketplace and its users.

Recommendation:
The `executeBuyWithCredit()` function should check the return value of the `IMarketplace.placeBid()` call to ensure that the bid was placed successfully. This can be done by adding an `if` statement to check if the return value is true, and if not, throwing an error.

## 7. Unchecked External Function Call in MarketplaceLogic Library

Summary: The `executeBuyWithCredit` function in the `MarketplaceLogic` library does not check the return value of the `getAskOrderInfo` external function call. As a result, an attacker could potentially manipulate the function to return a malicious orderInfo, leading to a potential re-entrancy attack.

Impact: If an attacker is able to manipulate the return value of the `getAskOrderInfo` function, they could potentially execute a re-entrancy attack, allowing them to execute arbitrary code on the contract. This could lead to loss of funds or other malicious actions.

Recommendation: The `executeBuyWithCredit` function should include a check on the return value of the `getAskOrderInfo` external function call to ensure it is valid before proceeding with the rest of the function logic.

## 8. Type Mismatch in Function Call

Summary: In the `executeBuyWithCredit` function, there is a type mismatch in the function call to `IMarketplace(marketplace.adapter).getAskOrderInfo(payload, vars.weth)`. The expected type for the second argument is `bytes32`, but `vars.weth` is of type `address`.

Impact: This type mismatch may result in unexpected behavior in the contract and can potentially lead to vulnerabilities.

Recommendation: The type of `vars.weth` should be changed to `bytes32` to match the expected type in the function call. The code should be updated as follows:
```
vars.weth = bytes32(poolAddressProvider.getWETH());
```

# --> 11th report for : PoolLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/PoolLogic.sol

## 1. Potential Reentrancy Vulnerability in `executeRescueTokens()` Function

Summary: The `executeRescueTokens()` function in the `PoolLogic` library contains a potential reentrancy vulnerability. If an attacker is able to control the `to` parameter, they can call the `executeRescueTokens() `function again before the function has completed, potentially leading to the attacker gaining an unauthorized amount of tokens.

Impact: If exploited, this vulnerability could allow an attacker to gain an unauthorized amount of tokens.

Recommendation: The `executeRescueTokens()` function should be modified to include a mutex or other mechanism to prevent reentrancy attacks. This could be done by using a `require()` statement to check a boolean value that is set at the beginning of the function and unset at the end, or by using a mutex contract to lock the function while it is being executed. This will help prevent the function from being called again before it has completed, mitigating the risk of a reentrancy attack.

## 2. Unrestricted `executeRescueTokens` function in PoolLogic library

Summary: The `executeRescueTokens` function in the PoolLogic library allows any user to call the function and rescue tokens locked in the contract. This function should only be accessible to authorized users.

Impact: An attacker could use this vulnerability to rescue tokens from the contract, potentially leading to financial loss for the contract owner or other users.

Recommendation: The `executeRescueTokens` function should be restricted to authorized users only, such as the contract owner or other trusted parties. This can be done by modifying the function to require that the caller have a specific role or permission before being able to execute the function. Additionally, access controls should be regularly reviewed and tested to ensure that they are functioning properly.

## 3. Potential Reentrancy Attack in `executeRescueTokens` Function

Summary: The `executeRescueTokens` function in the `PoolLogic` library is vulnerable to reentrancy attacks. The function calls the `transferFrom` function of the `token` contract without checking if the call is made from an untrusted contract. This allows an attacker to call the `transferFrom` function in a malicious contract that calls back into the `executeRescueTokens` function, potentially allowing the attacker to extract tokens from the contract repeatedly.

Impact: An attacker could potentially exploit this vulnerability to extract tokens from the contract repeatedly, resulting in a loss of funds for the contract owner and users of the contract.

Recommendation: To prevent this attack, the `executeRescueTokens` function should check if the caller is a trusted contract or address before calling the `transferFrom` function of the `token` contract. This can be done by checking if the caller is in a list of trusted contracts or addresses. Additionally, the `transferFrom` function should be called using the `call` or `delegatecall` opcodes instead of transfer, which will prevent the attacker's contract from executing its own code during the call. This will make it more difficult for the attacker to exploit the vulnerability.

## 4. Potential DoS attack in `executeRescueTokens()` function

Summary: The `executeRescueTokens()` function in the `PoolLogic` library contains a potential denial of service (DoS) attack vulnerability. If an attacker is able to repeatedly call the function with a large value for `amountOrTokenId`, it could cause the contract to run out of gas and become unresponsive.

Impact: An attacker could potentially use this vulnerability to cause the contract to become unresponsive, disrupting its normal operation and potentially leading to loss of funds.

Recommendation: In the `executeRescueTokens()` function, add a check to ensure that `amountOrTokenId` is not too large before executing the transfer. This will prevent attackers from calling the function with large values and potentially causing a DoS attack.

## 5. Solidity "require" Statement Type Coercion Vulnerability

Summary:
The code contains a potential vulnerability in which the Solidity "require" statement can be bypassed by an attacker through type coercion. This can occur in the "executeInitReserve" function when the "params.asset" value is not a contract address. In this case, the "require" statement is intended to check that "params.asset" is a contract address by calling the "isContract" function from the "Address" library. However, because the "isContract" function returns a boolean value, and the "require" statement expects a non-zero value, the attacker can provide a non-zero value that is not a contract address and still bypass the "require" check.

Impact:
If exploited, this vulnerability could allow an attacker to create a reserve with an invalid address and potentially cause further issues in the contract.

Recommendation:
The issue can be resolved by changing the "require" statement to directly check the boolean value returned by the "isContract" function. This can be done by using an equality comparison (==) instead of a non-zero check, as shown below:
```
require(Address.isContract(params.asset) == true, Errors.NOT_CONTRACT);
```
## 6. Lack of input validation in `executeInitReserve` function of `PoolLogic` contract

Summary: The `executeInitReserve` function in the `PoolLogic` contract does not properly validate its input parameters, which could potentially allow an attacker to manipulate the state of the contract in an unintended manner.

Impact: If exploited, this vulnerability could allow an attacker to add new reserves to the contract with arbitrary addresses, potentially allowing the attacker to manipulate the contract's state in unintended ways.

Recommendation: It is recommended to add proper input validation to the `executeInitReserve` function to ensure that only valid inputs are accepted by the contract. This can be done by checking that the `asset` parameter is a valid contract address, and by ensuring that the `reservesCount` parameter is less than the `maxNumberReserves` parameter. This will help to prevent attackers from being able to manipulate the contract's state in unintended ways.

## 7. Unrestricted emergency shutdown of contract

Summary: The `executeRescueTokens` function in the contract allows for unrestricted emergency shutdown of the contract by any caller.

Impact: This vulnerability allows anyone to call the `executeRescueTokens` function and immediately shut down the contract, potentially leading to loss of funds and disruption of services.

Recommendation: The `executeRescueTokens` function should be restricted to authorized users only, such as contract administrators or emergency shutdown procedures. This can be achieved by adding a require statement at the beginning of the function to check the caller's authorization. For example:
```
require(msg.sender == authorizedEmergencyShutdownAddress, "Caller is not authorized to perform emergency shutdown");
```

## 8. Unsafe address comparison in executeRescueTokens function

Summary:
The `executeRescueTokens` function in `PoolLogic` library contains an unsafe comparison of contract addresses. It uses the == operator to compare the address of the `token` parameter with the address of the `to` parameter. This can cause the function to transfer tokens to an attacker-controlled contract that has the same address as the `token` parameter.

Impact:
An attacker could create a contract with the same address as the `token` parameter, and use this vulnerability to steal tokens from the contract.

Recommendation:
The comparison of contract addresses in `executeRescueTokens` should be changed to use the `Address.isContract` function provided by the `Address` library. This will ensure that the `token` and `to` addresses are both valid contracts, and prevent an attacker from using a contract with the same address as the `token` parameter to steal tokens.

Here is an example of how the `executeRescueTokens` function could be updated to use the `Address.isContract` function:
```
function executeRescueTokens(
    DataTypes.AssetType assetType,
    address token,
    address to,
    uint256 amountOrTokenId
) external {
    require(Address.isContract(token), Errors.NOT_CONTRACT);
    require(Address.isContract(to), Errors.NOT_CONTRACT);

    // rest of function
}
```

## 9.  Incorrect error message when adding a reserve to the reserves list

Summary: The `executeInitReserve` function in the `PoolLogic` contract has a bug that causes it to return the incorrect error message when trying to add a reserve to the reserves list.

Impact: If a caller attempts to add a reserve to the reserves list and the list is already full, the contract will return the error message `Errors.RESERVE_ALREADY_ADDED` instead of the correct error message `Errors.NO_MORE_RESERVES_ALLOWED`. This could cause confusion for the caller and may lead to incorrect behavior in their code.

Recommendation: The `executeInitReserve` function should be updated to check if the reserves list is full before checking if the reserve has already been added. This will ensure that the correct error message is returned in all cases.

Here is an example of how the code could be updated:
```
function executeInitReserve(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    DataTypes.InitReserveParams memory params
) external returns (bool) {
    // Check if the reserves list is full
    if (params.reservesCount >= params.maxNumberReserves) {
        // Return the correct error message
        require(false, Errors.NO_MORE_RESERVES_ALLOWED);
    }

    // Check if the reserve has already been added
    bool reserveAlreadyAdded = reservesData[params.asset].id != 0 ||
        reservesList[0] == params.asset;
    require(!reserveAlreadyAdded, Errors.RESERVE_ALREADY_ADDED);

    // ...
}
```
# --> 12th report for : SupplyLogic.sol https://github.com/para-space/paraspace-core/blob/main/contracts/protocol/libraries/logic/SupplyLogic.sol

## 1. Unrestricted Contract Calling in `SupplyLogic` Library

Summary: The SupplyLogic library contains a vulnerability that allows any contract to call any of its functions. This vulnerability allows a malicious contract to call functions in the SupplyLogic library and potentially manipulate the state of the library's contracts.

Impact: This vulnerability can be exploited by a malicious contract to call functions in the SupplyLogic library and potentially manipulate the state of the library's contracts. This could allow the attacker to steal assets or disrupt the normal operation of the SupplyLogic library.

Recommendation: The SupplyLogic library should be updated to restrict which contracts are allowed to call its functions. This can be done by adding a `require` statement at the beginning of each function that checks the caller's address and only allows known, trusted contracts to call the function.

## 2. SupplyLogic: Supply and withdraw functions allow unauthorised access to user’s assets

Summary:
The `executeSupply()` and `executeWithdraw()` functions in the SupplyLogic library are vulnerable to unauthorised access.

Impact:
An attacker could exploit this vulnerability to manipulate user’s assets without authorisation. This could result in financial losses for the users and damage the reputation of the ParaSpace protocol.

Recommendation:
Add proper access controls to the `executeSupply()` and `executeWithdraw()` functions to ensure only authorised users can access them. This could be done by adding a check to verify the caller’s address against a whitelist of authorised users.

## 3. Missing SafeERC20 check in SupplyLogic

Summary: The `executeSupply` function in the SupplyLogic library does not check that the params.asset input is an instance of the `SafeERC20` contract. This allows an attacker to supply arbitrary ERC20 tokens to the ParaSpace protocol.

Impact: An attacker can supply any ERC20 token to the ParaSpace protocol, potentially allowing them to manipulate the protocol's reserves and potentially gain access to user funds.

Recommendation: The `executeSupply` function should be modified to check that the `params.asset` input is an instance of the SafeERC20 contract before executing the supply operation. This can be done by adding a check to the beginning of the function that calls the `isSafeERC20` function on the `params.asset` address.

## 4. Unchecked return value vulnerability in `SupplyLogic.executeSupply()`

Summary: The `executeSupply()` function in the SupplyLogic library does not check the return value of the `safeSupply()` call, which can result in user funds being lost.

Impact: If the `safeSupply()` call fails, the user's funds will be lost and there will be no indication that this has happened.

Recommendation: The `executeSupply()` function should check the `return` value of the `safeSupply()` `call` and `revert` if it is not successful. This will ensure that user funds are not lost in the event of a failure.

## 5. Unauthorized Access to Supply/Withdraw Functions in SupplyLogic Library

Summary:
The SupplyLogic library in the ParaSpace protocol does not check for authorization before allowing users to execute the `supply` and `withdraw` functions. This allows any user to `supply` or `withdraw` `assets` `to/from` the protocol without proper authorization, potentially leading to loss of funds.

Impact:
This vulnerability can allow unauthorized users to manipulate the supply and withdrawal of assets in the ParaSpace protocol, potentially leading to loss of funds.

Recommendation:
The `SupplyLogic` library should implement proper authorization checks to ensure that only authorized users are able to execute the `supply` and `withdraw` functions. This can be achieved by implementing access control mechanisms such as role-based access control or contract-based access control.

## 6. Uninitialized Variable Vulnerability in SupplyLogic Library

Summary: The `executeSupply()` function in the `SupplyLogic` library contains an uninitialized variable vulnerability that can allow attackers to manipulate the state of the contract. The `onBehalfOf variable` is not initialized in the function and is subsequently used in various other operations. This can allow attackers to supply arbitrary values to the contract.

Impact: The vulnerability allows attackers to manipulate the state of the contract by supplying arbitrary values. This can lead to incorrect balances, incorrect collateralization ratios, and other unexpected behavior in the contract.

Recommendation: The `onBehalfOf variable` should be initialized with a `default` value in the `executeSupply()` function. This will prevent attackers from supplying arbitrary values and ensure the correct operation of the contract.

## 7. SupplyLogic Library `executeSupply()` Function Unchecked Return Value

Summary:

The `executeSupply()` function in the `SupplyLogic` library has an unchecked `return` value vulnerability. The `executeSupply()` function does not check the `return` value of the `reserve.updateState()` function, which could result in unexpected behavior.

Impact:

If the `reserve.updateState()` function returns a false value, it indicates that the supply operation has failed. However, the `executeSupply()` function does not check this `return` value and continues to execute, potentially causing unintended consequences.

Recommendation:

The `executeSupply()` function should check the `return` value of the `reserve.updateState()` function and return if it is false. This will ensure that the supply operation only continues if it has been successful.

## 8. Incorrect Parameter Validation in SupplyLogic `executeSupply` Function

Summary:

The `SupplyLogic.executeSupply` function does not properly validate the params `input`, allowing an attacker to supply tokens to the protocol with a referral code of `0x0000`.

Impact:

An attacker could exploit this vulnerability to supply tokens to the protocol without applying any referral rewards, potentially allowing the attacker to gain an unfair advantage over other users.

Recommendation:

The `SupplyLogic.executeSupply` function should be updated to properly validate the `params.referralCode` input, ensuring that it is a non-zero value before applying any referral rewards.

## 9. Potential Reentrancy Vulnerability in SupplyLogic Library

Summary:
The SupplyLogic library in the ParaSpace Protocol contract contains a potential reentrancy vulnerability in the `executeSupply()` function. This vulnerability can be exploited by an attacker calling the `executeSupply()` function with a malicious contract as the `to` parameter, which could allow the attacker to reenter the `executeSupply()` function and execute arbitrary code.

Impact:
An attacker can exploit this vulnerability to steal funds and manipulate the state of the contract.

Recommendation:
To prevent this vulnerability, the `executeSupply()` function should be modified to use the `transfer()` or `transferFrom()` functions of the ERC20 and ERC721 contracts, which are reentrancy safe. This will prevent the attacker from being able to reenter the `executeSupply()` function and execute arbitrary code.

## 10. Incorrect Ordering of Update and Cache in SupplyLogic

Summary: In the `executeSupply()` function in the `SupplyLogic` library, the `updateState()` function is called before the `cache()` function. This means that the `reserveCache` variable being used in the `updateState()` function will not have the most up-to-date information, potentially leading to incorrect calculations and unintended behavior.

Impact: Incorrect calculations and unintended behavior in the `executeSupply()` function.

Recommendation: The `cache()` function should be called before the `updateState()` function in the `executeSupply()` function to ensure that the `reserveCache` variable has the most up-to-date information. This can be done by rearranging the lines of code as follows:
```
DataTypes.ReserveData storage reserve = reservesData[params.asset];
DataTypes.ReserveCache memory reserveCache = reserve.cache();
reserve.updateState(reserveCache, userConfig[params.user], params);
```

## 11. Improper Use of Solidity Library Functions

Summary: The code in the `executeSupply()` function uses the `withdraw()` function from the `GPv2SafeERC20` library in an improper manner. This could potentially allow an attacker to withdraw more than the intended amount of an asset.

Impact: An attacker could potentially withdraw more than the intended amount of an asset, leading to financial loss for the contract owner and users of the contract.

Recommendation: The `withdraw()` function should be used with proper checks and balances in place to ensure that the correct amount of assets are withdrawn. Additionally, the `withdraw()` function should be used in a safe and secure manner to prevent potential attacks.

## 12. Unchecked external call in `executeSupply()`

Summary: The `executeSupply()` function contains an `external` call to `supply()` in `GPv2SafeERC20` without checking the `return` value, potentially leading to user funds being transferred to an attacker-controlled contract.

Impact: This vulnerability allows an attacker to potentially steal user funds by calling `executeSupply()` and providing an attacker-controlled contract as the `_to` parameter in the params argument.

Recommendation: The return value of `the supply()` call in `executeSupply()` should be checked to ensure that the call was successful before continuing execution. Alternatively, the call to `supply()` can be removed and the logic for transferring funds can be implemented directly in `executeSupply()`.

## 13. SupplyLogic Library Integer Overflow

Summary:

The `executeSupply` function in the `SupplyLogic` library contains a vulnerability that allows an attacker to perform an integer overflow attack. This can be exploited by calling the `executeSupply` function with a large enough value for the `amount` parameter to cause the `totalSupply` and `totalBorrow` variables to overflow. This can result in the attacker gaining unauthorized access to the system and potentially stealing assets from other users.

Impact:

If exploited, this vulnerability can result in the attacker gaining unauthorized access to the system and potentially stealing assets from other users.

Recommendation:

The `totalSupply` and `totalBorrow` variables should be checked for overflow before the add operation is performed. This can be done by comparing the new value to the maximum value for a `uint256` variable, and throwing an error if the new value would exceed this maximum. This will prevent the integer overflow from occurring and protect the system from potential attacks.

## 14. Library function abuse in SupplyLogic

Summary:
The SupplyLogic library in the above Solidity code contains a function called `executeSupply()` that accepts a `DataTypes.ExecuteSupplyParams memory` parameter. This function is not marked as `view or `pure`, which means that it can modify the contract state and execute arbitrary Solidity code. An attacker could call this function from another contract and pass in malicious code that would be executed in the context of the SupplyLogic contract, allowing them to compromise the contract and potentially steal funds.

Impact:
An attacker could exploit this vulnerability to execute arbitrary code in the context of the SupplyLogic contract, potentially allowing them to steal funds or manipulate the contract state in other malicious ways.

Recommendation:
To fix this vulnerability, the executeSupply() function should be marked as view or pure to prevent it from modifying the contract state or executing arbitrary code. Alternatively, the function could be moved to a contract that is only intended to be called by trusted parties.

## 15. Potential Integer Overflow in SupplyLogic Library

Summary: The `executeSupply()` function in the `SupplyLogic` library has a potential integer overflow vulnerability in its calculation of `referralRewardWei` and `totalSupply` variables.

Impact: An attacker can exploit this vulnerability by calling the `executeSupply()` function with malicious input values, causing an integer overflow and potentially disrupting the expected behavior of the function.

Recommendation: To fix this issue, the `referralRewardWei` and `totalSupply` variables should be declared as `uint256` instead of `uint128` to prevent integer overflow. Additionally, the calculation of these variables should use the `safeAdd()` method from the `WadRayMath` library to ensure that the result is not overflowed.

## 16. Unsafe Solidity Library SupplyLogic

Summary: The SupplyLogic library in the code allows for unsafe solidity libraries to be called. This could lead to security vulnerabilities in the contract and allows for malicious users to exploit the contract.

Impact: Unsafe solidity libraries can be called, leading to security vulnerabilities in the contract and allowing for malicious users to exploit the contract.

Recommendation: Replace the using keyword with the import keyword to import safe libraries. This will prevent unsafe libraries from being called and will prevent security vulnerabilities in the contract.

## 17.  SupplyLogic Library - `executeSupply()` Function Vulnerability

Summary: The `executeSupply()` function in the `SupplyLogic` library is vulnerable to reentrancy attacks. The `updateState()` function is called before the `reserveCache.totalSupply` is incremented, allowing an attacker to potentially exploit this vulnerability by calling this function recursively and increasing the `totalSupply` value.

Impact: If exploited, an attacker could potentially use this vulnerability to manipulate the total supply of an asset and potentially gain unauthorized access to funds.

Recommendation: To fix this vulnerability, the `updateState()` function should be called after the `reserveCache.totalSupply` is incremented, or the `updateState()` function should be modified to check for and prevent reentrancy attacks. Additionally, the `executeSupply()` function should be audited for other potential vulnerabilities.

## 18. SupplyLogic Library Unrestricted Supply of ERC721 Tokens

Summary:
The SupplyLogic library contains a vulnerability that allows an attacker to supply an unlimited number of ERC721 tokens to the ParaSpace protocol without any restrictions. This can be exploited by calling the `executeSupply()` function with the `fromNToken` parameter set to `true`.

Impact:
An attacker could abuse this vulnerability to supply an unlimited number of ERC721 tokens to the ParaSpace protocol, potentially leading to a loss of funds for users of the protocol.

Recommendation:
The `executeSupply()` function should be updated to properly check the `fromNToken` parameter and only allow the supply of ERC721 tokens if it is set to true. This can be done by adding a check at the beginning of the function to ensure that `fromNToken` is true before allowing the supply of ERC721 tokens.

# --> 13th report for : ValidationLogic.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

## 1. Incorrect input validation in `validateSupply` function

Summary: 
The `validateSupply` function does not properly validate the `amount` parameter, which could allow a user to supply zero or negative tokens.

Impact: 
If a user is able to supply zero or negative tokens, this could potentially cause issues with the internal state of the contract and potentially lead to incorrect calculations or other unexpected behavior.

Recommendation: 
The `validateSupply` function should be updated to check that the `amount` parameter is greater than zero before allowing the supply to proceed. This will ensure that users cannot supply zero or negative tokens, which should prevent any issues with the contract's internal state.

## 2. Supply action overflow vulnerability in ValidationLogic contract

Summary
The `validateSupply` function in the `ValidationLogic` contract has a potential vulnerability where the `amount` is not checked against the `maxTotalSupply`. This can result in an overflow of the `totalSupply` variable.

Impact
If exploited, this vulnerability can cause the `totalSupply` variable to overflow, resulting in incorrect calculations and potentially causing loss of funds.

Recommendation
To fix this vulnerability, the `amount` should be checked against the `maxTotalSupply` before the `totalSupply` is updated. This can be done by adding a check like the following before the `totalSupply` is updated:
```
require(amount <= maxTotalSupply - totalSupply, "Total supply would exceed maxTotalSupply");
```

## 3. Incorrect default value for health factor liquidation threshold

Summary: 
The contract has a constant called `HEALTH_FACTOR_LIQUIDATION_THRESHOLD` that has a default value of 1e18, which is not a realistic value for a health factor.

Impact: 
This could lead to users being liquidated without good reason.

Recommendation: 
The default value of `HEALTH_FACTOR_LIQUIDATION_THRESHOLD` should be changed to a more realistic value.

## 4.  Invalid constant value for REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD

Summary: 
The value assigned to the constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD is 0.9e4, which does not represent a valid percentage value (90% should be represented as 9e3 or 9e4).

Impact: 
This bug can cause incorrect calculations related to the REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD constant, potentially leading to incorrect behavior in the contract.

Recommendation: 
Change the value assigned to the REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD constant to 9e3 or 9e4 to correctly represent 90%. Alternatively, the constant can be defined as a decimal value (e.g. 0.9) to avoid confusion.

## 5. Low Minimum Health Factor for Liquidation

Summary:
The `ValidationLogic` library defines the `MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD` constant as 0.95, which is a relatively low value compared to the default `HEALTH_FACTOR_LIQUIDATION_THRESHOLD` of 1. This means that positions with a health factor below 0.95 will be considered for liquidation.

Impact: 
A low minimum health factor for liquidation can result in positions being liquidated unnecessarily, leading to losses for users.

Recommendation: 
Consider increasing the `MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD` constant to a higher value, such as 0.99, to avoid unnecessary liquidation of user positions. Alternatively, consider implementing additional checks to ensure that liquidation is only performed when necessary.

## 6. Potential Memory Leak

Summary: 
The contract includes a `ReserveLogic.validateSupply` function that accepts a `reserveCache` parameter but never uses it, potentially leading to a memory leak.

Impact: 
In the worst-case scenario, an attacker could repeatedly call this function and cause a significant amount of unused data to be stored in the contract's memory, potentially causing the contract to run out of storage space. This could lead to the contract becoming unresponsive or to the attacker gaining unauthorized access to the contract's resources.

Recommendation: 
Remove the `reserveCache` parameter from the `validateSupply` function. If the parameter is necessary for the function to work correctly, it should be used within the body of the function.

## 7. Incorrect use of the `onlyOwner` modifier in the `cancel` function

Summary:
In the `cancel` function, the `onlyOwner` modifier is used to prevent non-owners from canceling the sale. However, the `onlyOwner` modifier is used incorrectly as it is placed at the beginning of the function, instead of before the `require` statement that checks if the sale has started. This means that the `onlyOwner` modifier has no effect, and any address can call the `cancel` function.

Impact: 
This vulnerability allows any address to cancel the sale, even if they are not the owner of the contract. This can allow an attacker to disrupt the sale and potentially steal funds from the sale receiver if the sale has already started.

Recommendation: 
The `onlyOwner` modifier should be placed before the `require` statement that checks if the sale has started. This will prevent non-owners from canceling the sale.

# 14th report for : DefaultReserveAuctionStrategy.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

## 1. Incorrect use of signed integer type

Summary: 
The contract `DefaultReserveAuctionStrategy` uses signed integer types `int256` in a number of places where unsigned types `uint256` should be used instead. This can cause unexpected behavior and potentially result in vulnerabilities.

Impact: 
Using signed integer types in place of unsigned types can result in the contract behaving in unexpected ways. For example, if a negative value is assigned to a signed integer variable, it will be interpreted as a very large positive value when treated as an unsigned integer. This can lead to incorrect calculations and potentially result in vulnerabilities if not handled properly.

Recommendation: 
Replace all instances of `int256` with `uint256` in the contract `DefaultReserveAuctionStrategy`. It is generally a good practice to use unsigned integer types for arithmetic operations in Solidity, as this can help prevent potential vulnerabilities and unintended behavior.

## 2. `ticks` variable is not initialized in the `calculateAuctionPriceMultiplier`

Summary:
The contract `DefaultReserveAuctionStrategy` contains a vulnerability in the `calculateAuctionPriceMultiplier` function, where the `ticks` variable is not initialized with a default value. If the contract is called and `currentTimestamp` is less than `auctionStartTimestamp`, `ticks` will be uninitialized, leading to an undefined behavior.

Impact
If the `currentTimestamp` is less than `auctionStartTimestamp`, the `ticks` variable will be uninitialized. This can cause unpredictable behavior and potentially lead to security vulnerabilities.

Recommendation
The `ticks` variable should be initialized with a default value in the `calculateAuctionPriceMultiplier` function to avoid any potential undefined behavior. For example, `uint256 ticks = 0`; should be added at the beginning of the function.

## 3. Incorrect Price Multiplier Calculation

Summary: 
The `_calculateAuctionPriceMultiplierByTicks` function in the `DefaultReserveAuctionStrategy` contract does not correctly calculate the auction price multiplier. Specifically, the `if` statement in the function incorrectly calculates the minimum value for the auction price multiplier. As a result, the calculated auction price multiplier may be too low, leading to incorrect prices being used in auctions.

Impact: 
If the calculated auction price multiplier is too low, users may be able to purchase auctioned positions at prices that are lower than they should be. This could result in users obtaining positions at prices that are below the fair market value of the positions, potentially leading to financial losses for other users.

Recommendation: 
The `if` statement in the `_calculateAuctionPriceMultiplierByTicks` function should be updated to correctly calculate the minimum value for the auction price multiplier. This will ensure that the calculated auction price multiplier is accurate and that auctions use the correct prices.

## 4. Integer Overflow in calculation of auction price multiplier

Summary:
The function `_calculateAuctionPriceMultiplierByTicks` in `DefaultReserveAuctionStrategy` contract contains an integer overflow vulnerability in the calculation of the auction price multiplier.

Impact:
An attacker could exploit this vulnerability by providing a large value for `ticks`, causing the result of the calculation to overflow, resulting in an incorrect auction price multiplier. This could result in incorrect allocation of funds and could lead to loss of funds for users participating in the auction.

Recommendation:
The calculation should be refactored to use safer arithmetic operations that prevent integer overflows. For example, the `SafeMath` library provided by OpenZeppelin can be used to perform arithmetic operations in a safe and secure manner.

## 5. Insufficient validation of input parameters in the `calculateAuctionPriceMultiplier function

Summary: 
The `calculateAuctionPriceMultiplier` function in the `DefaultReserveAuctionStrategy` contract does not validate that the `auctionStartTimestamp` parameter is not greater than the `currentTimestamp` parameter. This can allow attackers to manipulate the contract's state by passing a higher value for `auctionStartTimestamp`, leading to unexpected behavior.

Impact: 
If an attacker is able to pass a higher value for `auctionStartTimestamp` than `currentTimestamp`, they can cause the contract to enter an infinite loop. This can lead to a denial of service attack and can potentially cause the contract to run out of gas and become inoperable.

Recommendation: 
The `calculateAuctionPriceMultiplier` function should validate that the `auctionStartTimestamp` parameter is not greater than the `currentTimestamp` parameter. This can be done by adding a simple check at the beginning of the function that throws an error if the `auctionStartTimestamp` parameter is greater than the `currentTimestamp` parameter. This will ensure that the contract is able to handle invalid input and prevent attackers from manipulating its state.

## 6. Integer Overflow in DefaultReserveAuctionStrategy contract

Summary:
The contract contains an integer overflow vulnerability in the `_calculateAuctionPriceMultiplierByTicks` function. The vulnerability occurs when the `ticks` parameter is equal to or greater than the value of `PRBMath.SCALE` and the `ticksMinExp` value is greater than `2^256-1`. In this case, the `exponent` value will be greater than `2^256-1` which will cause an integer overflow when it is assigned to the `multiplier` variable.

Impact:
An attacker can exploit this vulnerability to manipulate the multiplier value and cause the contract to behave unpredictably. This can potentially lead to loss of funds or other security issues.

Recommendation:
To fix this issue, the contract should be modified to check for integer overflow in the `exponent` value before assigning it to the `multiplier` variable. This can be done by comparing the `exponent` value to the maximum possible value of `2^256-1` and returning a default value if it is greater.

# 15th report for : PoolApeStaking.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

## 1. Lack of access control in `withdrawApeCoin()` function

Summary:
The `withdrawApeCoin()` function does not check that the caller has permission to withdraw ApeCoin from the specified NFT reserve. This allows any contract or user to call the function and withdraw ApeCoin without permission.

Impact:
The lack of access control in the `withdrawApeCoin()` function allows any contract or user to call the function and withdraw ApeCoin without permission. This can lead to unauthorized withdrawals and potentially cause issues in the contract.

Recommendation:
The `withdrawApeCoin()` function should check that the caller has permission to withdraw ApeCoin from the specified NFT reserve before allowing the withdrawal. This can be done by checking the caller's address against a list of authorized addresses or by using a contract that manages access control.

## 2. Unchecked return value in `withdrawApeCoin()` function

Summary:
The `withdrawApeCoin()` function does not check the return value of the `INTokenApeStaking(nftReserve.xTokenAddress).withdrawApeCoin()` function. If this function returns a false value, the entire calculation will be false and the function will return a false value.

Impact:
If the `INTokenApeStaking(nftReserve.xTokenAddress).withdrawApeCoin()` function returns a false value, the entire calculation in the `withdrawApeCoin()` function will be false and the function will return a false value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `INTokenApeStaking(nftReserve.xTokenAddress).withdrawApeCoin()` function should be checked before performing the calculation in the `withdrawApeCoin()` function. If the return value is false, the calculation should be skipped and an error should be thrown.

## 3. Unchecked return value in `getUserHf()` function

Summary:
The `getUserHf()` function does not check the return value of the `_calculateHealthFactor()` function. If the `_calculateHealthFactor()` function returns a zero value, the entire calculation will be zero and the `getUserHf()` function will return a zero value.

Impact:
If the `_calculateHealthFactor()` function returns a zero value, the entire calculation in the `getUserHf()` function will be zero and the function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `_calculateHealthFactor()` function should be checked before performing the calculation in the `getUserHf()` function. If the return value is zero, the calculation should be skipped and an error should be thrown.

## 4. Reentrancy vulnerability in `withdrawApeCoin()` function

Summary:
The `withdrawApeCoin()` function in the `PoolApeStaking` contract is not protected against reentrancy attacks. The function calls the `INTokenApeStaking.withdrawApeCoin()` function, which can be called by an attacker to reenter the `withdrawApeCoin()` function and potentially manipulate its state.

Impact:
An attacker can use the reentrancy vulnerability in the `withdrawApeCoin()` function to manipulate the state of the contract and potentially steal funds.

Recommendation:
The `withdrawApeCoin()` function should be protected against reentrancy attacks by using the `ParaReentrancyGuard` contract or by implementing a mutex or other reentrancy protection mechanism.

# 16th report for : PoolCore.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol

## 1. Unchecked return value in `updateLastSUpdateTimestamp()` function

Summary:
The `updateLastSUpdateTimestamp()` function does not check the return value of the `poolStorage()` function. If the `poolStorage()` function returns a zero value, the entire calculation will be zero and the function will return a zero value.

Impact:
If the `poolStorage()` function returns a zero value, the entire calculation in the `updateLastSUpdateTimestamp()` function will be zero and the function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `poolStorage()` function should be checked before performing the calculation in the `updateLastSUpdateTimestamp()` function. If the return value is zero, the calculation should be skipped and an error should be thrown.

## 2. Unchecked return value in `addReserve()` function

Summary:
The `addReserve()` function does not check the return value of the `reserveLogic.isReserveCompatible()` function. If the `reserveLogic.isReserveCompatible()` function returns a false value, the contract will still add the reserve to the pool.

Impact:
If the `reserveLogic.isReserveCompatible()` function returns a false value, the contract will still add the reserve to the pool, potentially causing errors or unexpected behavior.

Recommendation:
The return value of the `reserveLogic.isReserveCompatible()` function should be checked before adding the reserve to the pool. If the return value is false, the contract should not add the reserve and an error should be thrown.

## 3. Incorrect type used in `getAvailableToLiquidate()` function

Summary:
The `getAvailableToLiquidate()` function uses the `SafeERC20.balanceOf()` function to retrieve the user's balance of the specified token. However, the `SafeERC20.balanceOf()` function expects a `address` type as its input, but the function passes in a `uint256` type.

Impact:
This will cause the `SafeERC20.balanceOf()` function to throw an error and the `getAvailableToLiquidate()` function will not be able to retrieve the user's balance of the specified token. This will prevent the user from being able to liquidate their position and can cause errors in the contract.

Recommendation:
The correct type should be used as the input for the `SafeERC20.balanceOf()` function in the `getAvailableToLiquidate()` function. The `address` type should be used instead of the `uint256` type.

## 4. Unchecked return value in `isUserValid()` function

Summary:
The `isUserValid()` function does not check the return value of the `_acl.isUserValid()` function. If the `_acl.isUserValid()` function returns a zero value, the entire calculation will be zero and the function will return a zero value.

Impact:
If the `_acl.isUserValid()` function returns a zero value, the entire calculation in the `isUserValid()` function will be zero and the function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `_acl.isUserValid()` function should be checked before performing the calculation in the `isUserValid()` function. If the return value is zero, the calculation should be skipped and an error should be thrown.

## 5. Unchecked return value in `_canUpdateCollateral()` function

Summary:
The `_canUpdateCollateral()` function does not check the return value of the `getReserveData()` function. If the `getReserveData()` function returns a zero value, the entire calculation will be zero and the function will return a zero value.

Impact:
If the `getReserveData()` function returns a zero value, the entire calculation in the `_canUpdateCollateral()` function will be zero and the function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `getReserveData()` function should be checked before performing the calculation in the `_canUpdateCollateral()` function. If the return value is zero, the calculation should be skipped and an error should be thrown.

## 6. Unchecked return value in `init()` function

Summary:
The `init()` function does not check the return value of the `getReserveConfiguration()` function. If the `getReserveConfiguration()` function returns a zero value, the entire calculation will be zero and the function will return a zero value.

Impact:
If the `getReserveConfiguration()` function returns a zero value, the entire calculation in the `init()` function will be zero and the function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `getReserveConfiguration()` function should be checked before performing the calculation in the `init()` function. If the return value is zero, the calculation should be skipped and an error should be thrown.

## 7. Unchecked return value in `isReserveStopped()` function

Summary:
The `isReserveStopped()` function does not check the return value of the `getReserveData()` function. If the `getReserveData()` function returns a zero value, the entire calculation will be zero and the function will return a zero value.

Impact:
If the `getReserveData()` function returns a zero value, the entire calculation in the `isReserveStopped()` function will be zero and the function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The return value of the `getReserveData()` function should be checked before performing the calculation in the `isReserveStopped()` function. If the return value is zero, the calculation should be skipped and an error should be thrown.

## 8. Uninitialized variable in `getReserve()` function

Summary:
The `getReserve()` function does not initialize the `reserveData` variable before using it in the calculation. If the `reserveData` variable is not initialized, the function will return a zero value.

Impact:
If the `reserveData` variable is not initialized, the `getReserve()` function will return a zero value. This can lead to incorrect calculations and potentially cause errors in the contract.

Recommendation:
The `reserveData` variable should be initialized before using it in the calculation in the `getReserve()` function. This can be done by setting `reserveData` equal to the `_reserves[reserveAddress]` property of the `poolStorage()` function.

## 9. Uninitialized storage variable in `init()` function

Summary:
The `init()` function does not initialize the `_reserves` storage variable before adding elements to it. As a result, the first element added to the `_reserves` mapping will overwrite the default zero value and potentially cause errors in the contract.

Impact:
If the `_reserves` mapping is not initialized before adding elements to it, the first element added will overwrite the default zero value and potentially cause errors in the contract.

Recommendation:
The `_reserves` mapping should be initialized with a default value before adding elements to it in the `init()` function. This will ensure that the default zero value is not overwritten and prevent potential errors in the contract.

# 17th report for : PoolMarketplace.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

## 1. Lack of Input Validation in PoolMarketplace contract

Summary: 
The PoolMarketplace contract does not properly validate input parameters in its buyWithCredit and acceptBidWithCredit functions, which can allow attackers to manipulate these functions and potentially cause harm to the contract and its users.

Impact: 
This vulnerability can potentially allow attackers to manipulate the contract's internal state and cause harm to its users.

Recommendation: 
Proper input validation should be implemented in the contract to ensure that the input parameters are valid and within the expected range. This can be done by adding require statements to check the validity of the input parameters before they are used in the contract's internal logic.

## 2. Misuse of SafeERC20

Summary: 
The code is using the SafeERC20 contract, which is designed to provide a safe interface for interacting with ERC20 tokens, as a base contract for the IERC20WithPermit interface. This is a misuse of the contract, as the IERC20WithPermit interface is not intended to be used with SafeERC20.

Impact: 
This misuse of the SafeERC20 contract can result in unexpected behavior and potential vulnerabilities in the contract, as the IERC20WithPermit interface is not designed to be used with SafeERC20.

Recommendation: 
The contract should not use SafeERC20 as a base contract for the IERC20WithPermit interface. Instead, it should use the IERC20 interface as the base contract for IERC20WithPermit, as this is the intended use of the interface.

## 3. Unrestricted Access to ADDRESSES_PROVIDER

Summary: 
The contract has a public immutable variable "ADDRESSES_PROVIDER" that holds the address of the "PoolAddressesProvider" contract. This variable is not restricted and can be accessed by any external contract. This can lead to security vulnerabilities as the external contracts can manipulate the internal state of the "PoolAddressesProvider" contract.

Impact: 
An attacker can access and manipulate the internal state of the "PoolAddressesProvider" contract which can lead to various security vulnerabilities.

Recommendation: 
The "ADDRESSES_PROVIDER" variable should be made private and only accessible to the contract functions. Access to this variable should be restricted to prevent external contracts from accessing and manipulating it.

## 4. Uninitialized Variable in PoolMarketplace Contract
Summary: 
The `PoolMarketplace` contract has an uninitialized `ADDRESSES_PROVIDER` variable, which could potentially be used in operations and cause unexpected behavior.
Impact: 
The uninitialized variable could potentially cause incorrect operation in functions that depend on it, resulting in potential loss of funds for users of the contract.
Recommendation: 
Initialize the `ADDRESSES_PROVIDER` variable in the constructor. This can be done by adding the following line to the constructor: `ADDRESSES_PROVIDER = provider;`

## 5. Reentrancy Vulnerability in PoolMarketplace Contract

Summary:
The PoolMarketplace contract contains a reentrancy vulnerability in the buyWithCredit and acceptBidWithCredit functions. These functions call the transferFrom function of the ERC20 token contract, which could be exploited by an attacker to reenter the contract and manipulate its internal state.

Impact:
An attacker could exploit this vulnerability to steal funds from the contract or manipulate its internal state.

Recommendation:
The contract should be updated to use the transferFrom function of the ERC20WithPermit contract instead of the ERC20 contract, which provides protection against reentrancy attacks. Additionally, the contract should use the require statement to ensure that the transferFrom function is only called once per function call.

## 6. Unchecked return value vulnerability

Summary: 
In the `acceptBidWithCredit` function of the `PoolMarketplace` contract, the return value of the `_getAuctionByID` function is not checked before being used. This could allow an attacker to manipulate the return value and cause the contract to behave unexpectedly.

Impact: 
If an attacker is able to manipulate the return value of the `_getAuctionByID` function, they could potentially cause the contract to execute incorrect or unintended operations, leading to loss of funds or other damage.

Recommendation: 
The return value of the `_getAuctionByID` function should be checked before being used, and an error should be thrown if the return value is not as expected. This can be done by adding an if statement to check the return value before using it, or by using the `require` keyword to ensure that the return value is valid.

## 7. Outdated Solidity version

Summary: 
The code is using an outdated version of Solidity (0.8.10) which is vulnerable to potential attacks.

Impact: 
Using an outdated version of Solidity can leave the contract open to potential attacks.

Recommendation: 
It is recommended to update the code to use the latest version of Solidity. This will ensure the contract is secure and not vulnerable to potential attacks.

## 8. Unsafe math operations

Summary
The contract uses unsafe math operations in the _calculateAuctionPriceMultiplierByTicks function, which can result in arithmetic overflows and underflows.

Impact
This vulnerability can lead to incorrect calculations and unpredictable behavior of the contract.

Recommendation:
Use the SafeMath library or similar libraries to perform arithmetic operations in a safe and predictable manner.

## 9. Lack of proper access controls for the `buyWithCredit` and `acceptBidWithCredit` functions in the `PoolMarketplace` contract.

Summary:

The `buyWithCredit` and `acceptBidWithCredit` functions in the `PoolMarketplace` contract do not have any access controls to prevent unauthorized users from calling these functions. This allows anyone to call these functions and perform transactions on the marketplace without any restrictions.

Impact:

This vulnerability allows anyone to call the `buyWithCredit` and `acceptBidWithCredit` functions and perform transactions on the marketplace without any authorization. This can lead to unauthorized purchases and other malicious actions on the marketplace.

Recommendation:

It is recommended to implement proper access controls for the `buyWithCredit` and `acceptBidWithCredit` functions in the `PoolMarketplace` contract. This can be done by adding a modifier to these functions that checks if the caller has the necessary permissions to perform the actions. This can be done by checking the caller's address against a list of authorized addresses or by checking if the caller has the required roles.`

## 10. Reentrancy Vulnerability in `PoolMarketplace` contract

Summary:
The `PoolMarketplace` contract is vulnerable to reentrancy attacks due to the lack of proper checks and balances in the `_batchBuyWithCredit` and `_batchAcceptBidWithCredit` functions. An attacker can repeatedly call these functions, resulting in an infinite loop which can lead to contract exhaustion and loss of funds.

Impact:
If exploited, this vulnerability can lead to contract exhaustion and loss of funds for the users of the contract.

Recommendation:
The `_batchBuyWithCredit` and `_batchAcceptBidWithCredit` functions should be modified to include proper checks and balances to prevent reentrancy attacks. This can be done by using the `require` statement to check if the contract has already been called by the attacker, and if so, to exit the function immediately. Additionally, the use of the `ParaReentrancyGuard` library can also help prevent reentrancy attacks.

## 11. Potential Integer Overflow in _getTotalReserve() Function

Summary:
The _getTotalReserve() function in the contract contains a potential integer overflow vulnerability. The totalReserve variable is calculated by adding the reserve of each token in the reserveTokens array. However, this calculation does not check for overflows, which can lead to incorrect calculations and potentially security vulnerabilities.

Impact:
An attacker could potentially exploit this vulnerability by adding large numbers to the reserveTokens array, causing the totalReserve variable to overflow and resulting in incorrect calculations. This could have negative consequences for users and the contract itself.

Recommendation:
It is recommended to add a check for integer overflows in the _getTotalReserve() function, to prevent this vulnerability from being exploited. This can be done by using the SafeMath library or similar methods to ensure that the calculations are performed safely and correctly.

## 12. Incorrect Access Control

Summary: 
The `buyWithCredit` and `acceptBidWithCredit` functions in the `PoolMarketplace` contract do not check the caller's permission to execute these actions. This allows any user to perform these actions without the proper authorization.

Impact: 
This vulnerability allows unauthorized users to buy and accept bids with credit, potentially leading to financial loss for the contract owner or other users.

Recommendation: 
Add proper access control checks to the `buyWithCredit` and `acceptBidWithCredit` functions to ensure that only authorized users can perform these actions. This can be done by calling the `ACLManager `contract's `checkPermission` function and checking the return value to ensure that the caller has the required permission.

## 13. Integer Overflow/Underflow in PoolMarketplace contract

Summary: 
The `PoolMarketplace` contract contains a potential integer overflow/underflow vulnerability in the `buyWithCredit` function. The function calculates the amount of credit to be transferred by multiplying the `reservePrice` and `quantity` arguments. If the product of these two values is greater than the maximum allowed value of a `uint256` variable, an overflow will occur.

Impact: 
An attacker could exploit this vulnerability to cause an overflow and transfer a large amount of credit to their own address, resulting in financial loss to the contract owner.

Recommendation: 
It is recommended to add a check to prevent overflow in the `buyWithCredit` function by comparing the product of `reservePrice` and `quantity` with the maximum allowed value of a `uint256` variable before performing the multiplication. This will ensure that the calculation is performed safely and prevent potential overflows.

# 18th report for : PoolParameters.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol

## 1. Callstack Limit Exceeded

Summary: 
The contract contains a recursive function `_getReserveDataByReserve` that can lead to a callstack limit exceeded exception.

Impact: 
This vulnerability can cause the contract to crash, leading to a denial of service attack.

Recommendation: 
The function should be rewritten to use an iterative approach instead of recursive. The loop can be implemented using a for or while loop to avoid the callstack limit exceeded exception.

## 2. Integer Overflow/Underflow Vulnerability in PoolParameters contract

Summary: 
The contract is vulnerable to an integer overflow/underflow attack as it does not properly check for arithmetic overflows/underflows.

Impact: 
An attacker can exploit this vulnerability to perform arbitrary arithmetic operations and potentially cause loss of funds or malicious behavior.

Recommendation: 
The contract should be updated to include proper checks for arithmetic overflows/underflows. This can be achieved by using the `SafeMath` library provided by OpenZeppelin.

## 3. Improper Access Control in PoolParameters contract

Summary: 
The `PoolParameters` contract has an improper access control vulnerability. The `onlyPoolAdmin` and `onlyPoolConfigurator` modifiers are supposed to be used to restrict access to certain functions to only the `PoolConfigurator` or `PoolAdmin` contracts. However, the `onlyPoolConfigurator` modifier is missing a `require` statement to check that the caller is the `PoolConfigurator`, and the `onlyPoolAdmin` modifier is missing a `require` statement to check that the caller is the `PoolAdmin`. This allows any contract or address to call these restricted functions.

Impact: 
This vulnerability allows any contract or address to call the restricted functions in the `PoolParameters` contract, potentially leading to unauthorized modifications of the contract's state.

Recommendation: 
The `onlyPoolConfigurator` and `onlyPoolAdmin` modifiers should be updated to include `require` statements to check that the caller is the `PoolConfigurator` or `PoolAdmin` respectively. This will properly restrict access to these functions and prevent unauthorized modifications of the contract's state.

## 4. Call Stack Depth Attack

Summary:
The contract contains a potential call stack depth attack vulnerability in the `_transferFromAuction` function. The `_transferFromAuction` function is called by the `mintToTreasury` function and the `addToPool` function. The `mintToTreasury` function is called by the `mint` function and the `acceptBidWithCredit` function. The `addToPool` function is called by the `buyWithCredit` function and the `batchBuyWithCredit` function.

Impact:
The attacker can perform a call stack depth attack by calling the `mintToTreasury` function or the `addToPool` function repeatedly in a recursive manner. This can result in the contract getting stuck in an infinite loop and exhausting all the gas. This can cause the contract to become unresponsive, leading to loss of funds.

Recommendation:
To fix the issue, the `_transferFromAuction` function should be moved to a separate contract and called by the `mintToTreasury` and `addToPool` functions via a delegatecall. This will prevent the call stack depth attack by limiting the call stack depth to a fixed number.

## 5. Reentrancy Vulnerability

Summary: 
The contract contains a reentrancy vulnerability in the `mintToTreasury` function. The function can be called by anyone who has permission to call `mintToTreasury` and `transfer` functions on the `INToken` contract. This allows an attacker to call the `mintToTreasury` function repeatedly, resulting in multiple calls to the `transfer` function on the `INToken` contract. This can be exploited to steal the tokens from the contract.

Impact: 
An attacker can steal the tokens from the contract by exploiting the reentrancy vulnerability in the `mintToTreasury` function.

Recommendation: 
The contract should be modified to prevent reentrancy attacks by using a mutex or an external contract to prevent multiple calls to the `mintToTreasury` function. Alternatively, the `mintToTreasury` function can be made internal to prevent external calls.

## 6. Improper input validation in `PoolParameters` contract

Summary: 
The `PoolParameters` contract does not properly validate user input for the `setAuctionHealthFactor` function, allowing an attacker to set the auction health factor to an arbitrary value outside the expected range of [1, 2].

Impact: 
An attacker could set the auction health factor to an extremely low or high value, potentially causing liquidity issues in the market and disrupting the stability of the protocol.

Recommendation: 
The `setAuctionHealthFactor` function should be modified to properly validate user input and ensure that the auction health factor is within the expected range of [1, 2]. This can be done by adding a check to ensure that the input value is within this range before setting the internal `_auctionHealthFactor` variable.

## 7. Unrestricted modification of auction health factors

Summary:
The `MAX_AUCTION_HEALTH_FACTOR` and `MIN_AUCTION_HEALTH_FACTOR` constants are defined in the `PoolParameters` contract and are used to limit the maximum and minimum health factors that can be used in the auctions. However, these constants are not restricted and can be modified by anyone.

Impact:
Since the `MAX_AUCTION_HEALTH_FACTOR` and `MIN_AUCTION_HEALTH_FACTOR` constants are not restricted and can be modified by anyone, an attacker can change these values to manipulate the auctions. For example, the attacker could set the `MAX_AUCTION_HEALTH_FACTOR` to a very low value, which would make it easy for them to win auctions and acquire assets at very low prices.

Recommendation:
The `MAX_AUCTION_HEALTH_FACTOR` and `MIN_AUCTION_HEALTH_FACTOR` constants should be restricted and only be modifiable by authorized parties. This can be done by adding a modifier to the functions that modify these constants and checking that the caller is authorized to make changes.

## 8. Use of Uninitialized Constant Value

Summary:
The contract uses the constant value `POOL_REVISION` without initializing it. This can cause unexpected behavior in the contract.

Impact:
The contract may not function as intended if the constant value `POOL_REVISION` is not initialized. This can result in financial loss for users interacting with the contract.

Recommendation:
Initialize the constant value `POOL_REVISION` with a default value before using it in the contract. This will ensure that the contract functions as intended and avoids any potential loss for users.

## 9. Improper Access Control in ParaSpace Protocol

Summary: 
The contract code contains a vulnerability in the `onlyPoolAdmin()` and `onlyPoolConfigurator()` modifiers which allow any address to call the functions that use these modifiers. The `_onlyPoolConfigurator()` function checks if the caller is the `PoolConfigurator` contract, but does not check if the caller is the `PoolConfigurator` contract of the specific market. Similarly, the `_onlyPoolAdmin()` function checks if the caller is the `PoolAdmin` contract, but does not check if the caller is the `PoolAdmin` contract of the specific market.

Impact: 
This vulnerability allows any address to call the functions that use the `onlyPoolAdmin()` and `onlyPoolConfigurator()` modifiers, bypassing the intended access controls. This can potentially allow attackers to manipulate the contract state and cause unauthorized minting, burning, and transferring of tokens.

Recommendation: 
The `_onlyPoolConfigurator()` and `_onlyPoolAdmin()` functions should be modified to check if the caller is the `PoolConfigurator` and `PoolAdmin` contract of the specific market, instead of just checking if the caller is the `PoolConfigurator` and `PoolAdmin` contract. This can be done by using the `ADDRESSES_PROVIDER` contract to get the address of the `PoolConfigurator` and `PoolAdmin` contract of the specific market and comparing it with the caller address.

## 10. Call to ADDRESSES_PROVIDER.getPoolConfigurator() is not protected

Summary: 
The `_onlyPoolConfigurator()` function calls `ADDRESSES_PROVIDER.getPoolConfigurator()` without checking if it is non-null. This can lead to a potential vulnerability where an attacker can call a function that should only be callable by the pool configurator.

Impact: 
An attacker can call functions that should only be callable by the pool configurator, potentially allowing them to manipulate the contract's state in ways that are not intended.

Recommendation: 
Add a check for a non-null `ADDRESSES_PROVIDER` before calling `ADDRESSES_PROVIDER.getPoolConfigurator()` in the `_onlyPoolConfigurator()` function. This will ensure that the contract is not vulnerable to this issue.

# 19th report for : PoolStorage.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol

## 1. Unsafe Typecast in 'PoolStorage' contract

Summary:

The 'PoolStorage' contract contains an unsafe typecast in the definition of the 'POOL_STORAGE_POSITION' constant. The code uses the 'uint256' function to cast the result of the 'keccak256' function to a 'uint256' type, but this can cause an overflow if the result of 'keccak256' is larger than 2^256 - 1.

Impact:

If the result of 'keccak256' overflows the 'uint256' type, it can result in incorrect behavior in the contract. This can potentially lead to security vulnerabilities and loss of funds.

Recommendation:

To avoid this issue, the code should be updated to use a safe typecast instead. This can be done by using the 'bytes32' type to store the result of 'keccak256' and then using the 'uint256' type to cast the 'bytes32' value to a 'uint256' type in a safe manner. For example:
```
bytes32 constant POOL_STORAGE_POSITION =
    bytes32(keccak256("paraspace.proxy.pool.storage"));

function poolStorage()
    internal
    pure
    returns (DataTypes.PoolStorage storage rgs)
{
    uint256 position = uint256(POOL_STORAGE_POSITION);
    assembly {
        rgs.slot := position
    }
}
```

## 2. Unrestricted Access to Internal Function

Summary: 
The function `poolStorage()` in the contract `PoolStorage` has the `internal` visibility modifier, but does not restrict access to only the contract itself or its descendants. This allows any contract to call this function and potentially gain access to sensitive data stored in the `DataTypes.PoolStorage` struct.

Impact: 
An attacker could exploit this vulnerability by calling the `poolStorage()` function and gaining access to sensitive data stored in the `DataTypes.PoolStorage` struct. This could potentially lead to the loss or manipulation of important data within the contract.

Recommendation: 
Restrict access to the `poolStorage()` function by adding the only keyword to the `internal` visibility modifier. This will ensure that only the contract itself or its descendants are able to call this function and access the data stored in the `DataTypes.PoolStorage` struct.

## 3. Unsafe usage of Solidity assembly

Summary: 
The contract code contains unsafe usage of the Solidity assembly feature. The `assembly` block in the `poolStorage` function is used to retrieve the storage position of the `DataTypes.PoolStorage` contract, but it does not properly check the position for errors or exceptions, potentially leading to security vulnerabilities.

Impact: 
Unsafe usage of assembly blocks can lead to vulnerabilities such as reentrancy attacks, access to unauthorized storage slots, and contract corruption. This can result in loss of funds or other assets, as well as disruption of the contract's intended functionality.

Recommendation: 
It is recommended to avoid the use of assembly blocks in Solidity contract code whenever possible. If it is necessary to use assembly, proper error checking and exception handling should be implemented to prevent security vulnerabilities. Alternatively, a safer method for retrieving storage positions can be used, such as the `storage` keyword in Solidity 0.8.11 or higher.

# 20th report for : MintableIncentivizedERC721.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

## 1. Reentrancy Vulnerability in MintableIncentivizedERC721 Contract

Summary:
The MintableIncentivizedERC721 contract is vulnerable to reentrancy attacks due to the use of the ReentrancyGuard contract. The ReentrancyGuard contract uses a mutex lock to prevent reentrancy, but this can be bypassed by calling external contracts that also call back into the MintableIncentivizedERC721 contract before the lock is released.

Impact:
If an attacker is able to exploit this vulnerability, they could potentially perform multiple actions within the contract in a single transaction, leading to financial losses for users of the contract.

Recommendation:
It is recommended to use a more robust method for preventing reentrancy attacks, such as using the `require` statement with a `whenNotPaused` modifier to prevent external contract calls during critical sections of code. Alternatively, consider using the `RevertReason` contract provided by OpenZeppelin to handle reentrancy attacks in a more secure manner.

## 2. Insecure Library Import in MintableIncentivizedERC721 Contract

Summary:
The MintableIncentivizedERC721 contract imports several external libraries, including the Openzeppelin libraries and the ICollateralizableERC721 and IAuctionableERC721 interfaces. These libraries and interfaces are not checked for security vulnerabilities, which could allow attackers to exploit vulnerabilities in these libraries to gain access to sensitive information or manipulate the contract's functions.

Impact:
If an attacker is able to exploit a vulnerability in one of the imported libraries or interfaces, they could potentially gain access to sensitive information or manipulate the contract's functions, leading to financial losses for users of the contract.

Recommendation:
It is recommended to perform thorough security audits on any external libraries or interfaces imported into the MintableIncentivizedERC721 contract to ensure that they are secure and do not contain vulnerabilities that could be exploited by attackers. Additionally, consider implementing security measures such as access controls or limits on the contract's functions to prevent unauthorized access or manipulation.

## 3. Unrestricted Access to MintableIncentivizedERC721 Contract Functions

Summary:
The MintableIncentivizedERC721 contract contains several functions that are marked with the `onlyPool` and `onlyPoolAdmin` modifiers, which are intended to restrict access to these functions to only the pool or pool admin. However, these modifiers are not properly implemented and do not provide any protection against unauthorized access.

Impact:
Due to the lack of proper access controls, any external contract or user can call the functions marked with the `onlyPool` and `onlyPoolAdmin` modifiers. This could allow attackers to manipulate the contract's functions and potentially cause financial losses for users of the contract.

Recommendation:
It is recommended to properly implement the `onlyPool` and `onlyPoolAdmin` modifiers to restrict access to these functions to only the intended parties. This can be done by using the `require()` function to check the caller's address against the address of the pool or pool admin. Additionally, consider implementing additional security measures such as access controls or limits on the contract's functions to prevent unauthorized access or manipulation.

## 4. Reentrancy Vulnerability in MintableIncentivizedERC721 Contract

Summary:
The MintableIncentivizedERC721 contract includes the ReentrancyGuard library, which is intended to prevent reentrancy attacks. However, this library is not used consistently throughout the contract, leaving some functions vulnerable to reentrancy attacks.

Impact:
If an attacker is able to successfully exploit this vulnerability, they could potentially manipulate the contract's functions to gain unauthorized access or control over assets within the contract, leading to financial losses for users.

Recommendation:
It is recommended to consistently use the ReentrancyGuard library on all functions within the MintableIncentivizedERC721 contract to prevent potential reentrancy attacks. Additionally, consider performing a security audit on the contract to identify and address any other potential vulnerabilities.

# 21th report for : NToken.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol

## 1. Unrestricted Contract Deployment in NToken Contract

Summary:
The NToken contract allows for the deployment of new contracts without any restrictions, which could potentially allow attackers to deploy malicious contracts that could compromise the security of the system.

Impact:
Unrestricted contract deployment can allow attackers to deploy malicious contracts that can steal user funds, manipulate the contract's functions, or gain access to sensitive information. This can lead to financial losses for users of the contract.

Recommendation:
It is recommended to implement access controls or restrictions on contract deployment to prevent unauthorized or malicious contract deployments. This can help protect users from potential attacks and ensure the security of the system.

## 2. Insecure External Library Import in NToken Contract

Summary:
The NToken contract imports several external libraries, including the Openzeppelin contracts library. These libraries are not checked for security vulnerabilities, which could allow attackers to exploit vulnerabilities in these libraries to gain access to sensitive information or manipulate the contract's functions.

Impact:
If an attacker is able to exploit a vulnerability in one of the imported libraries, they could potentially gain access to sensitive information or manipulate the contract's functions, leading to financial losses for users of the contract.

Recommendation:
It is recommended to perform thorough security audits on any external libraries imported into the NToken contract to ensure that they are secure and do not contain vulnerabilities that could be exploited by attackers. Additionally, consider implementing security measures such as access controls or limits on the contract's functions to prevent unauthorized access or manipulation.

# 22th report for : NTokenApeStaking.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

## 1. Insecure External Function Calls in NTokenApeStaking Contract

Summary:
The NTokenApeStaking contract makes several external function calls to the ApeCoinStaking contract, which is not checked for security vulnerabilities. This could allow attackers to exploit vulnerabilities in the ApeCoinStaking contract to gain access to sensitive information or manipulate the NTokenApeStaking contract's functions.

Impact:
If an attacker is able to exploit a vulnerability in the ApeCoinStaking contract, they could potentially gain access to sensitive information or manipulate the NTokenApeStaking contract's functions, leading to financial losses for users of the contract.

Recommendation:
It is recommended to perform thorough security audits on any external contracts that the NTokenApeStaking contract calls, to ensure that they are secure and do not contain vulnerabilities that could be exploited by attackers. Additionally, consider implementing security measures such as access controls or limits on the contract's functions to prevent unauthorized access or manipulation.

## 2. Potential Reentrancy Vulnerability in NTokenApeStaking Contract

Summary:
The NTokenApeStaking contract contains a potential reentrancy vulnerability in the _transfer function. The function calls the ApeStakingLogic.executeUnstakePositionAndRepay function, which could be called multiple times if the contract is attacked by a reentrant contract. This could potentially allow attackers to manipulate the contract's state and gain access to sensitive information or funds.

Impact:
If an attacker is able to exploit the reentrancy vulnerability in the _transfer function, they could potentially manipulate the contract's state and gain access to sensitive information or funds. This could lead to financial losses for users of the contract.

Recommendation:
It is recommended to implement a reentrancy guard or similar security measure in the _transfer function to prevent attackers from exploiting the potential vulnerability. Additionally, consider performing a thorough security audit on the contract to identify and address any other potential vulnerabilities.

## 3. Insecure Use of Type Max Value in NTokenApeStaking Contract

Summary:
The NTokenApeStaking contract uses the type max value in the approve function of the IERC20 contract. This can potentially cause overflow errors and result in unintended behavior.

Impact:
If an attacker is able to exploit the use of the type max value in the approve function, they could potentially cause overflow errors and manipulate the contract's functions, leading to financial losses for users of the contract.

Recommendation:
It is recommended to avoid using the type max value in the approve function and instead use a specific value that is appropriate for the intended use of the function. This will prevent potential overflow errors and ensure the contract's functions are executed as intended.

# 23th report for : NTokenBAYC.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

## 1. Poor error handling

Summary: 
The contract has poor error handling and does not handle invalid inputs or state properly.

Impact: 
This can lead to unexpected behavior and vulnerabilities such as reentrancy attacks or integer overflows.

Recommendation: 
Properly handle and validate all inputs and states in the contract to prevent unexpected behavior and vulnerabilities. Use require() and revert() statements to ensure proper error handling and prevent vulnerabilities.

## 2. Reentrancy Vulnerability in NTokenBAYC

Summary: 
The `claimBAKC` function in the NTokenBAKC contract does not implement proper reentrancy protection, allowing attackers to perform reentrancy attacks and steal funds from the contract.

Impact: 
The lack of reentrancy protection allows attackers to steal funds from the contract by calling the `claimBAKC` function in a malicious contract. This can result in significant financial losses to the contract owner and users.

Recommendation: 
The contract should implement proper reentrancy protection to prevent attackers from executing reentrancy attacks. This can be done by using a mutex lock or using the `payable` keyword in the function signature to prevent reentrancy attacks.

## 3. Use of `onlyPool` modifier in `NTokenBAKC` contract

Summary:
The `NTokenBAKC` contract uses the `onlyPool` modifier in some of its functions. The `onlyPool` modifier checks if the caller of the function is the `POOL` contract that is passed in the constructor of the contract. However, there is no such contract passed in the constructor and hence, the `POOL` contract is not initialized. As a result, the `onlyPool` check always passes, allowing any contract to call these functions.

Impact:
Since the `onlyPool` check is not effective, any contract can call the `depositBAKC`, `claimBAKC`, `withdrawBAKC`, `swapBAKC`, `depositApeCoin`, `claimApeCoin`, `withdrawApeCoin`, `stake`,` unstake`, `removeStake` and `depositERC20` functions of the `NTokenBAKC` contract. This can lead to unauthorized manipulation of the contract state and potentially result in loss of funds.

Recommendation:
The `NTokenBAKC` contract should pass the address of the `POOL` contract in the constructor and initialize it properly. The `onlyPool` modifier should then be used correctly to restrict access to these functions only to the `POOL` contract.

## 4. Improper authorization check for the `withdrawBAKC` function

Summary: 
The `withdrawBAKC` function does not properly check that the caller is authorized to execute the function. This allows any user to execute the function and potentially steal funds from the contract.

Impact: 
An attacker could use this vulnerability to steal funds from the contract by calling the `withdrawBAKC` function. This could result in financial loss for the contract owner and users of the contract.

Recommendation: 
The` withdrawBAKC` function should be updated to check that the caller is authorized to execute the function before allowing the withdrawal of funds. This can be done by checking the caller's address against a list of authorized addresses or by checking the caller's permission level using an access control library.

## 5. Insecure Fallback Function

Summary: 
The contract `NTokenBAYC` has a fallback function that can be triggered by anyone calling the contract with arbitrary data. This fallback function does not have any protective measures, and thus can be called to perform malicious actions on the contract.

Impact: 
An attacker can call this fallback function to potentially cause harm to the contract, such as transferring ownership of token or modifying state variables.

Recommendation: 
The fallback function should be removed or modified to prevent any potential harm. The contract owner should also consider implementing security measures to prevent unauthorized calls to the fallback function.

## 6. Insecure Use of User-Controlled Data for Method Calls

Summary: 
The code in the `withdrawBAKC` method of the `NTokenBAYC` contract uses user-controlled data to call the `withdrawBAKC` method of the `_apeCoinStaking` contract without proper validation, which can result in an attacker calling arbitrary methods on the `_apeCoinStaking` contract.

Impact: 
An attacker can call arbitrary methods on the `_apeCoinStaking` contract by manipulating the `_nftPairs` data passed to the `withdrawBAKC` method. This can allow the attacker to steal funds or manipulate the contract state in unintended ways.

Recommendation: 
The `withdrawBAKC` method should validate the `_nftPairs` data before calling the `withdrawBAKC` method of the `_apeCoinStaking` contract to ensure that only valid data is used for the method call. This can be done by checking the length of the `_nftPairs` array and the data within each element of the array to ensure it matches the expected format.

## 7. Reentrancy Vulnerability in NTokenBAKC.depositBAKC()

Summary:
The `NTokenBAKC.depositBAKC()` function is vulnerable to reentrancy attacks, as it calls an external contract method `_apeCoinStaking.depositBAKC()` without setting a mutex to prevent reentrancy. This allows an attacker to call `NTokenBAKC.depositBAKC()` in a recursive loop and potentially manipulate the contract state.

Impact:
An attacker can potentially manipulate the contract state, steal funds, and execute arbitrary code by exploiting the reentrancy vulnerability in `NTokenBAKC.depositBAKC()`.

Recommendation:
To fix this vulnerability, the contract developers should set a mutex (e.g. `require(!_isDepositInProgress, Errors.RECURSIVE_CALL_DETECTED);`) at the beginning of the `NTokenBAKC.depositBAKC()` function to prevent reentrancy attacks. A mutex should also be set in the external contract method `_apeCoinStaking.depositBAKC()` to prevent reentrancy attacks in that function.

# 24th report for : NTokenMAYC.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

## 1. Reentrancy vulnerability in depositApeCoin function

Summary:

The `depositApeCoin` function in the `NTokenMAYC` contract allows a reentrancy attack by calling external functions that can modify the contract state before the reentrant call completes. This can potentially allow an attacker to steal funds from the contract.

Impact:

If an attacker is able to exploit this vulnerability, they can potentially steal funds from the contract. This can lead to significant financial losses for the users of the contract.

Recommendation:

The `depositApeCoin` function should be refactored to avoid calling external functions that can modify the contract state before the reentrant call completes. This can be achieved by using the `transferAndCall` function or by using a mutex to ensure that the contract state cannot be modified during the reentrant call.

## 2. Non-Reentrancy Vulnerability in NTokenMAYC contract

Summary:
The contract `NTokenMAYC` is vulnerable to non-reentrancy attacks because it does not have the `nonReentrant` modifier on its `depositApeCoin()` and `withdrawApeCoin()` functions. This allows an attacker to call these functions multiple times in quick succession, causing the contract to execute the same code repeatedly and potentially resulting in unexpected behavior.

Impact:
If exploited, this vulnerability could result in loss of funds or other unintended consequences.

Recommendation:
To fix this vulnerability, the `nonReentrant` modifier should be added to the `depositApeCoin()` and `withdrawApeCoin()` functions. This will prevent the contract from executing the same code repeatedly, ensuring that it behaves as expected.

# 25th report for : NTokenMoonBirds.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

## 1. Reentrancy Attack on the `NTokenMoonBirds` contract

Summary:

The `NTokenMoonBirds` contract is vulnerable to a reentrancy attack in the `burn` function. The `_burnMultiple` function is called within the `burn` function, but it does not properly check if the `from` address is the caller of the function before performing the transfer. As a result, an attacker can call the `burn` function and supply a malicious contract as the `from` address. This contract can then call the `burn` function again and transfer the tokens to the attacker's address.

Impact:

This vulnerability can allow an attacker to steal tokens from the contract and transfer them to their own address.

Recommendation:

To fix this vulnerability, the `burn` function should check if the `from` address is the caller of the function before performing the transfer. This can be done by adding a check for `msg.sender == from` before calling the `_burnMultiple` function.

## 2. Unchecked return value in onERC721Received()

Summary: 
The onERC721Received() function does not check the return value of the POOL.supplyERC721() function, potentially leading to unintended behavior.

Impact: 
If the POOL.supplyERC721() function returns a non-zero value, the function will continue execution as if the transaction was successful. This could lead to incorrect state in the contract, potentially leading to vulnerabilities such as allowing unauthorized users to access contract functionality or allowing users to mint new tokens without proper checks.

Recommendation: 
The onERC721Received() function should check the return value of the POOL.supplyERC721() function and return if the transaction fails. This will ensure that the contract only continues execution if the transaction is successful, preventing any unintended behavior.

## 3. Incorrect data type used in onERC721Received function

Summary: 
In the `onERC721Received` function of the `NTokenMoonBirds` contract, the `id` parameter is declared as a `uint256` but is used as a `bytes32`. This can cause errors in the contract's execution and potentially result in loss of funds.

Impact: 
The use of incorrect data types in the `onERC721Received` function can cause the contract to behave unexpectedly, leading to potential loss of funds.

Recommendation: 
The `id` parameter in the `onERC721`Received function should be declared as a `bytes32` to match its usage in the contract. This will ensure that the contract executes as intended and prevent potential loss of funds.

# 26th report for : NTokenUniswapV3.sol https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

## 1. Unsafe Reentrancy in `NTokenUniswapV3` Contract

Summary:
The `NTokenUniswapV3` contract uses the `INonfungiblePositionManager` contract to decrease liquidity for a given token id. The `decreaseLiquidity` function of the `INonfungiblePositionManager` contract does not include a reentrancy guard, which makes it vulnerable to reentrancy attacks.

Impact:
An attacker can exploit this vulnerability by calling the decreaseLiquidity function and then calling the `burn` function of the `NTokenUniswapV3` contract. This will allow the attacker to decrease the liquidity and then immediately withdraw the funds, potentially causing loss of funds for the contract owner.

Recommendation:
To fix this vulnerability, the `decreaseLiquidity` function of the `INonfungiblePositionManager` contract should include a reentrancy guard to prevent attackers from calling the function multiple times in a single transaction. Alternatively, the burn function of the `NTokenUniswapV3` contract could be modified to prevent calling the `decreaseLiquidity` function within the same transaction.

## 2. Unsafe type cast in `NTokenUniswapV3` contract

Summary:
The `NTokenUniswapV3` contract uses the `SafeCast` library to perform a type cast from `address` to `IERC20`. However, the `SafeCast` library only performs a safe type cast from `address` to `IOneToken`, which is an interface with a subset of `IERC20` methods. As a result, calling any `IERC20` methods on the returned value from the `SafeCast` type cast will result in an error.

Impact:
This vulnerability can result in the contract being unable to perform some operations involving `IERC20` tokens, such as transferring tokens or checking their balances. This can lead to incorrect behavior and potential loss of funds.

Recommendation:
The `NTokenUniswapV3` contract should use a safe type cast library that supports casting to the `IERC20` interface, or perform its own runtime check to ensure the returned value is an instance of the `IERC20` interface before calling its methods.

## 3. Reentrancy Vulnerability in NTokenUniswapV3 contract

Summary: 
The NTokenUniswapV3 contract has a reentrancy vulnerability in the _decreaseLiquidity() function. The function calls the decreaseLiquidity() function on the INonfungiblePositionManager contract, which is an externally owned contract and can be maliciously modified to call back into the NTokenUniswapV3 contract in a reentrant manner. This can potentially allow an attacker to steal assets from the contract.

Impact: 
If exploited, this vulnerability can allow an attacker to steal assets from the contract.

Recommendation: 
The _decreaseLiquidity() function should be refactored to avoid calling external contracts in a reentrant manner. The Solidity `send` and `transfer` functions should be used instead of the `call` function to avoid reentrancy. Additionally, the `require` statement should be placed after the external call to ensure that the external contract has not been maliciously modified.