# \[G-01\] USING STORAGE INSTEAD OF MEMORY FOR STRUCTS/ARRAYS SAVES GAS -3

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct.

File: LooksRareAdapter.sol


OfferItem\[\] memory offer = new OfferItem[](/Applications/Joplin.app/Contents/Resources/app.asar/1 "1");

ConsiderationItem\[\] memory consideration = new ConsiderationItem[](/Applications/Joplin.app/Contents/Resources/app.asar/1 "1");

Lines of code:

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L37

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L47

# \[G‑02\] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are
CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

Almost all of the functions in this file have this issue.

File: PoolAddressesProvider.sol

function setMarketId(string memory newMarketId)
external
override
onlyOwner
{

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56

function setAddress(bytes32 id, address newAddress)
external
override
onlyOwner
{

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70

function setAddressAsProxy(bytes32 id, address newImplementationAddress)
external
override
onlyOwner
{

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L81

function updatePoolImpl(
IParaProxy.ProxyImplementation\[\] calldata implementationParams,
address _init,
bytes calldata _calldata
) external override onlyOwner {

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L105

function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
external
override
onlyOwner
{

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L121

# \[G-03\] Splitting require() statements that use && saves gas

See this issue which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by 3 gas:

File: SeaportAdapter.sol

require(
// NOT criteria based and must be basic order
advancedOrders.length == 2 `&&`
isBasicOrder(advancedOrders\[0\]) `&&`
isBasicOrder(advancedOrders\[1\]),
Errors.INVALID\_MARKETPLACE\_ORDER
);

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L71

# [G-04] ADDRESS MAPPINGS CAN BE COMBINED IN A SINGLE MAPPING
Combining mappings of address into a single mapping of address to a struct can save a Gssset (20000 gas) operation per mapping combined. This also makes it cheaper for functions reading and writing several of these mappings by saving a Gkeccak256 operation- 30 gas.

Combine the structs to a single one and create only one mapping from address to the newly created struct.

File: NFTFloorOracle.sol

mapping(address => PriceInformation) public assetPriceMap;
mapping(address => FeederPosition) private feederPositionMap;
mapping(address => FeederRegistrar) public assetFeederMap;


Lines of code:

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L74

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L81

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L88


# [G-06] x += y COSTS MORE GAS THAN x= x + y FOR STATE VARIABLES

Using the addition operator instead of plus-equals saves 113 gas

File: UniswapV3OracleWrapper.sol

token0Amount += positionData.tokensOwed0;
token1Amount += positionData.tokensOwed1;

sum += getTokenPrice(tokenIds[index]);


Lines of code:

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L149

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L150

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L211


File: GenericLogic.sol

totalValue += tokenPrice;
totalLTV += tmpLTV * tokenPrice;
totalValue += _getTokenPrice(

Lines of code:

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L479

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L496

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L380

# [G-07] Using `calldata` instead of `memory` for read-only arguments in EXTERNAL FUNCTIONS SAVES GAS - 2

### When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * &lt;mem_array&gt;.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it’s still valid for implementation contracs to use calldata arguments instead.

File: PoolApeStaking.sol

function withdrawBAKC(
        address nftAsset,
        ApeCoinStaking.PairNftWithAmount[] `memory` _nftPairs
    ) external nonReentrant {

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L122
