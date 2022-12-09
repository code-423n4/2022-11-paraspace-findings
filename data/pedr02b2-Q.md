### Open TODOs

remove these open todo comments before deplyment

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442

### typos

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol
NFTFloorOracle.sol:53:/// aggeregate prices which are not expired from different feeders, if number of valid/unexpired prices
should be
NFTFloorOracle.sol:53:/// aggregate prices which are not expired from different feeders, if number of valid/unexpired prices

NFTFloorOracle.sol:54:/// not enough, we do not aggeregate and just use previous price
should be 
NFTFloorOracle.sol:54:/// not enough, we do not aggregate and just use previous price

NFTFloorOracle.sol:412:        //aggeregate with price from all feeders
should be 
NFTFloorOracle.sol:412:        //aggregate with price from all feeders

NFTFloorOracle.sol:11://we do not accept price lags behind to much
should be 
NFTFloorOracle.sol:11://we do not accept price lags behind too much

### Constant/immutable variable naming convention

Variable names that consist of all capital letters should be reserved for constant/immutable variables

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

DefaultReserveAuctionStrategy.sol:26:    uint256 internal immutable _maxPriceMultiplier;
DefaultReserveAuctionStrategy.sol:31:    uint256 internal immutable _minExpPriceMultiplier;
DefaultReserveAuctionStrategy.sol:36:    uint256 internal immutable _minPriceMultiplier;
DefaultReserveAuctionStrategy.sol:41:    uint256 internal immutable _stepLinear;
DefaultReserveAuctionStrategy.sol:46:    uint256 internal immutable _stepExp;
DefaultReserveAuctionStrategy.sol:48:    uint256 internal immutable _tickLength;

### Declare public function external

public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents' functions and change the visibility from external to public.

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol
NFTFloorOracle.sol:261:    function getFeederSize() public view returns (uint256) {

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
UniswapV3OracleWrapper.sol:115:    ) public pure returns (uint256 token0Amount, uint256 token1Amount) {
UniswapV3OracleWrapper.sol:146:    ) public view returns (uint256 token0Amount, uint256 token1Amount) {
UniswapV3OracleWrapper.sol:156:    function getTokenPrice(uint256 tokenId) public view returns (uint256) {

### TYPO on IERC20 import which will hinder deployment

Currently if this is not picked up during the audit anything that relies on the logic of this import on the IERC20.sol will fail or revert and require the contract to be fixed before deployment, it is possible it may actually need fixing before deployment as it my not edploy atall due to the broken import

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L4

The import has a typo which will simply not allow the file to be imported correctly due to a double "//" at the end of the import statement "contracts//IERC20.sol"; which should of course be "contracts/IERC20.sol"; (double forward slash not required).

## Tools Used
static code audit

## Recommended Mitigation Steps

Fix the typo on the file import so that the files logic can be used within this contract before deployment, if by chance or pure fluke this is not picked up during audit it could lead to unexpected results and lead to the need of a redeployment of the contracts once fixed or hinder the contract being deployed atall

### Use require() over revert()

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L70

One potential issue with this function is the use of the revert() function in the getBidOrderInfo() function to throw an error if this function is called. This can cause issues with contract execution and gas consumption, as all gas used in the transaction will be consumed but not refunded. It is recommended to use the require() function instead of revert() in such cases, as it allows for a more controlled and efficient handling of errors.
To mitigate this issue, the revert() call in getBidOrderInfo() can be replaced with a require() call that checks if the function should be executed and reverts if it should not, as shown below:


function getBidOrderInfo( bytes memory /*params*/ ) external pure override returns (DataTypes.OrderInfo memory) {
 // Check if the function should be executed and revert if not 
require(false, Errors.CALL_MARKETPLACE_FAILED); } 

### Lack off access control on function 
The following function allows for feed er to be removed from the contract by the owner, only there is a lack of access control to ensure it is actually owner removing the feeder, to mitigate thi simply add an _onlyOwner access control to the function to stop just anyone from calling it and creating ptoentially unwanted outcomes.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167

Another potential issue with the contract is that it uses a fixed threshold of feeders to aggregate prices for a given asset. However, if the number of feeders changes, this threshold may no longer be appropriate. For example, if the number of feeders decreases, it may be difficult to reach the required threshold and the price for an asset may not be updated.
To mitigate this issue, the contract can be updated to use a dynamic threshold for aggregating prices. This can be done by introducing a new parameter to the _setAssetData function that specifies the number of feeders that have provided a price for a given asset. The function can then use this parameter to determine the appropriate threshold for aggregating prices.

### unimplemented function modifier

The _onlyAssetListingOrPoolAdmins function is not implemented, but it is used as a modifier on the setAssetSources and setFallbackOracle functions. This means that anyone can call these functions, which should only be accessible to the asset listing or pool admins. Implement the _onlyAssetListingOrPoolAdmins function and use it to restrict access to the setAssetSources and setFallbackOracle functions.


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L218

### Lack of 0 checks on position manger contracts

The contract uses the UNISWAP_V3_FACTORY and UNISWAP_V3_POSITION_MANAGER constants to interact with the Uniswap V3 factory and position manager contracts, respectively. These constants are initialized with the addresses of the corresponding contracts in the constructor. It is important to ensure that the correct addresses are provided and that the contracts at those addresses have the expected behavior.

Also add the address(0) check as part of the function to ensure no 0 address can be provided

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23

###  Any contract can use auction controls

the executeStartAuction and executeEndAuction functions use the AuctionLocalVars struct to store temporary variables. This struct is defined within the contract, but it is not marked as public, internal, or private, which means it is accessible to any contract that uses the AuctionLogic library. This could potentially allow other contracts to access or modify these temporary variables, which could lead to unintended behavior or security vulnerabilities.
To mitigate this issue, the AuctionLocalVars struct should be marked as private to ensure that it is only accessible within the AuctionLogic contract. This would prevent other contracts from accessing or modifying these variables, which would reduce the risk of unintended behavior or security vulnerabilities.
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L41
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L101




