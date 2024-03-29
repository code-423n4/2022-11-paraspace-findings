# LOW

## 1. Unused/empty receive()/fallback() function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert. Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds.

```
149      receive() external payable {}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol


## 2. Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions

See the following link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

- https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps

```
19	contract WPunkGateway is
20		ReentrancyGuard,
21		IWPunkGateway,
22		IERC721Receiver,
23		OwnableUpgradeable
24	{
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol


## 3. Add constructor initializers

As per OpenZeppelin’s (OZ) [recommendation](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/6), “The guidelines are now to make it impossible for anyone to run initialize on an implementation contract, by adding an empty constructor with the initializer modifier. So the implementation contract gets initialized automatically upon deployment.”

Note that this behaviour is also incorporated the OZ Wizard since the UUPS vulnerability discovery: “Additionally, we modified the code generated by the Wizard 19 to include a constructor that automatically initializes the implementation when deployed.”

```
57	function initialize() external initializer {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol
```
75	function initialize(IPoolAddressesProvider provider)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol
```
36	function initialize(
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
```
52	function initialize(
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol
```
97	function initialize(
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol


## 4. Immutable addresses lack zero-address check

Constructors should check the address written in an immutable address variable is not the zero address.

```
51	constructor(IPoolAddressesProvider provider) {
52		ADDRESSES_PROVIDER = provider;
53	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol
```
64	constructor(IPoolAddressesProvider provider) {
65		ADDRESSES_PROVIDER = provider;
66	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol
```
62	constructor(IPoolAddressesProvider provider) {
63		ADDRESSES_PROVIDER = provider;
64	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol
```
89	constructor(IPoolAddressesProvider provider) {
90		ADDRESSES_PROVIDER = provider;
91	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol
```
32	constructor(IPool pool, address apeCoinStaking) NToken(pool, false) {
33		_apeCoinStaking = ApeCoinStaking(apeCoinStaking);
34	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
```
43	constructor(
44		address _punk,
45		address _wpunk,
46		address _pool
47	) {
48		punk = _punk;
49		wpunk = _wpunk;
50		pool = _pool;
51
52		Punk = IPunks(punk);
53		WPunk = IWrappedPunks(wpunk);
54		Pool = IPool(pool);
55	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol
```
49	constructor(
50		IPoolAddressesProvider provider,
51		address[] memory assets,
52		address[] memory sources,
53		address fallbackOracle,
54		address baseCurrency,
55		uint256 baseCurrencyUnit
56	) {
57		ADDRESSES_PROVIDER = provider;
58		BASE_CURRENCY = baseCurrency;
59		BASE_CURRENCY_UNIT = baseCurrencyUnit;
60		_setFallbackOracle(fallbackOracle);
61		_setAssetsSources(assets, sources);
62		emit BaseCurrencySet(baseCurrency, baseCurrencyUnit);
63	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol
```
23	constructor(
24		address _factory,
25		address _manager,
26		address _addressProvider
27	) {
28		UNISWAP_V3_FACTORY = IUniswapV3Factory(_factory);
29		UNISWAP_V3_POSITION_MANAGER = INonfungiblePositionManager(_manager);
30		ADDRESSES_PROVIDER = IPoolAddressesProvider(_addressProvider);
31	}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol



# NON-CRITICAL

## 1. Use a more recent version of Solidity

- Use a solidity version of at least 0.8.12 to get `string.concat()` instead of `abi.encodePacked(<str>,<str>)`
- Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions

```
All contracts in scope use version 0.8.10
```


## 2. Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.

```
238      // TODO using bit shifting for the 2^96
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
```
59	makerAsk.price, // TODO: take minPercentageToAsk into account
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol


## 3. The `nonReentrant modifier` should occur before all other modifiers

This is a best-practice to protect against reentrancy in other modifiers.

```
79	function mint(
80		address onBehalfOf,
81		DataTypes.ERC721SupplyParams[] calldata tokenData
82	) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
...
87	function burn(
88		address from,
89		address receiverOfUnderlying,
90		uint256[] calldata tokenIds
91	) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
...
119	function transferOnLiquidation(
120		address from,
121		address to,
122		uint256 value
123	) external onlyPool nonReentrant {
...
199	function transferUnderlyingTo(address target, uint256 tokenId)
200		external
201		virtual
202		override
203		onlyPool
204		nonReentrant
205	{
...
214	function handleRepayment(address user, uint256 amount)
215		external
216		virtual
217		override
218		onlyPool
219		nonReentrant
220	{
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol
```
102	function burn(
103		address from,
104		address receiverOfUnderlying,
105		uint256[] calldata tokenIds
106	) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
...
159	function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)
160		external
161		onlyPool
162		nonReentrant
163	{
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
```
26	function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
27		external
28		onlyPool
29		nonReentrant
30	{
...
39	function claimApeCoin(uint256[] calldata _nfts, address _recipient)
40		external
41		onlyPool
42		nonReentrant
43	{
...
52	function withdrawApeCoin(
53		ApeCoinStaking.SingleNft[] calldata _nfts,
54		address _recipient
55	) external onlyPool nonReentrant {
...
66	function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
67		external
68		onlyPool
69		nonReentrant
70	{
...
82	function claimBAKC(
83		ApeCoinStaking.PairNft[] calldata _nftPairs,
84		address _recipient
85	) external onlyPool nonReentrant {
...
97	function withdrawBAKC(
98		ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99		address _apeRecipient
100	) external onlyPool nonReentrant {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol
```
26	function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
27		external
28		onlyPool
29		nonReentrant
30	{
...
39	function claimApeCoin(uint256[] calldata _nfts, address _recipient)
40		external
41		onlyPool
42		nonReentrant
43	{
...
52	function withdrawApeCoin(
53		ApeCoinStaking.SingleNft[] calldata _nfts,
54		address _recipient
55	) external onlyPool nonReentrant {
...
66	function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
67		external
68		onlyPool
69		nonReentrant
70	{
...
82	function claimBAKC(
83		ApeCoinStaking.PairNft[] calldata _nftPairs,
84		address _recipient
85	) external onlyPool nonReentrant {
...
97	function withdrawBAKC(
98		ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99		address _apeRecipient
100	) external onlyPool nonReentrant {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol
```
40	function burn(
41		address from,
42		address receiverOfUnderlying,
43		uint256[] calldata tokenIds
44	) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol
```
123	function decreaseUniswapV3Liquidity(
124		address user,
125		uint256 tokenId,
126		uint128 liquidityDecrease,
127		uint256 amount0Min,
128		uint256 amount1Min,
129		bool receiveEthAsWeth
130	) external onlyPool nonReentrant {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol


## 4. State variable visibility is not set

It is best practice to set visibility for state variables.

```
18	IUniswapV3Factory immutable UNISWAP_V3_FACTORY;
19	INonfungiblePositionManager immutable UNISWAP_V3_POSITION_MANAGER;
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
```
21	ApeCoinStaking immutable _apeCoinStaking;
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
```
22	uint256 constant BAYC_POOL_ID = 1;
23	uint256 constant MAYC_POOL_ID = 2;
24	uint256 constant BAKC_POOL_ID = 3;
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
```
23	bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol
```
16	bytes32 constant POOL_STORAGE_POSITION =
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol

