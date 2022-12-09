# Gas Report
## Finding Summary
||Issue|Instances|
|-|-|-|
|[G-01]|`uint256` Iterator Checked Each Iteration|46|
|[G-02]|`uint<size>` Used Rather Than `uint256`|14|
|[G-03]|Constants Are Not `Private`|8|
|[G-04]|&& In If Statement(s)|7|
|[G-05]|`i--`/`i-=1` Used Over `--i`|2|
|[G-06]|Tautological Check Used|1|

### [G-01] `uint256` Iterator Checked Each Iteration

A `uint256` iterator will not overflow before the check variable overflows. Unchecking the iterator increment saves gas.
**Example**
```solidity
//From
for (uint256 i; i < len; ++i) {
	//Do Something
}
//To
for (uint256 i; i < len;) {
	//Do Something
	unchecked { ++i; }
}
```

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [229](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229), [291](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291), [321](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321), [413](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413).
```solidity
229:	for (uint256 i = 0; i < _assets.length; i++) {
291:	for (uint256 i = 0; i < _assets.length; i++) {
321:	for (uint256 i = 0; i < _feeders.length; i++) {
413:	for (uint256 i = 0; i < feederSize; i++) {
```

*/paraspace-core/contracts/misc/ParaSpaceOracle.sol*
Links: [95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95), [197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197).
```solidity
95:	for (uint256 i = 0; i < assets.length; i++) {
197:	for (uint256 i = 0; i < assets.length; i++) {
```

*/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol*
Links: [193](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193), [210](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210).
```solidity
193:	for (uint256 index = 0; index < tokenIds.length; index++) {
210:	for (uint256 index = 0; index < tokenIds.length; index++) {
```

*/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol*
Links: [371](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371), [466](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466).
```solidity
371:	for (uint256 index = 0; index < totalBalance; index++) {
466:	for (uint256 index = 0; index < totalBalance; index++) {
```

*/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol*
Links: [177](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177), [284](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284), [382](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382), [470](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470).
```solidity
177:	for (uint256 i = 0; i < marketplaceIds.length; i++) {
284:	for (uint256 i = 0; i < marketplaceIds.length; i++) {
382:	for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {
470:	for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

*/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol*
Links: [104](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104).
```solidity
104:	for (uint256 i = 0; i < assets.length; i++) {
```

*/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol*
Links: [184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184), [195](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195).
```solidity
184:	for (uint256 index = 0; index < params.tokenData.length; index++) {
195:	for (uint256 index = 0; index < params.tokenData.length; index++) {
```

*/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol*
Links: [131](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131), [212](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212), [465](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465), [910](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910), [1034](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034).
```solidity
131:	for (uint256 index = 0; index < amount; index++) {
212:	for (uint256 index = 0; index < tokenIds.length; index++) {
465:	for (uint256 index = 0; index < tokenIds.length; index++) {
910:	for (uint256 index = 0; index < tokenIds.length; index++) {
1034:	for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```

*/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol*
Links: [72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72), [103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103), [138](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138), [172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172), [199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199), [215](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215), [279](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279), [289](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289), [305](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305).
```solidity
72:	for (uint256 index = 0; index < _nfts.length; index++) {
103:	for (uint256 index = 0; index < _nfts.length; index++) {
138:	for (uint256 index = 0; index < _nftPairs.length; index++) {
172:	for (uint256 index = 0; index < actualTransferAmount; index++) {
199:	for (uint256 index = 0; index < _nftPairs.length; index++) {
215:	for (uint256 index = 0; index < _nftPairs.length; index++) {
279:	for (uint256 index = 0; index < _nfts.length; index++) {
289:	for (uint256 index = 0; index < _nftPairs.length; index++) {
305:	for (uint256 index = 0; index < _nftPairs.length; index++) {
```

*/paraspace-core/contracts/protocol/pool/PoolCore.sol*
Links: [634](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634).
```solidity
634:	for (uint256 i = 0; i < reservesListCount; i++) {
```

*/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol*
Links: [493](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493).
```solidity
493:	for (uint256 index = 0; index < tokenIds.length; index++) {
```

*/paraspace-core/contracts/protocol/tokenization/NToken.sol*
Links: [106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106), [145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145).
```solidity
106:	for (uint256 index = 0; index < tokenIds.length; index++) {
145:	for (uint256 i = 0; i < ids.length; i++) {
```

*/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol*
Links: [107](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107).
```solidity
107:	for (uint256 index = 0; index < tokenIds.length; index++) {
```

*/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol*
Links: [51](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51), [97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97).
```solidity
51:	for (uint256 index = 0; index < tokenIds.length; index++) {
97:	for (uint256 index = 0; index < tokenIds.length; index++) {
```

*/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol*
Links: [207](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207), [280](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280).
```solidity
207:	for (uint256 index = 0; index < tokenData.length; index++) {
280:	for (uint256 index = 0; index < tokenIds.length; index++) {
```

*/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol*
Links: [213](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213).
```solidity
213:	for (uint256 index = 0; index < totalBalance; index++) {
```

*/paraspace-core/contracts/ui/WPunkGateway.sol*
Links: [82](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82), [109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109), [113](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113), [136](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136), [174](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174).
```solidity
82:	for (uint256 i = 0; i < punkIndexes.length; i++) {
109:	for (uint256 i = 0; i < punkIndexes.length; i++) {
113:	for (uint256 i = 0; i < punkIndexes.length; i++) {
136:	for (uint256 i = 0; i < punkIndexes.length; i++) {
174:	for (uint256 i = 0; i < punkIndexes.length; i++) {
```

### [G-02] `uint<size>` Used Rather Than `uint256`

The EVM deals with 32 bytes at a time. For `uint<size>`s less than 32 bytes the EVM performs more operations as a result of the needed size reduction from `uint256` to `uint<size>`. Consider using `uint256`.

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [9](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9), [12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12), [14](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14), [300](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L300), [312](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L312), [330](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L330).
```solidity
9:	uint8 constant MIN_ORACLES_NUM = 3;
12:	uint128 constant EXPIRATION_PERIOD = 1800;
14:	uint128 constant MAX_DEVIATION_RATE = 150;
300:	uint8 assetIndex = assetFeederMap[_asset].index;
312:	feederPositionMap[_feeder].index = uint8(feeders.length - 1);
330:	uint8 feederIndex = feederPositionMap[_feeder].index;
```

*/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol*
Links: [105](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L105), [106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L106), [173](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L173), [200](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L200), [205](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L205), [272](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272), [273](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L273), [408](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L408).
```solidity
105:	uint64 oldRecipientBalance = erc721Data.userState[to].balance;
106:	uint64 newRecipientBalance = oldRecipientBalance + 1;
173:	uint64 collateralizedBalance = erc721Data
200:	uint64 oldBalance = erc721Data.userState[to].balance;
205:	uint64 collateralizedTokens = 0;
272:	uint64 burntCollateralizedTokens = 0;
273:	uint64 balanceToBurn;
408:	uint64 balanceLimit = erc721Data.balanceLimit;
```

### [G-03] Constants Are Not `Private`

`public` constants can be made `private` to save on deployement gas. A `private` variable lets solc ignore having to generate getters. If an external user/contract would want to access the value they can check the source.

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [9](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9), [12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12), [14](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14).
```solidity
9:	uint8 constant MIN_ORACLES_NUM = 3;
12:	uint128 constant EXPIRATION_PERIOD = 1800;
14:	uint128 constant MAX_DEVIATION_RATE = 150;
```

*/paraspace-core/contracts/protocol/pool/PoolStorage.sol*
Links: [16](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16).
```solidity
16:	bytes32 constant POOL_STORAGE_POSITION =
```

*/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol*
Links: [23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23).
```solidity
23:	bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
```

*/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol*
Links: [22](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L22), [23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L23), [24](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L24).
```solidity
22:	uint256 constant BAYC_POOL_ID = 1;
23:	uint256 constant MAYC_POOL_ID = 2;
24:	uint256 constant BAKC_POOL_ID = 3;
```

### [G-04] && In If Statement(s)

Seperating if statements saves gas.
**Example**
```solidity
//From
if (a != HIGH && b != LOW) {
	//Do Something
}
//To
if (a != HIGH) {
	if (b != LOW) {
		//Do Something
	}
}
```

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [331](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L331).
```solidity
331:	if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
```

*/paraspace-core/contracts/misc/ParaSpaceOracle.sol*
Links: [130](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L130).
```solidity
130:	if (price == 0 && address(_fallbackOracle) != address(0)) {
```

*/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol*
Links: [151](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L151).
```solidity
151:	if (params.usePTokens && params.amount == type(uint256).max) {
```

*/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol*
Links: [474](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L474), [662](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L662).
```solidity
474:	if (params.from != params.to && params.amount != 0) {
662:	if (oldCollateralizedBalance == 0 && newCollateralizedBalance != 0) {
```

*/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol*
Links: [111](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L111), [145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L145).
```solidity
111:	if (from != to && erc721Data.auctions[tokenId].startTime > 0) {
145:	if (from != to && isUsedAsCollateral_) {
```

### [G-05] `i--`/`i-=1` Used Over `--i`

`--i` increments `i` directly. When using `i--`/`i-=1` solc must create a temporary value, thus resulting in higher gas.

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [443](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L443), [450](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L450).
```solidity
443:	while (pivot < arr[uint256(j)]) j--;
450:	j--;
```

### [G-06] Tautological Check Used

It should be noted that `uint8 >= 0` will always yield `true` as, by definition, an unsigned integer cannot be negative (has no sign). A `uint8 >= 0` check can be removed to save gas (make sure other checks were not supposed to occur instead). 

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [330-331](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L330-L331).
```solidity
330:	uint8 feederIndex = feederPositionMap[_feeder].index;
331:	if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
```