Th following errors where finded in the contract NFTFloorOracle.sol:

1.The use of constants for values that could change, such as MIN_ORACLES_NUM, EXPIRATION_PERIOD, and MAX_DEVIATION_RATE, can prevent the contract from being optimized and using the least amount of gas possible. Instead, you should save these values in variables and use them instead.

2.The OracleConfig structure has two fields, expirationPeriod and maxPriceDeviation, which are redundant with the fields of the same name in the PriceInformation structure. This can also prevent the contract from being optimized and using the least amount of gas possible.

3.The _addAssets() function iterates over all assets in the _assets array and calls the _addAsset() function for each of them. However, this function could be optimized using the Solidity "batch" function, which would allow multiple assets to be added at once instead of one at a time.

4.The _addFeeders() function does the same as _addAssets(), iterating over all feeders in the _feeders array and calling the _addFeeder() function for each of them. It could also be optimized using the "batch" function.

5.The _setConfig() function does not check if the value of the expirationPeriod parameter is less than the value of MIN_ORACLES_NUM, which could allow an expirationPeriod value to be set that is less than required. A verification should be added to ensure that this value is valid.