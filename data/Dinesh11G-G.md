### 1st BUG
Don't Initialize Variables with Default Value

#### Impact
Issue Information: Uninitialized variables are assigned with the types default value.

Explicitly initializing a variable with it's default value costs unnecessary gas.

Example
🤦 Bad:

uint256 x = 0;
bool y = false;
🚀 Good:

uint256 x;
bool y;

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::77 => for (uint256 i = 0; i < length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::77 => for (uint256 i = 0; i < length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::173 => for (uint256 i = 0; i < orderNonces.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::93 => for (uint256 i = 0; i < accounts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::209 => for (uint256 i = 0; i < ids.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::300 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::356 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::58 => for (uint256 i = 0; i < proof.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::71 => for (uint256 i = 0; i < proof.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::131 => uint256 leafPos = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::132 => uint256 hashPos = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::133 => uint256 proofPos = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::139 => for (uint256 i = 0; i < totalHashes; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::177 => uint256 leafPos = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::178 => uint256 hashPos = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::179 => uint256 proofPos = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::185 => for (uint256 i = 0; i < totalHashes; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::44 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::112 => for (uint256 i = 0; i < totalStandardTransfers; ) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::175 => for (uint256 i = 0; i < totalStandardTransfers; ) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::124 => for (uint256 i = 0; i < numTransfers; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::147 => for (uint256 i = 0; i < numTransfers; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::170 => for (uint256 j = 0; j < numItemsInTransfer; ++j) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::937 => for (uint256 i = 0; i < totalAdditionalRecipients; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::1058 => for (uint256 i = 0; i < totalAdditionalRecipients; ) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::54 => for (uint256 i = 0; i < totalCriteriaResolvers; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::168 => for (uint256 i = 0; i < totalAdvancedOrders; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::186 => for (uint256 j = 0; j < totalItems; ++j) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::201 => for (uint256 j = 0; j < totalItems; ++j) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::211 => for (uint256 i = 0; i < totalOrders; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::276 => for (uint256 j = 0; j < totalOfferItems; ++j) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::331 => for (uint256 j = 0; j < totalConsiderationItems; ++j) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::411 => for (uint256 i = 0; i < totalOrders; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::511 => uint256 totalFilteredExecutions = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::514 => for (uint256 i = 0; i < totalOfferFulfillments; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::540 => for (uint256 i = 0; i < totalConsiderationFulfillments; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::620 => for (uint256 i = 0; i < totalOrders; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::644 => for (uint256 j = 0; j < totalConsiderationItems; ++j) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::670 => for (uint256 i = 0; i < totalExecutions; ) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::801 => uint256 totalFilteredExecutions = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::804 => for (uint256 i = 0; i < totalFulfillments; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::214 => for (uint256 i = 0; i < totalOfferItems; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::306 => for (uint256 i = 0; i < totalConsiderationItems; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::463 => for (uint256 i = 0; i < totalOrders; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::324 => for (uint256 i = 0; i < totalOrders; ) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::406 => for (uint256 i = 0; i < totalOrders; ) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::399 => for (uint256 i = 0; i < secondsAgos.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/TickMath.sol::110 => uint256 msb = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::363 => for (uint256 i = 0; i < secondsAgos.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TickMath.sol::104 => uint256 msb = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::48 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::61 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::76 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::90 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::103 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::111 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::100 => for (uint256 i = 0; i < toAdd.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::104 => for (uint256 i = 0; i < toRemove.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::115 => for (uint256 i = 0; i < toAdd.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::119 => for (uint256 i = 0; i < toRemove.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::137 => for (uint256 i = 0; i < itemHashes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::165 => for (uint256 i = 0; i < input.orders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::169 => for (uint256 i = 0; i < input.details.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::251 => uint256 nativeAmount = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::327 => bool firstBid = false;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::471 => for (uint256 i = 0; i < src.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::562 => for (uint256 i = 0; i < sd.fees.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::570 => uint256 total = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::572 => for(uint256 i = 0; i < nftCount; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::581 => uint256 total = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::584 => for(uint256 i = 0; i < nftCount; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::593 => for(uint256 i = 0; i < nftCount; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::619 => uint256 offset = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::623 => for(uint256 i = 0; i < baycStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::628 => for(uint256 i = 0; i < maycStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::633 => for(uint256 i = 0; i < bakcStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::638 => for(uint256 i = 0; i < splitStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::651 => uint256 tokenId = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::701 => uint256 offset = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::702 => for(uint256 i = 0; i < baycSplitStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::707 => for(uint256 i = 0; i < maycSplitStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::720 => for(uint256 i = 0; i < nftContracts[_mainPoolId].balanceOf(_address); ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::749 => for(uint256 i = 0; i < nftCount; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::771 => for(uint256 i = 0; i < nftCount; ++i) {
2022-11-paraspace/paraspace-core/contracts/deployments/ReservesSetupHelper.sol::36 => for (uint256 i = 0; i < inputParams.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::50 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::91 => for (uint256 i = 0; i < reserves.length; i++) {
193 => for (uint256 index = 0; index < tokenIds.length; index++) {
208 => uint256 sum = 0;
210 => for (uint256 index = 0; index < tokenIds.length; index++) {
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::121 => uint256 typeIndex = 0;
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::146 => for (uint256 i = 0; i < vars.airdropBalance; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::322 => for (uint256 i = 0; i < addresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::338 => for (uint256 i = 0; i < numChunks; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2307 => for (uint256 i = 0; i < numberOfTokens; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2325 => for (uint256 i = 0; i < numberOfTokens; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::1058 => for (uint256 i = 0; i < count; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::113 => for (uint256 i = 0; i < n; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::190 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1827 => for (uint256 i = 0; i < addresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1853 => for (uint256 i = 0; i < numberOfTokens; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1924 => for (uint256 i = 0; i < numberOfTokens; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::1225 => for (uint256 i = 0; i < count; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1380 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1572 => for (uint256 i = 0; i < numMutants; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::400 => uint256 value = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::436 => for (uint256 i = 0; i < quantity; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::794 => for (uint256 i = 0; i < offer.makerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::809 => for (uint256 i = 0; i < offer.takerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::900 => for (uint256 i = 0; i < makerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::904 => for (uint256 i = 0; i < takerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::939 => for (uint256 i = 0; i < count; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC721.sol::84 => for (uint256 index = 0; index < count; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::45 => for (uint256 index = 0; index < erc20Tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::51 => for (uint256 index = 0; index < erc721Tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::63 => for (uint256 index = 0; index < len; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::72 => for (uint256 index = 0; index < len; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::80 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::88 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::96 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::109 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::136 => for (uint256 index = 0; index < _mockERC20Tokens.length(); index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::143 => for (uint256 index = 0; index < _mockERC721Tokens.length(); index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::156 => for (uint256 count = 0; count < punksToken.mintValue; count++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::162 => for (uint256 index = 0; index < 10000; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2323 => uint256 i = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2379 => for (uint256 i = 0; i < tokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3351 => for (uint256 i = 0; i < n; ++i) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::85 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::681 => uint256 tokenIdsIdx = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::683 => for (uint256 i = 0; i < numMintedSoFar; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::952 => for (uint256 i = 0; i < quantity; i++) {

2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::86 => for (uint256 i = 0; i < input.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::348 => for (uint256 i = 0; i < reserves.length; i++) {

2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::58 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::85 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::146 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::214 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::237 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::304 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::89 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::262 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::267 => for (uint256 j = 0; j < size; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::284 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::289 => for (uint256 j = 0; j < size; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::370 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::418 => uint8 i = 0;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::462 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::480 => for (uint256 j = 0; j < tokenLength; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::73 => for (uint256 i = 0; i < users.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::74 => for (uint256 j = 0; j < tokens.length; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::97 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::104 => for (uint256 j = 0; j < reserves.length; j++) {
```
#### Tools used
Manual








### 2nd BUG
Cache Array Length Outside of Loop

#### Impact
Issue Information: Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

Example
🤦 Bad:

for (uint256 i = 0; i < array.length; i++) {
    // invariant: array's length is not changed
}
🚀 Good:

uint256 len = array.length
for (uint256 i = 0; i < len; i++) {
    // invariant: array's length is not changed
}

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/gnosis/contracts/GPv2SafeERC20.sol::97 => ///  0x24..0x44 |                    string length
2022-11-paraspace/paraspace-core/contracts/dependencies/gnosis/contracts/GPv2SafeERC20.sol::99 => function revertWithMessage(length, message) {
2022-11-paraspace/paraspace-core/contracts/dependencies/gnosis/contracts/GPv2SafeERC20.sol::102 => mstore(0x24, length)
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::55 => return _whitelistedCurrencies.length();
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::69 => uint256 length = size;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::71 => if (length > _whitelistedCurrencies.length() - cursor) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::72 => length = _whitelistedCurrencies.length() - cursor;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::75 => address[] memory whitelistedCurrencies = new address[](length);
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::77 => for (uint256 i = 0; i < length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::81 => return (whitelistedCurrencies, cursor + length);
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::55 => return _whitelistedStrategies.length();
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::69 => uint256 length = size;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::71 => if (length > _whitelistedStrategies.length() - cursor) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::72 => length = _whitelistedStrategies.length() - cursor;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::75 => address[] memory whitelistedStrategies = new address[](length);
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::77 => for (uint256 i = 0; i < length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::81 => return (whitelistedStrategies, cursor + length);
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::171 => require(orderNonces.length > 0, "Cancel: Cannot be empty");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::173 => for (uint256 i = 0; i < orderNonces.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::16 => // Minimum auction length in seconds
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::24 => * @param _minimumAuctionLengthInSeconds minimum auction length in seconds
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::27 => require(_minimumAuctionLengthInSeconds >= 15 minutes, "Owner: Auction length must be > 15 min");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::55 => // Underflow checks and auction length check
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::99 => * @notice Update minimum auction length (in seconds)
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::100 => * @param _minimumAuctionLengthInSeconds minimum auction length in seconds
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::104 => require(_minimumAuctionLengthInSeconds >= 15 minutes, "Owner: Auction length must be > 15 min");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::37 => // This method relies on extcodesize/address.code.length, which returns 0
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::41 => return account.code.length > 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::210 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::29 => revert("ECDSA: invalid signature length");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::58 => // Check the signature length
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::61 => if (signature.length == 65) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::74 => } else if (signature.length == 64) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::203 => // 32 is the length in bytes of hash,
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::217 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::80 => * - `accounts` and `ids` must have the same length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::89 => require(accounts.length == ids.length, "ERC1155: accounts and ids length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::91 => uint256[] memory batchBalances = new uint256[](accounts.length);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::93 => for (uint256 i = 0; i < accounts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::202 => require(ids.length == amounts.length, "ERC1155: ids and amounts length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::209 => for (uint256 i = 0; i < ids.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::283 => * - `ids` and `amounts` must have the same length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::294 => require(ids.length == amounts.length, "ERC1155: ids and amounts length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::300 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::342 => * - `ids` and `amounts` must have the same length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::350 => require(ids.length == amounts.length, "ERC1155: ids and amounts length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::356 => for (uint256 i = 0; i < ids.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::390 => * transfers, the length of the `id` and `amount` arrays will be 1.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::401 => * - `ids` and `amounts` have the same, non-zero length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::131 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::210 => //solhint-disable-next-line max-line-length
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::482 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::63 => return _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::123 => uint256 length = ERC721.balanceOf(to);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::124 => _ownedTokens[to][length] = tokenId;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::125 => _ownedTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::133 => _allTokensIndex[tokenId] = _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::176 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::56 => // The value is stored at length-1, but we add 1 to all indexes
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::58 => set._indexes[value] = set._values.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::82 => uint256 lastIndex = set._values.length - 1;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::119 => function _length(Set storage set) private view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::120 => return set._values.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::131 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::187 => function length(Bytes32Set storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::188 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::199 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::255 => function length(AddressSet storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::256 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::267 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::320 => function length(UintSet storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::321 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/EnumerableSet.sol::332 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155.sol::74 => * - `accounts` and `ids` must have the same length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155.sol::130 => * - `ids` and `amounts` must have the same length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::46 => * @param ids An array containing ids of each token being transferred (order and length must match values array)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::47 => * @param values An array containing amounts of each token being transferred (order and length must match ids array)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::58 => for (uint256 i = 0; i < proof.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::71 => for (uint256 i = 0; i < proof.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::122 => uint256 leavesLen = leaves.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::123 => uint256 totalHashes = proofFlags.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::126 => require(leavesLen + proof.length - 1 == totalHashes, "MerkleProof: invalid multiproof");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::168 => uint256 leavesLen = leaves.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::169 => uint256 totalHashes = proofFlags.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::172 => require(leavesLen + proof.length - 1 == totalHashes, "MerkleProof: invalid multiproof");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::111 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::44 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::46 => length++;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::49 => return toHexString(value, length);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::53 => * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::55 => function toHexString(uint256 value, uint256 length)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::60 => bytes memory buffer = new bytes(2 * length + 2);
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::63 => for (uint256 i = 2 * length + 1; i > 1; --i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Strings.sol::67 => require(value == 0, "Strings: hex length insufficient");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::37 => // This method relies on extcodesize/address.code.length, which returns 0
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::41 => return account.code.length > 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::177 => if (returndata.length == 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::208 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol::27 => if (_data.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::111 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/UpgradeabilityProxy.sol::26 => if (_data.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::56 => sload(keccak256(ChannelKey_channel_ptr, ChannelKey_length))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::66 => revert(ChannelClosed_error_ptr, ChannelClosed_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::109 => uint256 totalStandardTransfers = transfers.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::172 => uint256 totalStandardTransfers = standardTransfers.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::152 => // Add new open channel length to associated mapping as index + 1.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::154 => conduitProperties.channels.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::167 => // Use length of channels array to determine index of last channel.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::168 => uint256 finalChannelIndex = conduitProperties.channels.length - 1;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::426 => totalChannels = _conduits[conduit].channels.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::449 => uint256 totalChannels = _conduits[conduit].channels.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/lib/ConduitConstants.sol::10 => uint256 constant ChannelClosed_error_length = 0x24;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/lib/ConduitConstants.sol::18 => uint256 constant ChannelKey_length = 0x40;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::99 => uint256 numTransfers = transfers.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::131 => sumOfItemsAcrossAllTransfers += transfer.items.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::135 => // Declare a new array in memory with length totalItems to populate with
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::162 => transfer.recipient.code.length != 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::166 => uint256 numItemsInTransfer = transferItems.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::243 => mload(add(data, 0x20)), // Data begins after length offset.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::249 => // the correct length and matches an expected custom error selector.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::251 => data.length == 4 &&
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/TokenTransferrerErrors.sol::92 => *      different lengths for ids and amounts arrays.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::41 => *      array length on a given set of order parameters is not less than the
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::42 => *      original consideration array length for that order and to retrieve
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::53 => // Ensure supplied consideration array length is not less than original.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::55 => orderParameters.consideration.length,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::69 => *      array length for an order to be fulfilled is not less than the
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::70 => *      original consideration array length for that order.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::81 => // Ensure supplied consideration array length is not less than original.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::119 => * 3. Signature offset == 0x260 + (recipients.length * 0x40)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::144 => // Additional recipients length at calldata 0x264.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::146 => BasicOrder_additionalRecipients_length_cdPtr
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::148 => // Each additional recipient has a length of 0x40.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::332 => // Ensure supplied consideration array length is not less than original.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::334 => parameters.additionalRecipients.length,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::357 => *   - END_ARR + 0x120: length of ReceivedItem array
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::414 => // Get the length of the additional recipients array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::416 => BasicOrder_additionalRecipients_length_cdPtr
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::419 => // Calculate pointer to length of OrderFulfilled consideration
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::422 => OrderFulfilled_consideration_length_baseOffset,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::426 => // Set the length of the consideration array to the number of
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::433 => BasicOrder_additionalRecipients_length_cdPtr
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::486 => // Read length of the additionalRecipients array from calldata
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::597 => // Overwrite length to length of the additionalRecipients array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::599 => BasicOrder_additionalRecipients_length_cdPtr
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::712 => OrderFulfilled_offer_length_baseOffset,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::715 => BasicOrder_additionalRecipients_length_cdPtr
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::721 => // Set a length of 1 for the offer array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::835 => *  - 0x120: 1 + recipients.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::843 => calldataload(BasicOrder_additionalRecipients_length_cdPtr),
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::872 => calldataload(BasicOrder_additionalRecipients_length_cdPtr),
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::932 => uint256 totalAdditionalRecipients = additionalRecipients.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::1054 => parameters.additionalRecipients.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Consideration.sol::504 => order.consideration.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::100 => // Name is right padded, so it touches the length which is left
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::12 => *        - Note that the length of an array is separate from and precedes the
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::38 => // Name is right padded, so it touches the length which is left padded. This
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::45 => uint256 constant Version_length = 3;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::66 => // Store the same constant in an abbreviated format for a line length fix.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::81 => uint256 constant Panic_error_length = 0x24;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::146 => *  - 0x80: offer.length (1)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::151 => *  - 0x120: consideration.length (1 + additionalRecipients.length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::160 => // Minimum length of the OrderFulfilled event data.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::162 => // (0xa0 * additionalRecipients.length) to calculate full size of the buffer.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::170 => // (32 * additionalRecipients.length) to calculate the pointer to event data.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::172 => uint256 constant OrderFulfilled_consideration_length_baseOffset = 0x2a0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::173 => uint256 constant OrderFulfilled_offer_length_baseOffset = 0x200;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::202 => uint256 constant BasicOrder_additionalRecipients_length_cdPtr = 0x264;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::303 => uint256 constant NoContract_error_length = 0x24; // 4 + 32 == 36
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::314 => uint256 constant Create2AddressDerivation_length = 0x55;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::336 => uint256 constant Conduit_execute_ConduitTransfer_length = 0x01;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::339 => uint256 constant Conduit_execute_ConduitTransfer_length_ptr = 0x24;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::355 => uint256 constant Accumulator_array_length_ptr = 0x64;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::384 => uint256 constant BadSignatureV_error_length = 0x24;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::390 => uint256 constant InvalidSigner_error_length = 0x04;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::396 => uint256 constant InvalidSignature_error_length = 0x04;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::402 => uint256 constant BadContractSignature_error_length = 0x04;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol::119 => // Total length, excluding dynamic array data: 0x264 (580)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol::150 => // offer.length                          // 0x160
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::47 => // Retrieve length of criteria resolvers array and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::48 => uint256 totalCriteriaResolvers = criteriaResolvers.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::50 => // Retrieve length of orders array and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::51 => uint256 totalAdvancedOrders = advancedOrders.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::91 => if (componentIndex >= offer.length) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::121 => if (componentIndex >= consideration.length) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::182 => // Read consideration length from memory and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::183 => uint256 totalItems = orderParameters.consideration.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::197 => // Read offer length from memory and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::198 => totalItems = orderParameters.offer.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::154 => // Write the length of the ConduitTransfer array to memory.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::158 => Conduit_execute_ConduitTransfer_length_ptr
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::160 => Conduit_execute_ConduitTransfer_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::438 => if (accumulator.length != AccumulatorArmed) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::468 => // Call begins at third word; the first is length or "armed" status,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::476 => mload(add(accumulator, Accumulator_array_length_ptr)),
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::485 => // Reset accumulator length to signal that it is now "disarmed".
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::602 => // value is held in the length of the accumulator array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::603 => if (accumulator.length == AccumulatorDisarmed) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::614 => mstore(add(accumulator, Accumulator_array_length_ptr), elements)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::620 => mload(add(accumulator, Accumulator_array_length_ptr)),
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::623 => mstore(add(accumulator, Accumulator_array_length_ptr), elements)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::53 => offerComponents.length == 0 || considerationComponents.length == 0
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::164 => // Retrieve fulfillment components array length and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::166 => if (fulfillmentComponents.length == 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::242 => revert(0, Panic_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::534 => revert(0, Panic_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::44 => // Get length of original consideration array and place it on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::72 => // Load the length.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::228 => // Restore consideration item length at the counter pointer.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::278 => Create2AddressDerivation_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::327 => // Allocate a string with the intended length.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::328 => version = new string(Version_length);
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::171 => // Read length of orders array and place on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::172 => uint256 totalOrders = advancedOrders.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::177 => // Override orderHashes length to zero after memory has been allocated.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::220 => // Update the length of the orderHashes array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::241 => // Update the length of the orderHashes array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::272 => // Read length of offer array and place on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::273 => uint256 totalOfferItems = offer.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::327 => // Read length of consideration array and place on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::328 => uint256 totalConsiderationItems = consideration.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::495 => // Retrieve length of offer fulfillments array and place on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::496 => uint256 totalOfferFulfillments = offerFulfillments.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::498 => // Retrieve length of consideration fulfillments array & place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::500 => considerationFulfillments.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::569 => // reduce the total length of the executions array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::580 => if (executions.length == 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::611 => // Retrieve the length of the advanced orders array and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::612 => uint256 totalOrders = advancedOrders.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::640 => // Read length of consideration array and place on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::641 => uint256 totalConsiderationItems = consideration.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::666 => // Retrieve the length of the executions array and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::667 => uint256 totalExecutions = executions.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::763 => advancedOrders.length,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::792 => // Retrieve fulfillments array length and place on the stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::793 => uint256 totalFulfillments = fulfillments.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::827 => // reduce the total length of the executions array.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::99 => // Create an array with length 1 containing the order.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::209 => // Read offer array length from memory and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::210 => uint256 totalOfferItems = orderParameters.offer.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::299 => // Read consideration array length from memory and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::302 => .length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::455 => uint256 totalOrders = orders.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::271 => revert(0, Panic_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::320 => // Read length of the orders array from memory and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::321 => uint256 totalOrders = orders.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::349 => order.consideration.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::402 => // Read length of the orders array from memory and place on stack.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::403 => uint256 totalOrders = orders.length;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::22 => *      ERC-1271 fallback will be attempted if either the signature length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::47 => // Get the length of the signature.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::50 => // Get the pointer to the value preceding the signature length.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::60 => // Take the difference between the max ECDSA signature length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::61 => // and the actual signature length. Overflow desired for any
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::106 => // Temporarily overwrite the signature length with `v` to
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::110 => // Temporarily overwrite the word before the length with
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::131 => // Restore cached signature length.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::153 => // Temporarily overwrite the word before the signature length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::207 => revert(0, BadContractSignature_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::210 => // Check if signature length was invalid.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::214 => revert(0, InvalidSignature_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::224 => revert(0, BadSignatureV_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::229 => revert(0, InvalidSigner_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::249 => revert(0, BadContractSignature_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::64 => ERC20_transferFrom_length,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::177 => TokenTransferGenericFailure_error_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::205 => BadReturnValueFromERC20OnTransfer_error_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::212 => revert(NoContract_error_sig_ptr, NoContract_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::252 => revert(NoContract_error_sig_ptr, NoContract_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::271 => ERC721_transferFrom_length,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::342 => TokenTransferGenericFailure_error_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::380 => revert(NoContract_error_sig_ptr, NoContract_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::401 => ERC1155_safeTransferFrom_data_length_offset
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::403 => mstore(ERC1155_safeTransferFrom_data_length_ptr, 0)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::411 => ERC1155_safeTransferFrom_length,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::482 => TokenTransferGenericFailure_error_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::518 => let len := batchTransfers.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::557 => revert(NoContract_error_sig_ptr, NoContract_error_length)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::562 => add(elementPtr, ConduitBatch1155Transfer_ids_length_offset)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::567 => ConduitBatch1155Transfer_amounts_length_baseOffset,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::574 => // ids.length == amounts.length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::588 => ConduitBatch1155Transfer_ids_length_offset
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::590 => // amounts_offset == 0xc0 + ids.length*32
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::612 => Invalid1155BatchTransferEncoding_length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::627 => // that the size includes both lengths as well as the data.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::634 => BatchTransfer1155Params_ids_length_offset,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::639 => // Set the length of the data array in memory to zero.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::642 => BatchTransfer1155Params_data_length_basePtr,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::656 => BatchTransfer1155Params_ids_length_ptr,
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::657 => add(elementPtr, ConduitBatch1155Transfer_ids_length_offset),
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::667 => transferDataSize, // Location of the length of callData.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::759 => // `token` uses the same number of bytes as `data.length`.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::12 => *        - Note that the length of an array is separate from and precedes the
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::57 => uint256 constant ERC20_transferFrom_length = 0x64; // 4 + 32 * 3 == 100
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::71 => uint256 constant ERC1155_safeTransferFrom_data_length_ptr = 0xa4;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::72 => uint256 constant ERC1155_safeTransferFrom_length = 0xc4; // 4 + 32 * 6 == 196
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::73 => uint256 constant ERC1155_safeTransferFrom_data_length_offset = 0xa0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::91 => uint256 constant ERC721_transferFrom_length = 0x64; // 4 + 32 * 3 == 100
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::99 => uint256 constant NoContract_error_length = 0x24; // 4 + 32 == 36
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::115 => uint256 constant TokenTransferGenericFailure_error_length = 0xa4;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::130 => uint256 constant BadReturnValueFromERC20OnTransfer_error_length = 0x84;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::142 => uint256 constant BatchTransfer1155Params_data_length_basePtr = 0xc4;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::145 => uint256 constant BatchTransfer1155Params_ids_length_ptr = 0xc4;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::147 => uint256 constant BatchTransfer1155Params_ids_length_offset = 0xa0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::148 => uint256 constant BatchTransfer1155Params_amounts_length_baseOffset = 0xc0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::149 => uint256 constant BatchTransfer1155Params_data_length_baseOffset = 0xe0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::156 => uint256 constant ConduitBatch1155Transfer_ids_length_offset = 0xa0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::157 => uint256 constant ConduitBatch1155Transfer_amounts_length_baseOffset = 0xc0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::160 => // Note: abbreviated version of above constant to adhere to line length limit.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::164 => uint256 constant Invalid1155BatchTransferEncoding_length = 0x04;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Verifiers.sol::59 => *      ERC-1271 fallback will be attempted if either the signature length
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ZoneInteraction.sol::118 => advancedOrder.extraData.length == 0 &&
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ZoneInteraction.sol::119 => criteriaResolvers.length == 0
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::107 => if (derivedAddress.code.length != 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::7 => /// Every pool is initialized with an oracle array length of 1. Anyone can pay the SSTOREs to increase the
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::8 => /// maximum length of the oracle array. New slots will be added when the array is fully populated.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::9 => /// Observations are overwritten when the full length of the oracle array is populated.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::10 => /// The most recent observation is available, independent of the length of the oracle array, by passing 0 to observe()
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::60 => /// @return cardinalityNext The new length of the oracle array, independent of population
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::76 => /// If the index is at the end of the allowable array length (according to cardinality), and the next cardinality
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::84 => /// @param cardinalityNext The new length of the oracle array, independent of population
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::395 => tickCumulatives = new int56[](secondsAgos.length);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::397 => secondsAgos.length
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::399 => for (uint256 i = 0; i < secondsAgos.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::151 => require(success && data.length >= 32);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::165 => require(success && data.length >= 32);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::7 => /// Every pool is initialized with an oracle array length of 1. Anyone can pay the SSTOREs to increase the
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::8 => /// maximum length of the oracle array. New slots will be added when the array is fully populated.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::9 => /// Observations are overwritten when the full length of the oracle array is populated.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::10 => /// The most recent observation is available, independent of the length of the oracle array, by passing 0 to observe()
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::52 => /// @return cardinalityNext The new length of the oracle array, independent of population
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::68 => /// If the index is at the end of the allowable array length (according to cardinality), and the next cardinality
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::76 => /// @param cardinalityNext The new length of the oracle array, independent of population
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::361 => tickCumulatives = new int56[](secondsAgos.length);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::362 => secondsPerLiquidityCumulativeX128s = new uint160[](secondsAgos.length);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::363 => for (uint256 i = 0; i < secondsAgos.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TransferHelper.sol::23 => success && (data.length == 0 || abi.decode(data, (bool))),
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::48 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::61 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::76 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::90 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::103 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::111 => for (uint256 i = 0; i < pairs.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::100 => for (uint256 i = 0; i < toAdd.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::104 => for (uint256 i = 0; i < toRemove.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::115 => for (uint256 i = 0; i < toAdd.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::119 => for (uint256 i = 0; i < toRemove.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::133 => bytes32 hash = keccak256(abi.encode(itemHashes.length, itemHashes, deadline));
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::137 => for (uint256 i = 0; i < itemHashes.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::165 => for (uint256 i = 0; i < input.orders.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::169 => for (uint256 i = 0; i < input.details.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::268 => if (order.dataMask.length > 0 && detail.dataReplacement.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::468 => require(src.length == replacement.length);
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::469 => require(src.length == mask.length);
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::471 => for (uint256 i = 0; i < src.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::479 => bytes32 hash = keccak256(abi.encode(input.shared, input.details.length, input.details));
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::498 => order.items.length,
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::562 => for (uint256 i = 0; i < sd.fees.length; i++) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::433 => require(pool.timeRanges.length == 0 || _startTimestamp == pool.timeRanges[pool.timeRanges.length-1].endTimestampHour
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::480 => for(uint256 i = currentIndex; i < pool.timeRanges.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::492 => return (rewards, pool.timeRanges.length - 1);
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::506 => uint256 lastTimestampHour = pool.timeRanges[pool.timeRanges.length-1].endTimestampHour;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::534 => for(current = pool.lastRewardsRangeIndex; current < pool.timeRanges.length; ++current) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::616 => uint256 count = (baycStakes.length + maycStakes.length + bakcStakes.length + splitStakes.length + 1);
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::623 => for(uint256 i = 0; i < baycStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::628 => for(uint256 i = 0; i < maycStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::633 => for(uint256 i = 0; i < bakcStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::638 => for(uint256 i = 0; i < splitStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::702 => for(uint256 i = 0; i < baycSplitStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::707 => for(uint256 i = 0; i < maycSplitStakes.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::851 => for(uint256 i; i < _nfts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::862 => for(uint256 i; i < _nfts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::909 => for(uint256 i; i < _nfts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::919 => for(uint256 i; i < _pairs.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::951 => for(uint256 i; i < _nfts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::967 => for(uint256 i; i < _nfts.length; ++i) {
2022-11-paraspace/paraspace-core/contracts/deployments/ReservesSetupHelper.sol::36 => for (uint256 i = 0; i < inputParams.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::226 => _assets.length == _twaps.length,
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::227 => "NFTOracle: Tokens and price length differ"
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::262 => return feeders.length;
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::284 => assetFeederMap[_asset].index = uint8(assets.length - 1);

2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::312 => feederPositionMap[_feeder].index = uint8(feeders.length - 1);

2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::332 => feeders[feederIndex] = feeders[feeders.length - 1];
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::408 => //use memory here so allocate with maximum length
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::409 => uint256 feederSize = feeders.length;
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::92 => assets.length == sources.length,

2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::196 => uint256[] memory prices = new uint256[](assets.length);

2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::48 => reserves.length
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::50 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::89 => reserves.length
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::91 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::191 => uint256[] memory prices = new uint256[](tokenIds.length);

2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::66 => require(nftTokenIds.length > 0, "empty token list");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::83 => vars.airdropTokenTypes.length > 0,
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::87 => vars.airdropTokenAddresses.length == vars.airdropTokenTypes.length,
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::88 => "invalid airdrop token address length"
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::91 => vars.airdropTokenIds.length == vars.airdropTokenTypes.length,
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::92 => "invalid airdrop token id length"
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::99 => require(vars.airdropParams.length >= 4, "invalid airdrop parameters");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::122 => typeIndex < vars.airdropTokenTypes.length;
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::43 => resolvers.length == 0 && isBasicOrder(advancedOrder),


2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::73 => advancedOrders.length == 2 &&


2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::137 => advancedOrder.extraData.length == 0;
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::38 => require(runInput.details.length == 1, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::51 => require(nfts.length == 1, Errors.INVALID_MARKETPLACE_ORDER);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::319 => addresses.length == numSlots.length,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::320 => "addresses does not match numSlots length"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::322 => for (uint256 i = 0; i < addresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::826 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::894 => // The value is stored at length-1, but we add 1 to all indexes
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::896 => set._indexes[value] = set._values.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::920 => uint256 lastIndex = set._values.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::958 => function _length(Set storage set) private view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::959 => return set._values.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::970 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::978 => set._values.length > index,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1030 => function length(Bytes32Set storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1031 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1042 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1098 => function length(AddressSet storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1099 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1110 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1163 => function length(UintSet storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1164 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1175 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1253 => // The entry is stored at length-1, but we add 1 to all indexes
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1255 => map._indexes[key] = map._entries.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1279 => uint256 lastIndex = map._entries.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1317 => function _length(Map storage map) private view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1318 => return map._entries.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1329 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1337 => map._entries.length > index,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1435 => function length(UintToAddressMap storage map)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1440 => return _length(map._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1450 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1650 => return _holderTokens[owner].length();
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1703 => if (bytes(base).length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1707 => if (bytes(_tokenURI).length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1740 => // _tokenOwners are indexed by tokenIds, so .length() returns the number of tokenIds
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1741 => return _tokenOwners.length();
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1827 => //solhint-disable-next-line max-line-length
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2003 => if (bytes(_tokenURIs[tokenId]).length != 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::128 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::210 => //solhint-disable-next-line max-line-length
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::461 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::557 => return _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::617 => uint256 length = ERC721.balanceOf(to);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::618 => _ownedTokens[to][length] = tokenId;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::619 => _ownedTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::627 => _allTokensIndex[tokenId] = _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::670 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::721 => if (bytes(base).length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::725 => if (bytes(_tokenURI).length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::763 => if (bytes(_tokenURIs[tokenId]).length != 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::112 => uint256 n = addresses.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::190 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::192 => length++;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::195 => return toHexString(value, length);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::199 => * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::201 => function toHexString(uint256 value, uint256 length)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::206 => bytes memory buffer = new bytes(2 * length + 2);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::209 => for (uint256 i = 2 * length + 1; i > 1; --i) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::213 => require(value == 0, "Strings: hex length insufficient");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::803 => // This method relies on extcodesize/address.code.length, which returns 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::807 => return account.code.length > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1013 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1181 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1260 => //solhint-disable-next-line max-line-length
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1532 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1673 => return _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1733 => uint256 length = ERC721.balanceOf(to);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1734 => _ownedTokens[to][length] = tokenId;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1735 => _ownedTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1743 => _allTokensIndex[tokenId] = _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1786 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1827 => for (uint256 i = 0; i < addresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::126 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::208 => //solhint-disable-next-line max-line-length
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::459 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::559 => return _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::619 => uint256 length = ERC721.balanceOf(to);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::620 => _ownedTokens[to][length] = tokenId;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::621 => _ownedTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::629 => _allTokensIndex[tokenId] = _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::672 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::902 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::537 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::619 => //solhint-disable-next-line max-line-length
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::870 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::973 => return _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1033 => uint256 length = ERC721.balanceOf(to);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1034 => _ownedTokens[to][length] = tokenId;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1035 => _ownedTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1043 => _allTokensIndex[tokenId] = _allTokens.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1086 => uint256 lastTokenIndex = _allTokens.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1330 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1380 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1382 => length++;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1385 => return toHexString(value, length);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1389 => * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1391 => function toHexString(uint256 value, uint256 length)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1396 => bytes memory buffer = new bytes(2 * length + 2);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1399 => for (uint256 i = 2 * length + 1; i > 1; --i) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1403 => require(value == 0, "Strings: hex length insufficient");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::524 => idToOwnerIndex[_tokenId] = ownerToIds[_to].length.sub(1);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::532 => uint256 lastTokenIndex = ownerToIds[_from].length.sub(1);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::544 => return ownerToIds[_owner].length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::592 => require(_index < ownerToIds[_owner].length);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::725 => require(signature.length == 65);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::785 => makerIds.length > 0 || takerIds.length > 0,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::794 => for (uint256 i = 0; i < offer.makerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::804 => offer.takerIds.length == 0,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::809 => for (uint256 i = 0; i < offer.takerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::900 => for (uint256 i = 0; i < makerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::904 => for (uint256 i = 0; i < takerIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::45 => for (uint256 index = 0; index < erc20Tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::51 => for (uint256 index = 0; index < erc721Tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::61 => uint256 len = _mockERC20Tokens.length();
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::70 => uint256 len = _mockERC721Tokens.length();
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::80 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::88 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::96 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::109 => for (uint256 index = 0; index < _tokens.length; index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::136 => for (uint256 index = 0; index < _mockERC20Tokens.length(); index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::143 => for (uint256 index = 0; index < _mockERC721Tokens.length(); index++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::65 => // The value is stored at length-1, but we add 1 to all indexes
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::67 => set._indexes[value] = set._values.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::91 => uint256 lastIndex = set._values.length - 1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::128 => function _length(Set storage set) private view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::129 => return set._values.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::140 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::196 => function length(Bytes32Set storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::197 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::208 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::264 => function length(AddressSet storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::265 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::276 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::329 => function length(UintSet storage set) internal view returns (uint256) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::330 => return _length(set._inner);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::341 => * - `index` must be strictly less than {length}.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1234 => bytes(baseURI).length != 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1340 => if (to.code.length != 0)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1388 => if (to.code.length != 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1759 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1885 => // We will need 1 32-byte word to store the length,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1891 => // Cache the end of the memory to calculate the length later.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1915 => let length := sub(end, ptr)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1916 => // Move the pointer 32 bytes leftwards to make room for the length.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1918 => // Store the length.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1919 => mstore(ptr, length)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2306 => @return The number of redeemed claims; either 0 or tokenIds.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2315 => if (maxAllowance == 0 || tokenIds.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2324 => i < tokenIds.length; /* note increment at end */
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2339 => endSameId < tokenIds.length &&
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2358 => return tokenIds.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2367 => @return The number of redeemed claims; either 0 or tokenIds.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2375 => if (tokenIds.length == 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2379 => for (uint256 i = 0; i < tokenIds.length; i++) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2393 => return tokenIds.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2980 => return _roleMembers[role].length();
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3228 => @notice Returns the length of time, in seconds, that the Moonbird has
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3234 => @return current Zero if not currently nesting, otherwise the length of time
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3350 => uint256 n = tokenIds.length;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::85 => uint256 length = 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::87 => length++;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::90 => return toHexString(value, length);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::94 => * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::96 => function toHexString(uint256 value, uint256 length)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::101 => bytes memory buffer = new bytes(2 * length + 2);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::104 => for (uint256 i = 2 * length + 1; i > 1; --i) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::108 => require(value == 0, "Strings: hex length insufficient");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::340 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::794 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::1089 => if (reason.length == 0) {
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::115 => _addressesProvidersIndexes[provider] = _addressesProvidersList.length;
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::129 => uint256 lastIndex = _addressesProvidersList.length - 1;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Errors.sol::72 => string public constant INCONSISTENT_PARAMS_LENGTH = "76"; // 'Array parameters that should be equal length are not'

2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::172 => marketplaceIds.length == payloads.length &&
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::173 => payloads.length == credits.length,

2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::280 => marketplaceIds.length == payloads.length &&
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::281 => payloads.length == credits.length,




2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::177 => params.tokenData.length,

2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::353 => uint256 amountToWithdraw = params.tokenIds.length;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::128 => uint256 amount = params.tokenData.length;

2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::85 => implIndex < _implementationData.length;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::119 => _functionSelectors.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::131 => .length
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::139 => selectorIndex < _functionSelectors.length;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::160 => _functionSelectors.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::172 => .length
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::180 => selectorIndex < _functionSelectors.length;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::202 => _functionSelectors.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::213 => selectorIndex < _functionSelectors.length;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::233 => .implementationAddressPosition = ds.implementationAddresses.length;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::276 => .length - 1;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::301 => .length - 1;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::331 => _calldata.length == 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::336 => _calldata.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::347 => if (error.length > 0) {

2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::136 => uint256[] memory transferredTokenIds = new uint256[](_nftPairs.length);

2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::86 => for (uint256 i = 0; i < input.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::348 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::642 => // Reduces the length of the reserves array by `droppedReservesCount`

2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::176 => require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);

2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::258 => //solhint-disable-next-line max-line-length

2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::605 => return _ERC721Data.allTokens.length;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::117 => uint256 oldTotalSupply = erc721Data.allTokens.length;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::204 => uint256 oldTotalSupply = erc721Data.allTokens.length;

2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::247 => uint64 newBalance = oldBalance + uint64(tokenData.length);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::274 => uint256 oldTotalSupply = erc721Data.allTokens.length;

2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::458 => uint256 length = erc721Data.allTokens.length;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::459 => _addTokenToAllTokensEnumeration(erc721Data, tokenId, length);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::470 => uint256 length = erc721Data.allTokens.length;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::471 => _removeTokenFromAllTokensEnumeration(erc721Data, tokenId, length);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::473 => uint256 length = erc721Data.userState[to].balance;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::474 => _addTokenToOwnerEnumeration(erc721Data, to, tokenId, length);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::487 => uint256 length
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::489 => erc721Data.ownedTokens[to][length] = tokenId;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::490 => erc721Data.ownedTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::500 => uint256 length
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::502 => erc721Data.allTokensIndex[tokenId] = length;
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::547 => uint256 length
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::552 => uint256 lastTokenIndex = length - 1;
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::55 => reserves.length
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::58 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::83 => xTokenRewardAddresses.length
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::85 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::144 => vTokenRewardAddresses.length
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::146 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::211 => user != address(0) ? reserves.length : 0
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::214 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::235 => xTokenRewardAddresses.length
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::237 => for (uint256 j = 0; j < xTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::302 => vTokenRewardAddresses.length
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::304 => for (uint256 j = 0; j < vTokenRewardAddresses.length; ++j) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::87 => memory reservesData = new AggregatedReserveData[](reserves.length);
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::89 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::258 => nTokenAddresses.length
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::262 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::264 => uint256 size = tokenIds[i].length;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::281 => nTokenAddresses.length
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::284 => for (uint256 i = 0; i < nTokenAddresses.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::286 => uint256 size = tokenIds[i].length;
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::367 => user != address(0) ? reserves.length : 0
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::370 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::444 => memory tokensData = new TokenInLiquidationData[][](assets.length);
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::462 => for (uint256 i = 0; i < assets.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::463 => uint256 tokenLength = tokenIds[i].length;

2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::71 => uint256[] memory balances = new uint256[](users.length * tokens.length);
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::73 => for (uint256 i = 0; i < users.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::74 => for (uint256 j = 0; j < tokens.length; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::75 => balances[i * tokens.length + j] = balanceOf(
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::96 => address[] memory reservesWithEth = new address[](reserves.length + 1);
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::97 => for (uint256 i = 0; i < reserves.length; i++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::100 => reservesWithEth[reserves.length] = MOCK_ETH_ADDRESS;
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::102 => uint256[] memory balances = new uint256[](reservesWithEth.length);
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::104 => for (uint256 j = 0; j < reserves.length; j++) {
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::116 => balances[reserves.length] = balanceOf(user, MOCK_ETH_ADDRESS);
```
#### Tools used
Manual





### 3rd BUG
Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: 
When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::171 => require(orderNonces.length > 0, "Cancel: Cannot be empty");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::574 => require(makerOrder.amount > 0, "Order: Amount cannot be 0");
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::134 => if (x & 0x8000000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::137 => if (x & 0x4000000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::140 => if (x & 0x2000000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::143 => if (x & 0x1000000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::146 => if (x & 0x800000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::149 => if (x & 0x400000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::152 => if (x & 0x200000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::155 => if (x & 0x100000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::158 => if (x & 0x80000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::161 => if (x & 0x40000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::164 => if (x & 0x20000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::167 => if (x & 0x10000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::170 => if (x & 0x8000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::173 => if (x & 0x4000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::176 => if (x & 0x2000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::179 => if (x & 0x1000000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::182 => if (x & 0x800000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::185 => if (x & 0x400000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::188 => if (x & 0x200000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::191 => if (x & 0x100000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::194 => if (x & 0x80000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::197 => if (x & 0x40000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::200 => if (x & 0x20000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::203 => if (x & 0x10000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::206 => if (x & 0x8000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::209 => if (x & 0x4000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::212 => if (x & 0x2000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::215 => if (x & 0x1000000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::218 => if (x & 0x800000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::221 => if (x & 0x400000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::224 => if (x & 0x200000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::227 => if (x & 0x100000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::230 => if (x & 0x80000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::233 => if (x & 0x40000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::236 => if (x & 0x20000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::239 => if (x & 0x10000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::242 => if (x & 0x8000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::245 => if (x & 0x4000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::248 => if (x & 0x2000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::251 => if (x & 0x1000000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::254 => if (x & 0x800000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::257 => if (x & 0x400000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::260 => if (x & 0x200000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::263 => if (x & 0x100000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::266 => if (x & 0x80000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::269 => if (x & 0x40000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::272 => if (x & 0x20000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::275 => if (x & 0x10000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::278 => if (x & 0x8000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::281 => if (x & 0x4000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::284 => if (x & 0x2000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::287 => if (x & 0x1000 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::290 => if (x & 0x800 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::293 => if (x & 0x400 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::296 => if (x & 0x200 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::299 => if (x & 0x100 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::302 => if (x & 0x80 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::305 => if (x & 0x40 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::308 => if (x & 0x20 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::311 => if (x & 0x10 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::314 => if (x & 0x8 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::317 => if (x & 0x4 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::320 => if (x & 0x2 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::323 => if (x & 0x1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::99 => if (x > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::518 => for (int256 delta = int256(HALF_SCALE); delta > 0; delta >>= 1) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::625 => uint256 rAbs = y & 1 > 0 ? xAbs : uint256(SCALE);
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::627 => // Equivalent to "for(y /= 2; y > 0; y /= 2)" but faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::629 => for (yAux >>= 1; yAux > 0; yAux >>= 1) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::633 => if (yAux & 1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::64 => // Equivalent to "x + delta * (remainder > 0 ? 1 : 0)" but faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::147 => // Equivalent to "x - remainder * (remainder > 0 ? 1 : 0)" but faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::388 => for (uint256 delta = HALF_SCALE; delta > 0; delta >>= 1) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::457 => result = y & 1 > 0 ? x : SCALE;
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::459 => // Equivalent to "for(y /= 2; y > 0; y /= 2)" but faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::460 => for (y >>= 1; y > 0; y >>= 1) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::464 => if (y & 1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::41 => return account.code.length > 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::210 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::163 => if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::131 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::147 => if (rounding == Rounding.Up && mulmod(x, y, denominator) > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::172 => if (x >> 128 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::176 => if (x >> 64 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::180 => if (x >> 32 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::184 => if (x >> 16 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::188 => if (x >> 8 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::192 => if (x >> 4 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::196 => if (x >> 2 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::145 => if (totalHashes > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::147 => } else if (leavesLen > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::191 => if (totalHashes > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::193 => } else if (leavesLen > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::111 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::41 => return account.code.length > 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::208 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol::27 => if (_data.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::111 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/UpgradeabilityProxy.sol::26 => if (_data.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::260 => // implies that maximumFulfilled > 0.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::14 => require(x > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::56 => require(x > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::60 => if (x & type(uint128).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::65 => if (x & type(uint64).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::70 => if (x & type(uint32).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::75 => if (x & type(uint16).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::80 => if (x & type(uint8).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::85 => if (x & 0xf > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::90 => if (x & 0x3 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::95 => if (x & 0x1 > 0) r -= 1;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::35 => require(denominator > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::122 => if (mulmod(a, b, denominator) > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::50 => (liquidity > 0 ? liquidity : 1)),
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Position.sol::88 => if (tokensOwed0 > 0 || tokensOwed1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SqrtPriceMath.sol::140 => require(sqrtPX96 > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SqrtPriceMath.sol::141 => require(liquidity > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SqrtPriceMath.sol::173 => require(sqrtPX96 > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SqrtPriceMath.sol::174 => require(liquidity > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SqrtPriceMath.sol::214 => require(sqrtRatioAX96 > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/TickMath.sol::80 => if (tick > 0) ratio = type(uint256).max / ratio;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Factory.sol::75 => require(tickSpacing > 0 && tickSpacing < 16384);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::515 => require(amount > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::530 => if (amount0 > 0) balance0Before = balance0();
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::531 => if (amount1 > 0) balance1Before = balance1();
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::537 => if (amount0 > 0)
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::539 => if (amount1 > 0)
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::575 => if (amount0 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::579 => if (amount1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::617 => if (amount0 > 0 || amount1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::717 => bool exactInput = amountSpecified > 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::791 => if (cache.feeProtocol > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::798 => if (state.liquidity > 0)
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::895 => if (state.protocolFee > 0) protocolFees.token0 += state.protocolFee;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::898 => if (state.protocolFee > 0) protocolFees.token1 += state.protocolFee;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::964 => require(_liquidity > 0, "L");
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::971 => if (amount0 > 0)
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::973 => if (amount1 > 0)
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::992 => if (paid0 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::995 => if (uint128(fees0) > 0) protocolFees.token0 += uint128(fees0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::1002 => if (paid1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::1005 => if (uint128(fees1) > 0) protocolFees.token1 += uint128(fees1);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::1056 => if (amount0 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::1061 => if (amount1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::14 => require(x > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::54 => require(x > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::57 => if (x & type(uint128).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::62 => if (x & type(uint64).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::67 => if (x & type(uint32).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::72 => if (x & type(uint16).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::77 => if (x & type(uint8).max > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::82 => if (x & 0xf > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::87 => if (x & 0x3 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::92 => if (x & 0x1 > 0) r -= 1;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::34 => require(denominator > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::119 => if (mulmod(a, b, denominator) > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::43 => ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)),
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::114 => require(current > 0, "I");
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::359 => require(cardinality > 0, "I");
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Position.sol::56 => require(_self.liquidity > 0, "NP"); // disallow pokes for 0 liquidity positions
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Position.sol::85 => if (tokensOwed0 > 0 || tokensOwed1 > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SqrtPriceMath.sol::136 => require(sqrtPX96 > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SqrtPriceMath.sol::137 => require(liquidity > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SqrtPriceMath.sol::169 => require(sqrtPX96 > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SqrtPriceMath.sol::170 => require(liquidity > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SqrtPriceMath.sol::209 => require(sqrtRatioAX96 > 0);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TickMath.sol::76 => if (tick > 0) ratio = type(uint256).max / ratio;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::152 => if (input.shared.amountToWeth > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::158 => if (input.shared.amountToEth > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::186 => if (amountEth > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::268 => if (order.dataMask.length > 0 && detail.dataReplacement.length > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::365 => if (bidRefund + incentive > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::399 => if (auc.netPrice > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::525 => if (amount > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::540 => if (amount > 0) {
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::653 => uint256 unclaimed = deposited > 0 ? this.pendingRewards(0, _address, tokenId) : 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::654 => uint256 rewards24Hrs = deposited > 0 ? _estimate24HourRewards(0, _address, 0) : 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::729 => uint256 unclaimed = deposited > 0 ? this.pendingRewards(BAKC_POOL_ID, currentOwner, bakcTokenId) : 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::730 => uint256 rewards24Hrs = deposited > 0 ? _estimate24HourRewards(BAKC_POOL_ID, currentOwner, bakcTokenId): 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::765 => DashboardStake[] memory dashboardStakes = nftCount > 0 ? new DashboardStake[](nftCount) : new DashboardStake[](0);
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::774 => uint256 unclaimed = deposited > 0 ? this.pendingRewards(_poolId, _address, tokenId) : 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::775 => uint256 rewards24Hrs = deposited > 0 ? _estimate24HourRewards(_poolId, _address, tokenId): 0;
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::855 => require(position.stakedAmount > 0 || nftContracts[_poolId].ownerOf(tokenId) == msg.sender, "Token not owned by caller");

2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::66 => require(nftTokenIds.length > 0, "empty token list");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::83 => vars.airdropTokenTypes.length > 0,
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::135 => if (vars.airdropBalance > 0) {

2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::212 => require(allowlist[msg.sender] > 0, "not eligible for allowlist mint");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::497 => require(b > 0, "SafeMath: division by zero");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::514 => require(b > 0, "SafeMath: modulo by zero");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::560 => require(b > 0, errorMessage);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::584 => require(b > 0, errorMessage);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::622 => return size > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::826 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1600 => *     => 0x70a08231 ^ 0x6352211e ^ 0x095ea7b3 ^ 0x081812fc ^
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1610 => *     => 0x06fdde03 ^ 0x95d89b41 ^ 0xc87b56dd == 0x5b5e139f
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1619 => *     => 0x18160ddd ^ 0x2f745c59 ^ 0x4f6ccce7 == 0x780e9d63
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1707 => if (bytes(_tokenURI).length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::128 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::725 => if (bytes(_tokenURI).length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::881 => require(value > 0, "Counter: decrement overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::301 => require(msg.value > 0, "CryptoPunksMarket: should send eth value");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::309 => if (existing.value > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::807 => return account.code.length > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1013 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1181 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::126 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::902 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::537 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1130 => return size > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1330 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1536 => publicSaleStartTime > 0 ? block.timestamp - publicSaleStartTime : 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1540 => require(publicSaleStartTime > 0, "Public sale hasn't started yet");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1599 => bacc.balanceOf(msg.sender, serumTypeId) > 0,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1637 => require(mutantId > 0, "Invalid MEGA Mutant Id");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1653 => return megaMutationIdsByApe[apeId] > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1715 => elapsed >= publicSaleDuration && publicSaleStartTime > 0,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::54 => // assert(b > 0); // Solidity automatically throws when dividing by 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::279 => addressCheck = size > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::456 => _createVia > 0 && _createVia <= 10512,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::785 => makerIds.length > 0 || takerIds.length > 0,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::868 => if (msg.value > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC1155.sol::19 => require(id > 0, "id is zero");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::171 => if (nextPunkIndex > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::140 => return size > 0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::340 => if (returndata.length > 0) {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::638 => collectionSize_ > 0,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::641 => require(maxBatchSize_ > 0, "ERC721A: max batch size must be nonzero");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::794 => bytes(baseURI).length > 0
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::1043 => require(quantity > 0, "quantity must be nonzero");
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::861 => * @notice Convert msg.value to WETH and check if liquidationAsset is WETH (if msg.value > 0)


2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::119 => _functionSelectors.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::160 => _functionSelectors.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::202 => _functionSelectors.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::336 => _calldata.length > 0,
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::347 => if (error.length > 0) {
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::365 => require(contractSize > 0, _errorMessage);
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::44 => // Slope of the variable interest curve when usage ratio > 0 and <= OPTIMAL_USAGE_RATIO. Expressed in ray
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::79 => * @dev Its the variable rate when usage ratio > 0 and <= OPTIMAL_USAGE_RATIO
```
#### Tools used
Manual







### 4th BUG
Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: 
⚡️ Only valid for solidity versions <0.6.12 ⚡️

Access roles marked as constant results in computing the keccak256 operation each time the variable is used because assigned operations for constant variables are re-evaluated every time.

Changing the variables to immutable results in computing the hash only once on deployment, leading to gas savings.

Example
🤦 Bad:

bytes32 public constant GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");
🚀 Good:

bytes32 public immutable GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::137 => DOMAIN_SEPARATOR = keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::139 => 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::140 => 0xda9101ba92939daf4bb2e18cd5f942363b9297fbc3232c9dd964abb1fb70ed71, // keccak256("LooksRareExchange")
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::141 => 0xc89efdaa54c0f20c7adf612882df0950f5a951637e0307cdcb4c672f298b8bc6, // keccak256(bytes("1")) for versionId = 1
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyAnyItemInASetForFixedPrice.sol::47 => bytes32 node = keccak256(abi.encodePacked(takerAsk.tokenId));
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/OrderTypes.sol::9 => // keccak256("MakerOrder(bool isOrderAsk,address signer,address collection,uint256 price,uint256 tokenId,uint256 amount,address strategy,address currency,uint256 nonce,uint256 startTime,uint256 endTime,uint256 minPercentageToAsk,bytes params)")
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/OrderTypes.sol::42 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/OrderTypes.sol::57 => keccak256(makerOrder.params)
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol::61 => bytes32 digest = keccak256(abi.encodePacked("\x19\x01", domainSeparator, hash));
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/AccessControl.sol::22 => * bytes32 public constant MY_ROLE = keccak256("MY_ROLE");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::205 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::217 => return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(s.length), s));
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::230 => return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::17 => * `bytes4(keccak256("onERC1155Received(address,address,uint256,uint256,bytes)"))`
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::25 => * @return `bytes4(keccak256("onERC1155Received(address,address,uint256,uint256,bytes)"))` if transfer is allowed
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::41 => * `bytes4(keccak256("onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))`
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::49 => * @return `bytes4(keccak256("onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))` if transfer is allowed
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::11 => * Note: the hashing algorithm should be keccak256 and pair sorting should be enabled.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::16 => * hashing, or use a hash function other than keccak256 for hashing leaves.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/MerkleProof.sol::209 => value := keccak256(0x00, 0x40)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AdminUpgradeabilityProxy.sol::30 => ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/BaseAdminUpgradeabilityProxy.sol::24 => * This is the keccak-256 hash of "eip1967.proxy.admin" subtracted by 1, and is
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol::22 => * This is the keccak-256 hash of "eip1967.proxy.implementation" subtracted by 1, and is
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/InitializableAdminUpgradeabilityProxy.sol::33 => ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol::24 => bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/UpgradeabilityProxy.sol::23 => bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::56 => sload(keccak256(ChannelKey_channel_ptr, ChannelKey_length))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::33 => _CONDUIT_CREATION_CODE_HASH = keccak256(type(Conduit).creationCode);
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::76 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::348 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/lib/ConduitConstants.sol::15 => // keccak256(abi.encode(account, channels.slot))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::106 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ImmutableCreate2FactoryInterface.sol::43 => *      `keccak256( 0xff ++ address ++ salt ++ keccak256(init_code)))[12:]`
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ImmutableCreate2FactoryInterface.sol::64 => *      keccak256 hash of the contract's initialization code. The CREATE2
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ImmutableCreate2FactoryInterface.sol::66 => *      `keccak256( 0xff ++ address ++ salt ++ keccak256(init_code)))[12:]`
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ImmutableCreate2FactoryInterface.sol::73 => * @param initCodeHash The keccak256 hash of the initialization code that
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::403 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::533 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::577 => *   `keccak256(abi.encodePacked(receivedItemHashes))`
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::584 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::692 => //   `keccak256(abi.encode(offeredItem))`
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::695 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::704 => *   `keccak256(abi.encodePacked(offerItemHashes))`
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::706 => mstore(BasicOrder_order_offerHashes_ptr, keccak256(0, OneWord))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::746 => *   - 0xe0:   keccak256(abi.encodePacked(offerHashes))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::747 => *   - 0x100:  keccak256(abi.encodePacked(considerationHashes))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::802 => orderHash := keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::75 => return keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::150 => nameHash = keccak256(bytes(_nameString()));
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::153 => versionHash = keccak256(bytes("1.1"));
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::200 => eip712DomainTypehash = keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::212 => offerItemTypehash = keccak256(offerItemTypeString);
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::215 => considerationItemTypehash = keccak256(considerationItemTypeString);
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::218 => orderTypehash = keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::251 => *   - 0xe0:   keccak256(abi.encodePacked(offerHashes))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::252 => *   - 0x100:  keccak256(abi.encodePacked(considerationHashes))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::257 => let computedHash := keccak256(0, OneWord)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::285 => computedHash := keccak256(0, TwoWords)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::339 => let dataHash := keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::464 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::635 => let dataHash := keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::747 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::94 => mstore(hashArrPtr, keccak256(ptr, EIP712_OfferItem_size))
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::105 => offerHash := keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::151 => keccak256(ptr, EIP712_ConsiderationItem_size)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::163 => considerationHash := keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::217 => orderHash := keccak256(typeHashPtr, EIP712_Order_size)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::274 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::365 => value := keccak256(0, EIP712_DigestPayload_size)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::69 => zoneCreationCode = keccak256(type(PausableZone).creationCode);
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::94 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/PoolAddress.sol::43 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/PoolAddress.sol::47 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Position.sol::38 => keccak256(abi.encodePacked(owner, tickLower, tickUpper))
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3PoolDeployer.sol::43 => salt: keccak256(abi.encode(token0, token1, fee))
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Position.sol::37 => keccak256(abi.encodePacked(owner, tickLower, tickUpper))
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::13 => bytes32 public constant DELEGATION_CALLER = keccak256('DELEGATION_CALLER');
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::133 => bytes32 hash = keccak256(abi.encode(itemHashes.length, itemHashes, deadline));
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::208 => keccak256(
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::479 => bytes32 hash = keccak256(abi.encode(input.shared, input.details.length, input.details));
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::488 => bytes32 orderHash = keccak256(
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::70 => bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::308 => return keccak256(abi.encode(initiator, nftAsset, nftTokenIds, params));
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::297 => * bytes4(keccak256('supportsInterface(bytes4)')) == 0x01ffc9a7
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1561 => // Equals to `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1590 => *     bytes4(keccak256('balanceOf(address)')) == 0x70a08231
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1591 => *     bytes4(keccak256('ownerOf(uint256)')) == 0x6352211e
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1592 => *     bytes4(keccak256('approve(address,uint256)')) == 0x095ea7b3
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1593 => *     bytes4(keccak256('getApproved(uint256)')) == 0x081812fc
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1594 => *     bytes4(keccak256('setApprovalForAll(address,bool)')) == 0xa22cb465
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1595 => *     bytes4(keccak256('isApprovedForAll(address,address)')) == 0xe985e9c5
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1596 => *     bytes4(keccak256('transferFrom(address,address,uint256)')) == 0x23b872dd
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1597 => *     bytes4(keccak256('safeTransferFrom(address,address,uint256)')) == 0x42842e0e
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1598 => *     bytes4(keccak256('safeTransferFrom(address,address,uint256,bytes)')) == 0xb88d4fde
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1606 => *     bytes4(keccak256('name()')) == 0x06fdde03
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1607 => *     bytes4(keccak256('symbol()')) == 0x95d89b41
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1608 => *     bytes4(keccak256('tokenURI(uint256)')) == 0xc87b56dd
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1615 => *     bytes4(keccak256('totalSupply()')) == 0x18160ddd
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1616 => *     bytes4(keccak256('tokenOfOwnerByIndex(address,uint256)')) == 0x2f745c59
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1617 => *     bytes4(keccak256('tokenByIndex(uint256)')) == 0x4f6ccce7
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::287 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::514 => bytes32 tokenIdGeneration = keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::391 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::672 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::677 => keccak256(abi.encodePacked(offer.makerIds)),
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::679 => keccak256(abi.encodePacked(offer.takerIds)),
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::711 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::14 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::18 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::34 => DOMAIN_SEPARATOR = keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::37 => keccak256(bytes(name)),
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::38 => keccak256(EIP712_REVISION),
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::60 => bytes32 digest = keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::64 => keccak256(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1539 => approvedAddressSlot := keccak256(0x00, 0x40)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2647 => * bytes32 public constant MY_ROLE = keccak256("MY_ROLE");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3202 => bytes32 public constant EXPULSION_ROLE = keccak256("EXPULSION_ROLE");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::15 => bytes32 public constant override POOL_ADMIN_ROLE = keccak256("POOL_ADMIN");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::17 => keccak256("EMERGENCY_ADMIN");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::18 => bytes32 public constant override RISK_ADMIN_ROLE = keccak256("RISK_ADMIN");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::20 => keccak256("FLASH_BORROWER");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::21 => bytes32 public constant override BRIDGE_ROLE = keccak256("BRIDGE");
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::23 => keccak256("ASSET_LISTING_ADMIN");
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1076 => keccak256(abi.encodePacked(params.orderInfo.id)) ==
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1077 => keccak256(abi.encodePacked(params.credit.orderId)),
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1115 => bytes32 typeHash = keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1123 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1128 => keccak256(abi.encodePacked(credit.orderId))
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1135 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1137 => 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1138 => 0x88d989289235fb06c18e3c2f7ea914f41f773e86fb0073d632539f566f4df353, // keccak256("ParaSpace")
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1139 => 0x722c0e0c80487266e8c6a45e3a1a803aab23378a9c32e6ebe029d4fad7bfc965, // keccak256(bytes("1.1")),
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol::37 => bytes32(uint256(keccak256("paraspace.proxy.rg.storage")) - 1);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol::18 => bytes32(uint256(keccak256("paraspace.proxy.version.storage")) - 1);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::13 => uint256(keccak256("paraspace.proxy.implementation.storage")) - 1
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol::17 => bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::25 => uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::35 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::227 => bytes32 digest = keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::231 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::26 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::61 => bytes32 digest = keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::65 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::12 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::56 => keccak256(
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::59 => keccak256(bytes(_EIP712BaseId())),
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::60 => keccak256(EIP712_REVISION),
2022-11-paraspace/paraspace-core/foundry_test/NFTFloorOracle.t.sol::209 => bytes32 _admin = _contract.getRoleAdmin(keccak256("UPDATER_ROLE"));
2022-11-paraspace/paraspace-core/foundry_test/NFTFloorOracle.t.sol::222 => _contract.grantRole(keccak256("UPDATER_ROLE"), newOwner);
2022-11-paraspace/paraspace-core/foundry_test/NFTFloorOracle.t.sol::223 => assertTrue(_contract.hasRole(keccak256("UPDATER_ROLE"), newOwner));
```
#### Tools used
Manual









### 5th BUG
Long Revert Strings

#### Impact
Issue Information: 
Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Example
🤦 Bad:

require(condition, "UniswapV3: The reentrancy guard. A transaction cannot re-enter the pool mid-swap");
🚀 Good (with shorter string):

// TODO: Provide link to a reference of error codes
require(condition, "LOK");
🚀 Good (with custom errors):

/// @notice A transaction cannot re-enter the pool mid-swap.
error NoReentrancy();

// ...

if (!condition) {
    revert NoReentrancy();
}

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/gnosis/contracts/GPv2SafeERC20.sol::4 => import {IERC20} from "../../openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::4 => import {Ownable} from "../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::5 => import {EnumerableSet} from "../../openzeppelin/contracts/EnumerableSet.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::7 => import {ICurrencyManager} from "./interfaces/ICurrencyManager.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::4 => import {Ownable} from "../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::5 => import {EnumerableSet} from "../../openzeppelin/contracts/EnumerableSet.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::7 => import {IExecutionManager} from "./interfaces/IExecutionManager.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::5 => import {Ownable} from "../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::6 => import {ReentrancyGuard} from "../../openzeppelin/contracts/ReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::7 => import {IERC20, SafeERC20} from "../../openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::10 => import {ICurrencyManager} from "./interfaces/ICurrencyManager.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::11 => import {IExecutionManager} from "./interfaces/IExecutionManager.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::12 => import {IExecutionStrategy} from "./interfaces/IExecutionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::13 => import {IRoyaltyFeeManager} from "./interfaces/IRoyaltyFeeManager.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::14 => import {ILooksRareExchange} from "./interfaces/ILooksRareExchange.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::15 => import {ITransferManagerNFT} from "./interfaces/ITransferManagerNFT.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::16 => import {ITransferSelectorNFT} from "./interfaces/ITransferSelectorNFT.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::139 => 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::159 => require(minNonce > userMinOrderNonce[msg.sender], "Cancel: Order nonce lower than current");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::160 => require(minNonce < userMinOrderNonce[msg.sender] + 500000, "Cancel: Cannot cancel more orders");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::174 => require(orderNonces[i] >= userMinOrderNonce[msg.sender], "Cancel: Order nonce lower than current");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::541 => require(transferManager != address(0), "Transfer: No NFT transfer manager available");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/RoyaltyFeeManager.sol::4 => import {Ownable} from "../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/RoyaltyFeeManager.sol::5 => import {IERC165, IERC2981} from "../../openzeppelin/contracts/IERC2981.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/RoyaltyFeeManager.sol::7 => import {IRoyaltyFeeManager} from "./interfaces/IRoyaltyFeeManager.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/RoyaltyFeeManager.sol::8 => import {IRoyaltyFeeRegistry} from "./interfaces/IRoyaltyFeeRegistry.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::4 => import {Ownable} from "../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::5 => import {IERC165} from "../../openzeppelin/contracts/IERC165.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::7 => import {ITransferSelectorNFT} from "./interfaces/ITransferSelectorNFT.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::47 => require(collection != address(0), "Owner: Collection cannot be null address");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::48 => require(transferManager != address(0), "Owner: TransferManager cannot be null address");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::62 => "Owner: Collection has no transfer manager"
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyAnyItemFromCollectionForFixedPrice.sol::5 => import {IExecutionStrategy} from "../interfaces/IExecutionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyAnyItemInASetForFixedPrice.sol::4 => import {MerkleProof} from "../../../openzeppelin/contracts/MerkleProof.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyAnyItemInASetForFixedPrice.sol::6 => import {IExecutionStrategy} from "../interfaces/IExecutionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::4 => import {Ownable} from "../../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::6 => import {IExecutionStrategy} from "../interfaces/IExecutionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::27 => require(_minimumAuctionLengthInSeconds >= 15 minutes, "Owner: Auction length must be > 15 min");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::56 => require(endTime >= (startTime + minimumAuctionLengthInSeconds), "Dutch Auction: Length must be longer");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::57 => require(startPrice > endPrice, "Dutch Auction: Start price must be greater than end price");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::104 => require(_minimumAuctionLengthInSeconds >= 15 minutes, "Owner: Auction length must be > 15 min");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyPrivateSale.sol::5 => import {IExecutionStrategy} from "../interfaces/IExecutionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyStandardSaleForFixedPrice.sol::5 => import {IExecutionStrategy} from "../interfaces/IExecutionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/OrderTypes.sol::9 => // keccak256("MakerOrder(bool isOrderAsk,address signer,address collection,uint256 price,uint256 tokenId,uint256 amount,address strategy,address currency,uint256 nonce,uint256 startTime,uint256 endTime,uint256 minPercentageToAsk,bytes params)")
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol::4 => import {Address} from "../../../openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol::5 => import {IERC1271} from "../../../openzeppelin/contracts/IERC1271.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeRegistry.sol::4 => import {Ownable} from "../../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeRegistry.sol::6 => import {IRoyaltyFeeRegistry} from "../interfaces/IRoyaltyFeeRegistry.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeRegistry.sol::32 => require(_royaltyFeeLimit <= 9500, "Owner: Royalty fee limit too high");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeRegistry.sol::41 => require(_royaltyFeeLimit <= 9500, "Owner: Royalty fee limit too high");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeSetter.sol::4 => import {Ownable} from "../../../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeSetter.sol::5 => import {IERC165} from "../../../openzeppelin/contracts/IERC165.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeSetter.sol::7 => import {IRoyaltyFeeRegistry} from "../interfaces/IRoyaltyFeeRegistry.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC1155.sol::4 => import {IERC1155} from "../../../openzeppelin/contracts/IERC1155.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC1155.sol::6 => import {ITransferManagerNFT} from "../interfaces/ITransferManagerNFT.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC1155.sol::38 => require(msg.sender == LOOKS_RARE_EXCHANGE, "Transfer: Only LooksRare Exchange");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC721.sol::4 => import {IERC721} from "../../../openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC721.sol::6 => import {ITransferManagerNFT} from "../interfaces/ITransferManagerNFT.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC721.sol::38 => require(msg.sender == LOOKS_RARE_EXCHANGE, "Transfer: Only LooksRare Exchange");
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerNonCompliantERC721.sol::4 => import {IERC721} from "../../../openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerNonCompliantERC721.sol::5 => import {ITransferManagerNFT} from "../interfaces/ITransferManagerNFT.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerNonCompliantERC721.sol::36 => require(msg.sender == LOOKS_RARE_EXCHANGE, "Transfer: Only LooksRare Exchange");
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::377 => // Note that the "mul" in this block is the assembly mul operation, not the "mul" function defined in this contract.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::517 => // The "delta >>= 1" part is equivalent to "delta /= 2", but shifting bits is faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::611 => /// - All from "abs" and "PRBMath.mulDivFixedPoint".
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::64 => // Equivalent to "x + delta * (remainder > 0 ? 1 : 0)" but faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::147 => // Equivalent to "x - remainder * (remainder > 0 ? 1 : 0)" but faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::257 => // Note that the "mul" in this block is the assembly multiplication operation, not the "mul" function defined
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::387 => // The "delta >>= 1" part is equivalent to "delta /= 2", but shifting bits is faster.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/AccessControl.sol::190 => "AccessControl: can only renounce roles for self"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::64 => require(success, "Address: unable to send value, recipient may have reverted");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::119 => return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::134 => require(address(this).balance >= value, "Address: insufficient balance for call");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::148 => return functionStaticCall(target, data, "Address: low-level static call failed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::162 => require(isContract(target), "Address: static call to non-contract");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::175 => return functionDelegateCall(target, data, "Address: low-level delegate call failed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::189 => require(isContract(target), "Address: delegate call to non-contract");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::31 => revert("ECDSA: invalid signature 's' value");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::33 => revert("ECDSA: invalid signature 'v' value");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::71 => require(account != address(0), "ERC1155: balance query for the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::89 => require(accounts.length == ids.length, "ERC1155: accounts and ids length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::126 => "ERC1155: caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::143 => "ERC1155: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::167 => require(to != address(0), "ERC1155: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::174 => require(fromBalance >= amount, "ERC1155: insufficient balance for transfer");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::202 => require(ids.length == amounts.length, "ERC1155: ids and amounts length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::203 => require(to != address(0), "ERC1155: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::214 => require(fromBalance >= amount, "ERC1155: insufficient balance for transfer");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::266 => require(to != address(0), "ERC1155: mint to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::293 => require(to != address(0), "ERC1155: mint to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::294 => require(ids.length == amounts.length, "ERC1155: ids and amounts length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::322 => require(from != address(0), "ERC1155: burn from the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::329 => require(fromBalance >= amount, "ERC1155: burn amount exceeds balance");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::349 => require(from != address(0), "ERC1155: burn from the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::350 => require(ids.length == amounts.length, "ERC1155: ids and amounts length mismatch");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::361 => require(fromBalance >= amount, "ERC1155: burn amount exceeds balance");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::380 => require(owner != operator, "ERC1155: setting approval status for self");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::425 => revert("ERC1155: ERC1155Receiver rejected tokens");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::430 => revert("ERC1155: transfer to non ERC1155Receiver implementer");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::448 => revert("ERC1155: ERC1155Receiver rejected tokens");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::453 => revert("ERC1155: transfer to non ERC1155Receiver implementer");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::180 => "ERC20: transfer amount exceeds allowance"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::235 => "ERC20: decreased allowance below zero"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::260 => require(sender != address(0), "ERC20: transfer from the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::261 => require(recipient != address(0), "ERC20: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::267 => "ERC20: transfer amount exceeds balance"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::304 => require(account != address(0), "ERC20: burn from the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::310 => "ERC20: burn amount exceeds balance"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::334 => require(owner != address(0), "ERC20: approve from the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::335 => require(spender != address(0), "ERC20: approve to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::77 => "ERC721: address zero is not a valid owner"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::95 => "ERC721: owner query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::126 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::150 => require(to != owner, "ERC721: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::154 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::172 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::213 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::241 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::273 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::304 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::338 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::412 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::414 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721.sol::484 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::54 => "ERC721Enumerable: owner index out of bounds"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::78 => "ERC721Enumerable: global index out of bounds"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::17 => * `bytes4(keccak256("onERC1155Received(address,address,uint256,uint256,bytes)"))`
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::25 => * @return `bytes4(keccak256("onERC1155Received(address,address,uint256,uint256,bytes)"))` if transfer is allowed
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::41 => * `bytes4(keccak256("onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))`
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::49 => * @return `bytes4(keccak256("onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))` if transfer is allowed
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Ownable.sol::70 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::34 => "SafeCast: value doesn't fit in 224 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::52 => "SafeCast: value doesn't fit in 128 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::70 => "SafeCast: value doesn't fit in 96 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::88 => "SafeCast: value doesn't fit in 64 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::106 => "SafeCast: value doesn't fit in 32 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::124 => "SafeCast: value doesn't fit in 16 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::142 => "SafeCast: value doesn't fit in 8 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::175 => "SafeCast: value doesn't fit in 128 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::196 => "SafeCast: value doesn't fit in 64 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::217 => "SafeCast: value doesn't fit in 32 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::238 => "SafeCast: value doesn't fit in 16 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::259 => "SafeCast: value doesn't fit in 8 bits"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeCast.sol::275 => "SafeCast: value doesn't fit in an int256"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::56 => "SafeERC20: approve from non-zero to non-zero allowance"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::77 => require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::96 => require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::113 => require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::64 => require(success, "Address: unable to send value, recipient may have reverted");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::119 => return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::134 => require(address(this).balance >= value, "Address: insufficient balance for call");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::146 => return functionStaticCall(target, data, "Address: low-level static call failed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AdminUpgradeabilityProxy.sol::4 => import "./BaseAdminUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/BaseAdminUpgradeabilityProxy.sol::65 => "Cannot change the admin of a proxy to the zero address"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/BaseAdminUpgradeabilityProxy.sol::128 => "Cannot call fallback function from the proxy admin"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol::56 => "Cannot set a proxy implementation to a non-contract address"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/Initializable.sol::83 => "Initializable: contract is already initialized"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/Initializable.sol::111 => "Initializable: contract is already initialized"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/Initializable.sol::125 => require(_initializing, "Initializable: contract is not initializing");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/Initializable.sol::136 => require(!_initializing, "Initializable: contract is initializing");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/InitializableAdminUpgradeabilityProxy.sol::4 => import "./BaseAdminUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/InitializableAdminUpgradeabilityProxy.sol::5 => import "./InitializableUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol::75 => require(newOwner != address(0), "Ownable: new owner is the zero address");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::7 => import "./draft-IERC20PermitUpgradeable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::56 => "SafeERC20: approve from non-zero to non-zero allowance"
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::77 => require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::96 => require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::113 => require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::4 => import { ConduitInterface } from "../interfaces/ConduitInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::17 => import { INToken } from "../../../../interfaces/INToken.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::18 => import { IProtocolDataProvider } from "../../../../interfaces/IProtocolDataProvider.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::6 => } from "../interfaces/ConduitControllerInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::8 => import { ConduitInterface } from "../interfaces/ConduitInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::4 => import { IERC721Receiver } from "../interfaces/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::8 => import { ConduitInterface } from "../interfaces/ConduitInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::12 => } from "../interfaces/ConduitControllerInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::16 => import { ConduitTransfer } from "../conduit/lib/ConduitStructs.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::20 => } from "../interfaces/TransferHelperInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::22 => import { TransferHelperErrors } from "../interfaces/TransferHelperErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ConduitInterface.sol::7 => } from "../conduit/lib/ConduitStructs.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/TransferHelperInterface.sol::7 => } from "../helpers/TransferHelperStructs.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/AmountDeriver.sol::6 => } from "../interfaces/AmountDerivationErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::10 => } from "../interfaces/TokenTransferrerErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::4 => import { ConduitInterface } from "../interfaces/ConduitInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Consideration.sol::6 => } from "../interfaces/ConsiderationInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::6 => } from "../interfaces/ConduitControllerInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::10 => } from "../interfaces/ConsiderationEventsAndErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::187 => "ConsiderationItem[] consideration,",
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::15 => *    - The term "body" is used in place of the term "head" used in the ABI
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CounterManager.sol::6 => } from "../interfaces/ConsiderationEventsAndErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::18 => } from "../interfaces/CriteriaResolutionErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::4 => import { ConduitInterface } from "../interfaces/ConduitInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::20 => } from "../interfaces/FulfillmentApplicationErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::511 => // The "full" order types are even, while "partial" order types are odd.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ReentrancyGuard.sol::4 => import { ReentrancyErrors } from "../interfaces/ReentrancyErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::4 => import { EIP1271Interface } from "../interfaces/EIP1271Interface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::8 => } from "../interfaces/SignatureVerificationErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::8 => } from "../interfaces/TokenTransferrerErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::10 => import { ConduitBatch1155Transfer } from "../conduit/lib/ConduitStructs.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::15 => *    - The term "body" is used in place of the term "head" used in the ABI
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::49 => // abi.encodeWithSignature("transferFrom(address,address,uint256)")
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::60 => //     "safeTransferFrom(address,address,uint256,uint256,bytes)"
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::76 => //     "safeBatchTransferFrom(address,address,uint256[],uint256[],bytes)"
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::102 => //     "TokenTransferGenericFailure(address,address,address,uint256,uint256)"
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::118 => //     "BadReturnValueFromERC20OnTransfer(address,address,address,uint256)"
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ZoneInteraction.sol::12 => import { ZoneInteractionErrors } from "../interfaces/ZoneInteractionErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZone.sol::5 => import { ZoneInteractionErrors } from "../interfaces/ZoneInteractionErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZone.sol::9 => } from "./interfaces/PausableZoneEventsAndErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZone.sol::11 => import { SeaportInterface } from "../interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZone.sol::22 => import { PausableZoneInterface } from "./interfaces/PausableZoneInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::8 => } from "./interfaces/PausableZoneControllerInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::12 => } from "./interfaces/PausableZoneEventsAndErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::23 => import { SeaportInterface } from "../interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneControllerInterface.sol::6 => import { PausableZoneEventsAndErrors } from "./PausableZoneEventsAndErrors.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneControllerInterface.sol::15 => } from "../../lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneControllerInterface.sol::17 => import { SeaportInterface } from "../../interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneInterface.sol::4 => import { SeaportInterface } from "../../interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneInterface.sol::13 => } from "../../lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IERC721Permit.sol::4 => import "../openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/INonfungiblePositionManager.sol::5 => import "../openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/INonfungiblePositionManager.sol::6 => import "../openzeppelin/contracts/IERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Factory.sol::4 => import "./interfaces/IUniswapV3Factory.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::23 => import "./interfaces/IUniswapV3PoolDeployer.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::24 => import "./interfaces/IUniswapV3Factory.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::26 => import "./interfaces/callback/IUniswapV3MintCallback.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::27 => import "./interfaces/callback/IUniswapV3SwapCallback.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3Pool.sol::28 => import "./interfaces/callback/IUniswapV3FlashCallback.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/UniswapV3PoolDeployer.sol::4 => import "./interfaces/IUniswapV3PoolDeployer.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IUniswapV3Pool.sol::4 => import "./pool/IUniswapV3PoolImmutables.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IUniswapV3Pool.sol::6 => import "./pool/IUniswapV3PoolDerivedState.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IUniswapV3Pool.sol::8 => import "./pool/IUniswapV3PoolOwnerActions.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/weth/WETH9.sol::382 => A "User Product" is either (1) a "consumer product", which means any
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::5 => import "../openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::6 => import "../openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::7 => import "../openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::8 => import "../openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::9 => import "../openzeppelin/contracts/ERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::252 => * Example 3: (BAYC + BAKC + 1 ApeCoin) and (MAYC + BAKC + 1 ApeCoin): [[0, 0, "1000000000000000000"], [0, 1, "1000000000000000000"]]
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::428 => require (_startTimestamp < _endTimeStamp, "_startTimestamp should be less than _endTimeStamp");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::429 => require(getMinute(_startTimestamp) == 0 && getSecond(_startTimestamp) == 0, "_startTimestamp is not a whole hour");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::430 => require(getMinute(_endTimeStamp) == 0 && getSecond(_endTimeStamp) == 0, "_endTimeStamp is not a whole hour");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::434 => ,"_startTimestamp should be equal to the last endTimestampHour");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::870 => , "Main Token not owned by caller or already paired");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::872 => ,"BAKC Token not owned by caller or already paired");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::930 => && secondToMain.tokenId == mainTokenId && secondToMain.isPaired, "The provided Token IDs are not paired");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::938 => require(_amount <= _position.stakedAmount, "Can't withdraw more than staked amount");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::976 => require(mainTokenOwner == msg.sender || bakcOwner == msg.sender, "At least one token in pair must be owned by caller");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::978 => && secondToMain.tokenId == mainTokenId && secondToMain.isPaired, "The provided Token IDs are not paired");
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::981 => require(mainTokenOwner == bakcOwner || amount == position.stakedAmount, "Split pair can't partially withdraw");
2022-11-paraspace/paraspace-core/contracts/deployments/ReservesSetupHelper.sol::4 => import {PoolConfigurator} from "../protocol/pool/PoolConfigurator.sol";
2022-11-paraspace/paraspace-core/contracts/deployments/ReservesSetupHelper.sol::5 => import {Ownable} from "../dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IAuctionableERC721.sol::3 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IERC20WithPermit.sol::4 => import {IERC20} from "../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::4 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::5 => import {Errors} from "../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::6 => import {OrderTypes} from "../dependencies/looksrare/contracts/libraries/OrderTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::7 => import {SeaportInterface} from "../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::8 => import {ILooksRareExchange} from "../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::9 => import {SignatureChecker} from "../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::10 => import {ConsiderationItem} from "../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::11 => import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::12 => import {Address} from "../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IMarketplace.sol::13 => import {IERC1271} from "../dependencies/openzeppelin/contracts/IERC1271.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/INToken.sol::4 => import {IERC721} from "../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/INToken.sol::5 => import {IERC721Receiver} from "../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/INToken.sol::6 => import {IERC721Enumerable} from "../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/INToken.sol::7 => import {IERC1155Receiver} from "../dependencies/openzeppelin/contracts/IERC1155Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/INToken.sol::11 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/INTokenApeStaking.sol::4 => import "../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPToken.sol::4 => import {IERC20} from "../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPoolAddressesProvider.sol::4 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPoolApeStaking.sol::4 => import "../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPoolConfigurator.sol::4 => import {ConfiguratorInputTypes} from "../protocol/libraries/types/ConfiguratorInputTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPoolCore.sol::5 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPoolMarketplace.sol::5 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IPoolParameters.sol::5 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IProtocolDataProvider.sol::4 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IReserveInterestRateStrategy.sol::4 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IUniswapV3OracleWrapper.sol::4 => import {IAtomicPriceAggregator} from "../interfaces/IAtomicPriceAggregator.sol";
2022-11-paraspace/paraspace-core/contracts/interfaces/IUniswapV3OracleWrapper.sol::5 => import {IUniswapV3PositionInfoProvider} from "../interfaces/IUniswapV3PositionInfoProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::4 => import {IEACAggregatorProxy} from "../interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::5 => import {Errors} from "../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ERC721OracleWrapper.sol::7 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::4 => import "../dependencies/openzeppelin/contracts/AccessControl.sol";
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::5 => import "../dependencies/openzeppelin/upgradeability/Initializable.sol";
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::227 => "NFTOracle: Tokens and price length differ"
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::356 => require(_twap > 0, "NFTOracle: price should be more than 0");
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::5 => import {IUniswapV2Factory} from "./interfaces/IUniswapV2Factory.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::6 => import {IUniswapV2Router01} from "./interfaces/IUniswapV2Router01.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::8 => import {IERC165} from "../dependencies/openzeppelin/contracts/IERC165.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol::9 => import {ERC20} from "../dependencies/openzeppelin/contracts/ERC20.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::4 => import {IEACAggregatorProxy} from "../interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::6 => import {Errors} from "../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::8 => import {IAtomicPriceAggregator} from "../interfaces/IAtomicPriceAggregator.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::9 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::10 => import {IPriceOracleGetter} from "../interfaces/IPriceOracleGetter.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol::11 => import {IParaSpaceOracle} from "../interfaces/IParaSpaceOracle.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::4 => import {IERC20Detailed} from "../dependencies/openzeppelin/contracts/IERC20Detailed.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::5 => import {IERC721Metadata} from "../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::6 => import {ReserveConfiguration} from "../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::7 => import {UserConfiguration} from "../protocol/libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::8 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::9 => import {WadRayMath} from "../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::10 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::11 => import {IVariableDebtToken} from "../interfaces/IVariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::12 => import {ICollateralizableERC721} from "../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::13 => import {IScaledBalanceToken} from "../interfaces/IScaledBalanceToken.sol";
2022-11-paraspace/paraspace-core/contracts/misc/ProtocolDataProvider.sol::17 => import {IProtocolDataProvider} from "../interfaces/IProtocolDataProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::4 => import {IUniswapV3OracleWrapper} from "../interfaces/IUniswapV3OracleWrapper.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::5 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::6 => import {IPriceOracleGetter} from "../interfaces/IPriceOracleGetter.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::7 => import {IUniswapV3Factory} from "../dependencies/uniswap/IUniswapV3Factory.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::8 => import {IUniswapV3PoolState} from "../dependencies/uniswap/IUniswapV3PoolState.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::9 => import {INonfungiblePositionManager} from "../dependencies/uniswap/INonfungiblePositionManager.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::10 => import {LiquidityAmounts} from "../dependencies/uniswap/LiquidityAmounts.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::11 => import {TickMath} from "../dependencies/uniswap/libraries/TickMath.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::13 => import {FullMath} from "../dependencies/uniswap/libraries/FullMath.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::14 => import {IERC20Detailed} from "../dependencies/openzeppelin/contracts/IERC20Detailed.sol";
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::15 => import {UinswapV3PositionData} from "../interfaces/IUniswapV3PositionInfoProvider.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::4 => import "../interfaces/IFlashClaimReceiver.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::5 => import "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::6 => import "../../dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::7 => import "../../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::8 => import "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::9 => import "../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::10 => import "../../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::11 => import "../../dependencies/openzeppelin/contracts/ERC721Holder.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::12 => import "../../dependencies/openzeppelin/contracts/IERC1155.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::13 => import "../../dependencies/openzeppelin/contracts/ERC1155Holder.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::14 => import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol::88 => "invalid airdrop token address length"
2022-11-paraspace/paraspace-core/contracts/misc/flashclaim/UserFlashclaimRegistry.sol::5 => import "../interfaces/IUserFlashclaimRegistry.sol";
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IWrappedPunks.sol::4 => import {IERC721} from "../../../contracts/dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IWrappedPunks.sol::6 => //import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::4 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::5 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::6 => import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::7 => import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::8 => import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::9 => import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::10 => import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::11 => import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::12 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::13 => import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::14 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol::15 => import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::4 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::5 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::6 => import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::7 => import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::8 => import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::9 => import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::10 => import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::11 => import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::12 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::13 => import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::14 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol::15 => import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::4 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::5 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::6 => import {OrderTypes} from "../../dependencies/looksrare/contracts/libraries/OrderTypes.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::7 => import {SeaportInterface} from "../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::8 => import {ILooksRareExchange} from "../../dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::10 => import {SignatureChecker} from "../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::11 => import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::12 => import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::13 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::14 => import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::15 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol::16 => import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/helpers/MockIncentivesController.sol::4 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/helpers/MockReserveConfiguration.sol::4 => import {ReserveConfiguration} from "../../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/helpers/MockReserveConfiguration.sol::5 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/oracle/CLAggregators/MockAggregator.sol::4 => import {IEACAggregatorProxy} from "../../../interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/oracle/PriceOracle.sol::4 => import {IPriceOracle} from "../../interfaces/IPriceOracle.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/oracle/SequencerOracle.sol::4 => import {Ownable} from "../../dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/oracle/SequencerOracle.sol::5 => import {ISequencerOracle} from "../../interfaces/ISequencerOracle.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockAirdropProject.sol::5 => import "../tokens/AirdropMintableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockAirdropProject.sol::7 => import "../../dependencies/openzeppelin/contracts/ERC721.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockAirdropProject.sol::8 => import "../../dependencies/openzeppelin/contracts/ERC721Holder.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockAirdropProject.sol::9 => import "../../dependencies/openzeppelin/contracts/ERC1155Holder.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::5 => import {WadRayMath} from "../../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::6 => import {PercentageMath} from "../../protocol/libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::7 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::8 => import {IReserveAuctionStrategy} from "../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::9 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::11 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::12 => import {PRBMathUD60x18} from "../../dependencies/math/PRBMathUD60x18.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveAuctionStrategy.sol::13 => import {PRBMath} from "../../dependencies/math/PRBMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveInterestRateStrategy.sol::4 => import {IReserveInterestRateStrategy} from "../../interfaces/IReserveInterestRateStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveInterestRateStrategy.sol::5 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveInterestRateStrategy.sol::6 => import {WadRayMath} from "../../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockReserveInterestRateStrategy.sol::7 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tests/WadRayMathWrapper.sol::4 => import {WadRayMath} from "../../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/AirdropMintableERC721.sol::4 => import "../../dependencies/openzeppelin/contracts/ERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::71 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::198 => "not enough remaining reserved for auction to support desired mint amount"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::230 => "called with incorrect public sale key"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::320 => "addresses does not match numSlots length"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::331 => "too many already minted before dev mint"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::335 => "can only mint a multiple of the maxBatchSize"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::480 => require(c / a == b, "SafeMath: multiplication overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::651 => "Address: unable to send value, recipient may have reverted"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::715 => "Address: low-level call with value failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::733 => "Address: insufficient balance for call"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::759 => "Address: low-level static call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::774 => require(isContract(target), "Address: static call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::795 => "Address: low-level delegate call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::810 => require(isContract(target), "Address: delegate call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::979 => "EnumerableSet: index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1338 => "EnumerableMap: index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1561 => // Equals to `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1648 => "ERC721: balance query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1666 => "ERC721: owner query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1696 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1763 => require(to != owner, "ERC721: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1768 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1786 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1830 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1858 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1890 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1921 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::1955 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2032 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2034 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2062 => "ERC721Metadata: URI set of nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2103 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2206 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2296 => "Can only mint 20 tokens at a time"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2300 => "Purchase would exceed max supply of Apes"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::74 => "ERC721: balance query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::92 => "ERC721: owner query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::123 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::147 => require(to != owner, "ERC721: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::151 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::169 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::213 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::241 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::273 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::304 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::338 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::408 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::410 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::463 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::548 => "ERC721Enumerable: owner index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::572 => "ERC721Enumerable: global index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::714 => "ERC721URIStorage: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::745 => "ERC721URIStorage: URI set of nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::835 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::972 => "Contract has been locked and URI can't be changed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::4 => import {Ownable} from "../../../contracts/dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::9 => "ac39af4793119ee46bbff351d8cb6b5f23da60222126add4268e261199a2921b";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::93 => require(allPunksAssigned, "CryptoPunksMarket:  allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::94 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::123 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::126 => "CryptoPunksMarket: empty punksRemainingToAssign"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::132 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::143 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::148 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::171 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::176 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::192 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::197 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::215 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::220 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::234 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::235 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::240 => "CryptoPunksMarket: punk not actually for sale"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::244 => "CryptoPunksMarket: punk not supposed to be sold to this user"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::249 => "CryptoPunksMarket: Didn't send enough ETH"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::253 => "CryptoPunksMarket: Seller no longer owner of punk"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::280 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::291 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::292 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::295 => "CryptoPunksMarket: can not buy your own punk"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::299 => "CryptoPunksMarket: can not buy unassigned punk"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::301 => require(msg.value > 0, "CryptoPunksMarket: should send eth value");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::306 => "CryptoPunksMarket: should send more eth value"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::319 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::324 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::329 => require(bid.value >= minPrice, "CryptoPunksMarket: bid value to small");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::352 => require(punkIndex < 10000, "CryptoPunksMarket: punkIndex overflow");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::353 => require(allPunksAssigned, "CryptoPunksMarket: not allPunksAssigned");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::356 => "CryptoPunksMarket: punk not assigned"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::360 => "CryptoPunksMarket: can not withdraw self"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CryptoPunksMarket.sol::364 => require(bid.bidder == msg.sender, "CryptoPunksMakrket: not bid bidder");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::131 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::835 => "Address: unable to send value, recipient may have reverted"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::899 => "Address: low-level call with value failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::917 => "Address: insufficient balance for call"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::942 => "Address: low-level static call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::957 => require(isContract(target), "Address: static call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::977 => "Address: low-level delegate call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::992 => require(isContract(target), "Address: delegate call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1127 => "ERC721: balance query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1145 => "ERC721: owner query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1176 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1200 => require(to != owner, "ERC721: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1204 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1222 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1263 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1291 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1323 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1354 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1388 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1462 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1464 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1534 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1664 => "ERC721Enumerable: owner index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1688 => "ERC721Enumerable: global index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1841 => "Exceeded max available to purchase"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1910 => require(saleIsActive, "Sale must be active to mint tokens");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::72 => "ERC721: balance query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::90 => "ERC721: owner query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::121 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::145 => require(to != owner, "ERC721: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::149 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::167 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::211 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::239 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::271 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::302 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::336 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::406 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::408 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::461 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::550 => "ERC721Enumerable: owner index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::574 => "ERC721Enumerable: global index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::840 => "SafeERC20: approve from non-zero to non-zero allowance"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::873 => "SafeERC20: decreased allowance below zero"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::906 => "SafeERC20: ERC20 operation did not succeed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::978 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::294 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::483 => "ERC721: balance query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::501 => "ERC721: owner query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::532 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::556 => require(to != owner, "ERC721: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::560 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::578 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::622 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::650 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::682 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::713 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::747 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::817 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::819 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::872 => "ERC721: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::964 => "ERC721Enumerable: owner index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::988 => "ERC721Enumerable: global index out of bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1158 => "Address: unable to send value, recipient may have reverted"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1222 => "Address: low-level call with value failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1240 => "Address: insufficient balance for call"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1265 => "Address: low-level static call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1280 => require(isContract(target), "Address: static call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1300 => "Address: low-level delegate call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1315 => require(isContract(target), "Address: delegate call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1427 => "ca7151cc436da0dc3a3d662694f8c9da5ae39a7355fabaafc00e6aa580927175";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1496 => "Minted Mutants starting index is already set"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1500 => "Mega Mutants starting index is already set"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1596 => "Must own the ape you're attempting to mutate"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1600 => "Must own at least one of this serum type to mutate"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1608 => "Would exceed supply of serum-mutatable MEGA MUTANTS"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1612 => "Ape already mutated with MEGA MUTATION SERUM"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1622 => "Ape already mutated with this type of serum"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1667 => "Mega mutant ID can't be calculated"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1675 => "tokenId outside collection bounds"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1716 => "Invalid setStartingIndices conditions"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::132 => "QmfXYgfX1qNfzQ6NRyFnupniZusasFPMeiWn5aaDnx7YXo";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::461 => "Already minted with this punk/glyph"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::651 => "https://meebits.larvalabs.com/meebit/",
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::791 => "Maker does not have sufficient balance."
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::797 => "At least one maker token doesn't belong to maker."
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::805 => "If trade is offered to anybody, cannot specify tokens from taker."
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::812 => "At least one taker token doesn't belong to taker."
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::829 => require(maker == msg.sender, "Only the maker can cancel this offer.");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::892 => "Insufficient funds to execute trade."
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableDelegationERC20.sol::4 => import {ERC20} from "../../dependencies/openzeppelin/contracts/ERC20.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableDelegationERC20.sol::5 => import {IDelegationToken} from "../../interfaces/IDelegationToken.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC1155.sol::4 => import "../../dependencies/openzeppelin/contracts/ERC1155.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::4 => import {ERC20} from "../../dependencies/openzeppelin/contracts/ERC20.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::5 => import {IERC20WithPermit} from "../../interfaces/IERC20WithPermit.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::15 => "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC20.sol::19 => "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC721.sol::6 => import {ERC721Enumerable} from "../../dependencies/openzeppelin/contracts/ERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC721.sol::7 => import {ERC721} from "../../dependencies/openzeppelin/contracts/ERC721.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MintableERC721.sol::8 => import {Context} from "../../dependencies/openzeppelin/contracts/Context.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockAToken.sol::5 => import {WadRayMath} from "../../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::3 => import "../../dependencies/openzeppelin/contracts/EnumerableSet.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::4 => import "../../dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MockTokenFaucet.sol::5 => import "../../dependencies/openzeppelin/contracts/IMintableERC20.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::418 => "Ownable: new owner is the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::462 => favours the latter half of "APIs should be easy to use and hard to misuse"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2144 => require(ERC721A._exists(tokenId), "ERC721ACommon: Token doesn't exist");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2153 => "ERC721ACommon: Not approved nor owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2349 => "ERC721Redeemer: over allowance for",
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2385 => "ERC721Redeemer: over allowance for",
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2409 => "ERC721Redeemer: not approved nor owner of",
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2854 => "AccessControl: can only renounce roles for self"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3132 => "ERC2981: royalty fee will exceed salePrice"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3162 => "ERC2981: royalty fee will exceed salePrice"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WETH9Mocked.sol::4 => import {WETH9} from "../../dependencies/weth/WETH9.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::4 => import {Ownable} from "../../../../contracts/dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::5 => import {ERC721} from "../../../../contracts/dependencies/openzeppelin/contracts/ERC721.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::6 => import {ERC721Enumerable} from "../../../../contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::7 => import {Pausable} from "../../../../contracts/dependencies/openzeppelin/contracts/Pausable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::10 => import {IWrappedPunks} from "../../../misc/interfaces/IWrappedPunks.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::43 => "PunkWrapper: caller has registered the proxy"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::82 => "PunkWrapper: caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::92 => return "https://wrappedpunks.com:3000/api/punks/metadata/";
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::168 => "Address: unable to send value, recipient may have reverted"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::232 => "Address: low-level call with value failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::250 => "Address: insufficient balance for call"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::275 => "Address: low-level static call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::290 => require(isContract(target), "Address: static call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::310 => "Address: low-level delegate call failed"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::325 => require(isContract(target), "Address: delegate call to non-contract");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::639 => "ERC721A: collection must have a nonzero supply"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::641 => require(maxBatchSize_ > 0, "ERC721A: max batch size must be nonzero");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::664 => require(index < totalSupply(), "ERC721A: global index out of bounds");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::679 => require(index < balanceOf(owner), "ERC721A: owner index out of bounds");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::695 => revert("ERC721A: unable to get token of owner by index");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::721 => "ERC721A: balance query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::729 => "ERC721A: number minted query for the zero address"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::739 => require(_exists(tokenId), "ERC721A: owner query for nonexistent token");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::753 => revert("ERC721A: unable to determine the owner of token");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::789 => "ERC721Metadata: URI query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::813 => require(to != owner, "ERC721A: approval to current owner");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::817 => "ERC721A: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::834 => "ERC721A: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::900 => "ERC721A: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::936 => require(to != address(0), "ERC721A: mint to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::939 => require(quantity <= maxBatchSize, "ERC721A: quantity to mint too high");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::956 => "ERC721A: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::988 => "ERC721A: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::993 => "ERC721A: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::995 => require(to != address(0), "ERC721A: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::1049 => require(_exists(endIndex), "not enough minted yet for this cleanup");
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::1091 => "ERC721A: transfer to non ERC721Receiver implementer"
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/stETH.sol::6 => import {WadRayMath} from "../../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockInitializableImplementation.sol::4 => import {VersionedInitializable} from "../../protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockNToken.sol::4 => import {NToken} from "../../protocol/tokenization/NToken.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockNToken.sol::6 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPToken.sol::4 => import {PToken} from "../../protocol/tokenization/PToken.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPToken.sol::6 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::4 => import {ParaVersionedInitializable} from "../../protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::5 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::6 => import {ReserveConfiguration} from "../../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::7 => import {PoolLogic} from "../../protocol/libraries/logic/PoolLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::8 => import {ReserveLogic} from "../../protocol/libraries/logic/ReserveLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::9 => import {SupplyLogic} from "../../protocol/libraries/logic/SupplyLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::10 => import {MarketplaceLogic} from "../../protocol/libraries/logic/MarketplaceLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::11 => import {BorrowLogic} from "../../protocol/libraries/logic/BorrowLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::12 => import {LiquidationLogic} from "../../protocol/libraries/logic/LiquidationLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::13 => import {AuctionLogic} from "../../protocol/libraries/logic/AuctionLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::14 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::15 => import {IERC20WithPermit} from "../../interfaces/IERC20WithPermit.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::16 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::20 => import {PoolStorage} from "../../protocol/pool/PoolStorage.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::21 => import {FlashClaimLogic} from "../../protocol/libraries/logic/FlashClaimLogic.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::22 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::23 => import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::24 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::25 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::26 => import {ParaReentrancyGuard} from "../../protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::27 => import {IAuctionableERC721} from "../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockPoolCore.sol::28 => import {IReserveAuctionStrategy} from "../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/mocks/upgradeability/MockVariableDebtToken.sol::4 => import {VariableDebtToken} from "../../protocol/tokenization/VariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::4 => import {AccessControl} from "../../dependencies/openzeppelin/contracts/AccessControl.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::5 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::4 => import {Ownable} from "../../dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::5 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::7 => import {InitializableImmutableAdminUpgradeabilityProxy} from "../libraries/paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::8 => import {ParaProxy} from "../libraries/paraspace-upgradeability/ParaProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::9 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::10 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol::11 => import {Errors} from "../../protocol/libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::4 => import {Ownable} from "../../dependencies/openzeppelin/contracts/Ownable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol::6 => import {IPoolAddressesProviderRegistry} from "../../interfaces/IPoolAddressesProviderRegistry.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol::5 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol::6 => import {IPriceOracleSentinel} from "../../interfaces/IPriceOracleSentinel.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol::7 => import {ISequencerOracle} from "../../interfaces/ISequencerOracle.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/helpers/Helpers.sol::4 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::4 => import {DataTypes} from "../../libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::7 => import {IAuctionableERC721} from "../../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol::8 => import {IReserveAuctionStrategy} from "../../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::4 => import {GPv2SafeERC20} from "../../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::5 => import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::6 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::7 => import {IVariableDebtToken} from "../../../interfaces/IVariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::9 => import {UserConfiguration} from "../configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol::10 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::5 => import {IInitializablePToken} from "../../../interfaces/IInitializablePToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::6 => import {IInitializableNToken} from "../../../interfaces/IInitializableNToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::7 => import {IInitializableDebtToken} from "../../../interfaces/IInitializableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::8 => import {IRewardController} from "../../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::9 => import {InitializableImmutableAdminUpgradeabilityProxy} from "../paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::10 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ConfiguratorLogic.sol::12 => import {ConfiguratorInputTypes} from "../types/ConfiguratorInputTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::4 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::5 => import {IFlashClaimReceiver} from "../../../misc/interfaces/IFlashClaimReceiver.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::10 => import "../../../interfaces/INTokenApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol::11 => import {XTokenType, IXTokenType} from "../../../interfaces/IXTokenType.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::4 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::5 => import {IERC721Enumerable} from "../../../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::6 => import {Math} from "../../../dependencies/openzeppelin/contracts/Math.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::7 => import {IScaledBalanceToken} from "../../../interfaces/IScaledBalanceToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::9 => import {ICollateralizableERC721} from "../../../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::10 => import {IPriceOracleGetter} from "../../../interfaces/IPriceOracleGetter.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::11 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::12 => import {UserConfiguration} from "../configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::17 => import {INonfungiblePositionManager} from "../../../dependencies/uniswap/INonfungiblePositionManager.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol::18 => import {XTokenType} from "../../../interfaces/IXTokenType.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::4 => import {IERC20} from "../../../dependencies/openzeppelin/contracts//IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::5 => import {GPv2SafeERC20} from "../../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::6 => import {PercentageMath} from "../../libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::7 => import {WadRayMath} from "../../libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::8 => import {Math} from "../../../dependencies/openzeppelin/contracts/Math.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::9 => import {Helpers} from "../../libraries/helpers/Helpers.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::10 => import {DataTypes} from "../../libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::15 => import {UserConfiguration} from "../../libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::16 => import {ReserveConfiguration} from "../../libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::17 => import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::19 => import {IWETH} from "../../../misc/interfaces/IWETH.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::20 => import {ICollateralizableERC721} from "../../../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::21 => import {IAuctionableERC721} from "../../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::23 => import {PRBMath} from "../../../dependencies/math/PRBMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::24 => import {PRBMathUD60x18} from "../../../dependencies/math/PRBMathUD60x18.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::25 => import {IReserveAuctionStrategy} from "../../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::26 => import {IVariableDebtToken} from "../../../interfaces/IVariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::27 => import {IPriceOracleGetter} from "../../../interfaces/IPriceOracleGetter.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::28 => import {IPoolAddressesProvider} from "../../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol::29 => import {Errors} from "../../libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::4 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::6 => import {IPoolAddressesProvider} from "../../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::7 => import {XTokenType} from "../../../interfaces/IXTokenType.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::8 => import {ICollateralizableERC721} from "../../../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::15 => import {SeaportInterface} from "../../../dependencies/seaport/contracts/interfaces/SeaportInterface.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::16 => import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::17 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::18 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::19 => import {ConsiderationItem, OfferItem} from "../../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::20 => import {ItemType} from "../../../dependencies/seaport/contracts/lib/ConsiderationEnums.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::21 => import {AdvancedOrder, CriteriaResolver, Fulfillment} from "../../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::22 => import {IWETH} from "../../../misc/interfaces/IWETH.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::23 => import {UserConfiguration} from "../configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::24 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::25 => import {IMarketplace} from "../../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::26 => import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::4 => import {GPv2SafeERC20} from "../../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::5 => import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::6 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::7 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::9 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol::16 => import {IXTokenType, XTokenType} from "../../../interfaces/IXTokenType.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::4 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::5 => import {GPv2SafeERC20} from "../../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::6 => import {IVariableDebtToken} from "../../../interfaces/IVariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::7 => import {IReserveInterestRateStrategy} from "../../../interfaces/IReserveInterestRateStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::8 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ReserveLogic.sol::14 => import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::4 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::5 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::6 => import {GPv2SafeERC20} from "../../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::8 => import {INonfungiblePositionManager} from "../../../dependencies/uniswap/INonfungiblePositionManager.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::10 => import {INTokenApeStaking} from "../../../interfaces/INTokenApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::11 => import {ICollateralizableERC721} from "../../../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::12 => import {IAuctionableERC721} from "../../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::14 => import {UserConfiguration} from "../configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::20 => import {XTokenType} from "../../../interfaces/IXTokenType.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol::21 => import {INTokenUniswapV3} from "../../../interfaces/INTokenUniswapV3.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::4 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::5 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::6 => import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::7 => import {GPv2SafeERC20} from "../../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::8 => import {IReserveInterestRateStrategy} from "../../../interfaces/IReserveInterestRateStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::9 => import {IScaledBalanceToken} from "../../../interfaces/IScaledBalanceToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::10 => import {IPriceOracleGetter} from "../../../interfaces/IPriceOracleGetter.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::12 => import {ICollateralizableERC721} from "../../../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::13 => import {IAuctionableERC721} from "../../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::15 => import {SignatureChecker} from "../../../dependencies/looksrare/contracts/libraries/SignatureChecker.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::16 => import {IPriceOracleSentinel} from "../../../interfaces/IPriceOracleSentinel.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::17 => import {ReserveConfiguration} from "../configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::18 => import {UserConfiguration} from "../configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::25 => import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::27 => import {XTokenType, IXTokenType} from "../../../interfaces/IXTokenType.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::29 => import {INonfungiblePositionManager} from "../../../dependencies/uniswap/INonfungiblePositionManager.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::30 => import "../../../interfaces/INTokenApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1117 => "Credit(address token,uint256 amount,bytes orderId)"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol::1137 => 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol::4 => import {BaseUpgradeabilityProxy} from "../../../dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol::85 => "Cannot call fallback function from the proxy admin"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol::4 => import {InitializableUpgradeabilityProxy} from "../../../dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol::5 => import {Proxy} from "../../../dependencies/openzeppelin/upgradeability/Proxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol::6 => import {BaseImmutableAdminUpgradeabilityProxy} from "./BaseImmutableAdminUpgradeabilityProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::10 => import {IParaProxy} from "../../../interfaces/IParaProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::45 => "ParaProxy: Function does not exist"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol::53 => "Contract instance has already been initialized"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol::36 => "Contract instance has already been initialized"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::8 => import {IParaProxy} from "../../../../interfaces/IParaProxy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::13 => uint256(keccak256("paraspace.proxy.implementation.storage")) - 1
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::67 => "ParaProxy: Must be contract owner"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::107 => revert("ParaProxy: Incorrect ProxyImplementationAction");
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::120 => "ParaProxy: No selectors in implementation to cut"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::125 => "ParaProxy: Add implementation can't be address(0)"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::148 => "ParaProxy: Can't add function that already exists"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::161 => "ParaProxy: No selectors in implementation to cut"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::166 => "ParaProxy: Add implementation can't be address(0)"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::189 => "ParaProxy: Can't replace function with same function"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::203 => "ParaProxy: No selectors in implementation to cut"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::209 => "ParaProxy: Remove implementation address must be address(0)"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::229 => "ParaProxy: New implementation has no code"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::262 => "ParaProxy: Can't remove function that doesn't exist"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::267 => "ParaProxy: Can't remove immutable function"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::332 => "ParaProxy: _init is address(0) but_calldata is not empty"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::337 => "ParaProxy: _calldata is empty but _init is not address(0)"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::342 => "ParaProxy: _init address has no code"
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::351 => revert("ParaProxy: _init function reverted");
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/types/DataTypes.sol::4 => import {OfferItem, ConsiderationItem} from "../../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::6 => import {PercentageMath} from "../libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::8 => import {IReserveAuctionStrategy} from "../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::9 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::12 => import {PRBMathUD60x18} from "../../dependencies/math/PRBMathUD60x18.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol::13 => import {PRBMath} from "../../dependencies/math/PRBMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::6 => import {PercentageMath} from "../libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::8 => import {IReserveInterestRateStrategy} from "../../interfaces/IReserveInterestRateStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol::9 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::4 => import "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::5 => import "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::7 => import "../../interfaces/IPoolApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::9 => import "../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::11 => import "../../interfaces/INTokenApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::12 => import {ValidationLogic} from "../libraries/logic/ValidationLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::13 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::16 => import {ReserveLogic} from "../libraries/logic/ReserveLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::17 => import {GenericLogic} from "../libraries/logic/GenericLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::18 => import {UserConfiguration} from "../libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::19 => import {ApeStakingLogic} from "../tokenization/libraries/ApeStakingLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::20 => import "../libraries/logic/BorrowLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::21 => import "../libraries/logic/SupplyLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::4 => import {VersionedInitializable} from "../libraries/paraspace-upgradeability/VersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::5 => import {ReserveConfiguration} from "../libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::6 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::8 => import {PercentageMath} from "../libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::10 => import {ConfiguratorLogic} from "../libraries/logic/ConfiguratorLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::11 => import {ConfiguratorInputTypes} from "../libraries/types/ConfiguratorInputTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::12 => import {IPoolConfigurator} from "../../interfaces/IPoolConfigurator.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol::15 => import {IProtocolDataProvider} from "../../interfaces/IProtocolDataProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::4 => import {ParaVersionedInitializable} from "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::6 => import {ReserveConfiguration} from "../libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::8 => import {ReserveLogic} from "../libraries/logic/ReserveLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::9 => import {SupplyLogic} from "../libraries/logic/SupplyLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::10 => import {MarketplaceLogic} from "../libraries/logic/MarketplaceLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::11 => import {BorrowLogic} from "../libraries/logic/BorrowLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::12 => import {LiquidationLogic} from "../libraries/logic/LiquidationLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::13 => import {AuctionLogic} from "../libraries/logic/AuctionLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::15 => import {IERC20WithPermit} from "../../interfaces/IERC20WithPermit.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::16 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::21 => import {FlashClaimLogic} from "../libraries/logic/FlashClaimLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::22 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::23 => import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::24 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::26 => import {ParaReentrancyGuard} from "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::27 => import {IAuctionableERC721} from "../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol::28 => import {IReserveAuctionStrategy} from "../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::4 => import {ParaVersionedInitializable} from "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::6 => import {ReserveConfiguration} from "../libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::8 => import {ReserveLogic} from "../libraries/logic/ReserveLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::9 => import {SupplyLogic} from "../libraries/logic/SupplyLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::10 => import {MarketplaceLogic} from "../libraries/logic/MarketplaceLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::11 => import {BorrowLogic} from "../libraries/logic/BorrowLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::12 => import {LiquidationLogic} from "../libraries/logic/LiquidationLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::14 => import {IERC20WithPermit} from "../../interfaces/IERC20WithPermit.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::15 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::16 => import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::18 => import {ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationEnums.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::19 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::20 => import {IPoolMarketplace} from "../../interfaces/IPoolMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::24 => import {FlashClaimLogic} from "../libraries/logic/FlashClaimLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::25 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::26 => import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::27 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::29 => import {ParaReentrancyGuard} from "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::30 => import {IAuctionableERC721} from "../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol::31 => import {IReserveAuctionStrategy} from "../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::4 => import {ParaVersionedInitializable} from "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::6 => import {ReserveConfiguration} from "../libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::8 => import {ReserveLogic} from "../libraries/logic/ReserveLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::9 => import {SupplyLogic} from "../libraries/logic/SupplyLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::10 => import {MarketplaceLogic} from "../libraries/logic/MarketplaceLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::11 => import {BorrowLogic} from "../libraries/logic/BorrowLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::12 => import {LiquidationLogic} from "../libraries/logic/LiquidationLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::14 => import {IERC20WithPermit} from "../../interfaces/IERC20WithPermit.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::15 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::16 => import {IPoolParameters} from "../../interfaces/IPoolParameters.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::20 => import {FlashClaimLogic} from "../libraries/logic/FlashClaimLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::21 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::22 => import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::23 => import {IMarketplace} from "../../interfaces/IMarketplace.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::25 => import {ParaReentrancyGuard} from "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::26 => import {IAuctionableERC721} from "../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol::27 => import {IReserveAuctionStrategy} from "../../interfaces/IReserveAuctionStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol::4 => import {UserConfiguration} from "../libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol::5 => import {ReserveConfiguration} from "../libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol::6 => import {ReserveLogic} from "../libraries/logic/ReserveLogic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/DelegationAwarePToken.sol::5 => import {IDelegationToken} from "../../interfaces/IDelegationToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::5 => import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::6 => import {IERC1155} from "../../dependencies/openzeppelin/contracts/IERC1155.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::7 => import {IERC721Metadata} from "../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::8 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::9 => import {GPv2SafeERC20} from "../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::10 => import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::11 => import {VersionedInitializable} from "../libraries/paraspace-upgradeability/VersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::16 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::17 => import {IInitializableNToken} from "../../interfaces/IInitializableNToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::20 => import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol::21 => import {MintableIncentivizedERC721} from "./base/MintableIncentivizedERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::5 => import {ApeCoinStaking} from "../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::7 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::8 => import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::9 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::11 => import {PercentageMath} from "../libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::12 => import "../../interfaces/INTokenApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::25 => uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol::4 => import {ApeCoinStaking} from "../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol::4 => import {ApeCoinStaking} from "../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::5 => import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::6 => import {IERC1155} from "../../dependencies/openzeppelin/contracts/IERC1155.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::7 => import {IERC721Metadata} from "../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::8 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::9 => import {GPv2SafeERC20} from "../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::10 => import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::11 => import {IMoonBirdBase} from "../../dependencies/erc721-collections/IMoonBird.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::12 => import {IMoonBird} from "../../dependencies/erc721-collections/IMoonBird.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::17 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol::100 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::5 => import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::6 => import {IERC1155} from "../../dependencies/openzeppelin/contracts/IERC1155.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::7 => import {IERC721Metadata} from "../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::8 => import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::9 => import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::10 => import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::16 => import {INonfungiblePositionManager} from "../../dependencies/uniswap/INonfungiblePositionManager.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol::19 => import {INTokenUniswapV3} from "../../interfaces/INTokenUniswapV3.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::5 => import {GPv2SafeERC20} from "../../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::6 => import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::7 => import {VersionedInitializable} from "../libraries/paraspace-upgradeability/VersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::12 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::13 => import {IInitializablePToken} from "../../interfaces/IInitializablePToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::14 => import {ScaledBalanceTokenBaseERC20} from "./base/ScaledBalanceTokenBaseERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PToken.sol::36 => "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::6 => import {INTokenApeStaking} from "../../interfaces/INTokenApeStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::9 => import {ApeCoinStaking} from "../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::12 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol::13 => import {IScaledBalanceToken} from "../../interfaces/IScaledBalanceToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::5 => import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::6 => import {VersionedInitializable} from "../libraries/paraspace-upgradeability/VersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::10 => import {IRewardController} from "../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::11 => import {IInitializableDebtToken} from "../../interfaces/IInitializableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::12 => import {IVariableDebtToken} from "../../interfaces/IVariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol::15 => import {ScaledBalanceTokenBaseERC20} from "./base/ScaledBalanceTokenBaseERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::4 => import {Context} from "../../../dependencies/openzeppelin/contracts/Context.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::5 => import {Errors} from "../../libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::6 => import {VersionedInitializable} from "../../libraries/paraspace-upgradeability/VersionedInitializable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::7 => import {ICreditDelegationToken} from "../../../interfaces/ICreditDelegationToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/DebtTokenBase.sol::27 => "DelegationWithSig(address delegatee,uint256 value,uint256 nonce,uint256 deadline)"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/EIP712Base.sol::13 => "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::4 => import {Context} from "../../../dependencies/openzeppelin/contracts/Context.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::5 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::6 => import {IERC20Detailed} from "../../../dependencies/openzeppelin/contracts/IERC20Detailed.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::7 => import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::8 => import {WadRayMath} from "../../libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::9 => import {Errors} from "../../libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::10 => import {IRewardController} from "../../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::11 => import {IPoolAddressesProvider} from "../../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::13 => import {IACLManager} from "../../../interfaces/IACLManager.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol::4 => import {IRewardController} from "../../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::4 => import {Context} from "../../../dependencies/openzeppelin/contracts/Context.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::5 => import {Strings} from "../../../dependencies/openzeppelin/contracts/Strings.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::6 => import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::7 => import {IERC165} from "../../../dependencies/openzeppelin/contracts/IERC165.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::8 => import {IERC721Metadata} from "../../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::9 => import {IERC721Receiver} from "../../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::10 => import {IERC721Enumerable} from "../../../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::11 => import {ICollateralizableERC721} from "../../../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::12 => import {IAuctionableERC721} from "../../../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::13 => import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::14 => import {WadRayMath} from "../../libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::15 => import {Errors} from "../../libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::16 => import {IRewardController} from "../../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::17 => import {IPoolAddressesProvider} from "../../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::19 => import {IACLManager} from "../../../interfaces/IACLManager.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::20 => import {DataTypes} from "../../libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::21 => import {ReentrancyGuard} from "../../../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::22 => import {MintableERC721Logic, UserState, MintableERC721Data} from "../libraries/MintableERC721Logic.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::197 => "ERC721: approve caller is not owner nor approved for all"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::215 => "ERC721: approved query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::261 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::298 => "ERC721: transfer caller is not owner nor approved"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::356 => "ERC721: operator query for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::596 => "ERC721Enumerable: owner index out of bounds"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::620 => "ERC721Enumerable: global index out of bounds"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::4 => import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::5 => import {Errors} from "../../libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::6 => import {WadRayMath} from "../../libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol::8 => import {IScaledBalanceToken} from "../../../interfaces/IScaledBalanceToken.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::3 => import {ApeCoinStaking} from "../../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::4 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::5 => import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::6 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::8 => import {DataTypes} from "../../libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::9 => import {PercentageMath} from "../../libraries/math/PercentageMath.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::10 => import {Math} from "../../../dependencies/openzeppelin/contracts/Math.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::3 => import {ApeCoinStaking} from "../../../dependencies/yoga-labs/ApeCoinStaking.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::4 => import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::5 => import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::6 => import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::7 => import "../../../interfaces/IRewardController.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::8 => import "../../libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::10 => import {Errors} from "../../libraries/helpers/Errors.sol";
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::90 => "ERC721: transfer from incorrect owner"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::92 => require(to != address(0), "ERC721: transfer to the zero address");
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::379 => "ERC721: startAuction for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol::397 => "ERC721: endAuction for nonexistent token"
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::4 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::5 => import {IRewardsController} from "./interfaces/IRewardsController.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::6 => import {IUiIncentiveDataProvider} from "./interfaces/IUiIncentiveDataProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::8 => import {IncentivizedERC20} from "../protocol/tokenization/base/IncentivizedERC20.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::9 => import {UserConfiguration} from "../protocol/libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::10 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::11 => import {IERC20Detailed} from "../dependencies/openzeppelin/contracts/IERC20Detailed.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiIncentiveDataProvider.sol::12 => import {IEACAggregatorProxy} from "./interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::4 => import {IERC20Detailed} from "../dependencies/openzeppelin/contracts/IERC20Detailed.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::5 => import {IERC721Metadata} from "../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::6 => import {IERC721} from "../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::7 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::8 => import {IUiPoolDataProvider} from "./interfaces/IUiPoolDataProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::10 => import {IParaSpaceOracle} from "../interfaces/IParaSpaceOracle.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::12 => import {ICollateralizableERC721} from "../interfaces/ICollateralizableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::13 => import {IAuctionableERC721} from "../interfaces/IAuctionableERC721.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::15 => import {IVariableDebtToken} from "../interfaces/IVariableDebtToken.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::16 => import {WadRayMath} from "../protocol/libraries/math/WadRayMath.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::17 => import {ReserveConfiguration} from "../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::18 => import {UserConfiguration} from "../protocol/libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::19 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::20 => import {DefaultReserveInterestRateStrategy} from "../protocol/pool/DefaultReserveInterestRateStrategy.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::21 => import {IEACAggregatorProxy} from "./interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::22 => import {IERC20DetailedBytes} from "./interfaces/IERC20DetailedBytes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::24 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::25 => import {IUniswapV3OracleWrapper} from "../interfaces/IUniswapV3OracleWrapper.sol";
2022-11-paraspace/paraspace-core/contracts/ui/UiPoolDataProvider.sol::26 => import {UinswapV3PositionData} from "../interfaces/IUniswapV3PositionInfoProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::4 => import {OwnableUpgradeable} from "../dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::5 => import {IERC20} from "../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::10 => import {ReserveConfiguration} from "../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::11 => import {UserConfiguration} from "../protocol/libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::12 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::13 => import {Helpers} from "../protocol/libraries/helpers/Helpers.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::14 => import {ReentrancyGuard} from "../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::15 => import {SafeERC20} from "../dependencies/openzeppelin/contracts/SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::110 => "msg.value is less than repayment amount"
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::4 => import {OwnableUpgradeable} from "../dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::6 => import {ReserveConfiguration} from "../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::7 => import {UserConfiguration} from "../protocol/libraries/configuration/UserConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::8 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::11 => import {IERC721} from "../dependencies/openzeppelin/contracts/IERC721.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::12 => import {IERC721Receiver} from "../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::14 => import {IWrappedPunks} from "../misc/interfaces/IWrappedPunks.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol::17 => import {ReentrancyGuard} from "../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::4 => import {Address} from "../dependencies/openzeppelin/contracts/Address.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::5 => import {IERC20} from "../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::7 => import {IPoolAddressesProvider} from "../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::9 => import {GPv2SafeERC20} from "../dependencies/gnosis/contracts/GPv2SafeERC20.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::10 => import {ReserveConfiguration} from "../protocol/libraries/configuration/ReserveConfiguration.sol";
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::11 => import {DataTypes} from "../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IERC20DetailedBytes.sol::4 => import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::6 => import {IEACAggregatorProxy} from "../../ui/interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IRewardsController.sol::7 => import {RewardsDataTypes} from "../libraries/RewardsDataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiIncentiveDataProvider.sol::4 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::4 => import {IPoolAddressesProvider} from "../../interfaces/IPoolAddressesProvider.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IUiPoolDataProvider.sol::5 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWETHGateway.sol::4 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/interfaces/IWPunkGateway.sol::4 => import {DataTypes} from "../../protocol/libraries/types/DataTypes.sol";
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::4 => import {ITransferStrategyBase} from "../interfaces/ITransferStrategyBase.sol";
2022-11-paraspace/paraspace-core/contracts/ui/libraries/RewardsDataTypes.sol::5 => import {IEACAggregatorProxy} from "../../ui/interfaces/IEACAggregatorProxy.sol";
2022-11-paraspace/paraspace-core/foundry_test/NFTFloorOracle.t.sol::4 => import "../contracts/misc/NFTFloorOracle.sol";
2022-11-paraspace/paraspace-core/foundry_test/NFTFloorOracle.t.sol::5 => import "../foundry_libs/ds-test/src/test.sol";
```
#### Tools used
Manual






### 6th BUG
Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: 
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

Example
🤦 Bad:

uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
🚀 Good:

uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::217 => // Execution part 1/2
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::227 => // Execution part 2/2
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::269 => // Execution part 1/2
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::281 => // Execution part 2/2
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::323 => // Execution part 1/2
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::326 => // Execution part 2/2
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::446 => // 2. Royalty fee
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::499 => // 2. Royalty fee
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/RoyaltyFeeManager.sol::42 => // 2. If the receiver is address(0), fee is null, check if it supports the ERC2981 interface
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol::25 => // https://ethereum.stackexchange.com/questions/83174/is-it-best-practice-to-check-signature-malleability-in-ecrecover
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol::26 => // https://crypto.iacr.org/2019/affevents/wac/medias/Heninger-BiasedNonceSense.pdf
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeSetter.sol::135 => * 2: setter can be set using owner()
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeSetter.sol::137 => * 4: setter cannot be set, nor support for ERC2981
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC721.sol::39 => // https://docs.openzeppelin.com/contracts/2.x/api/token/erc721#IERC721-safeTransferFrom
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::124 => /// See https://ethereum.stackexchange.com/a/96594/24693.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::334 => // This works because 2^(191-ip) = 2^ip / 2^191, where "ip" is the integer part "2^n".
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::361 => if (x >= 2**8) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::365 => if (x >= 2**4) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::369 => if (x >= 2**2) {
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::381 => /// @dev Credit to Remco Bloemen under MIT license https://xn--2-umb.com/21/muldiv.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::401 => // variables such that product = prod1 * 2^256 + prod0.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::493 => ///     2. (x * y) % SCALE >= SCALE / 2
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::194 => /// @dev See https://ethereum.stackexchange.com/q/79903/24693.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::206 => // This works because 2^(-x) = 1/2^x.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::208 => // 2^59.794705707972522262 is the maximum number whose inverse does not truncate down to zero.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::218 => // 2^192 doesn't fit within the 192.64-bit format used internally in this function.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::501 => // Calculate the integer part of the logarithm and add it to the result and finally calculate y = x * 2^(-n).
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::508 => // This is y = x * 2^(-n).
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::526 => // Corresponds to z/2 on Wikipedia.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::114 => /// @dev See https://ethereum.stackexchange.com/q/79903/24693.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::123 => // 2^192 doesn't fit within the 192.64-bit format used internally in this function.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::371 => // Calculate the integer part of the logarithm and add it to the result and finally calculate y = x * 2^(-n).
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::378 => // This is y = x * 2^(-n).
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::396 => // Corresponds to z/2 on Wikipedia.
2022-11-paraspace/paraspace-core/contracts/dependencies/math/SqrtLib.sol::12 => // y in [256, 256*2^16). We ensure y>= 256 so that the relative difference
2022-11-paraspace/paraspace-core/contracts/dependencies/math/SqrtLib.sol::41 => // and when x = 256 or 1/256. In the worst case, this needs seven Babylonian iterations.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::53 => * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Context.sol::20 => this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::161 => // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::18 => * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC20.sol::81 => * be displayed to a user as `5,05` (`505 / 10 ** 2`).
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC20.sol::51 => * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::35 => // (a + b) / 2 can overflow.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::36 => return (a & b) + (a ^ b) / 2;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::52 => * @dev Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv)
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::63 => // variables such that product = prod1 * 2^256 + prod0.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::167 => // This gives `2**k < a <= 2**(k+1)` → `2**(k/2) <= sqrt(a) < 2 ** (k/2+1)`.
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::168 => // Using an algorithm similar to the msb computation, we are able to compute `result = 2**(k/2)` which is a
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::53 => * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/ContextUpgradeable.sol::34 => * See https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/IERC20Upgradeable.sol::62 => * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol::92 => * See https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/PausableUpgradeable.sol::114 => * See https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/ReentrancyGuardUpgradeable.sol::72 => * See https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::118 => * 2. Additional recipients arr offset == 0x240
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::120 => * 4. BasicOrderType between 0 and 23 (i.e. < 24)
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::410 => * 2. Write a ReceivedItem struct for the primary consideration
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::576 => * 4. Hash packed array of ConsiderationItem EIP712 hashes:
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::702 => * 2. Calculate hash of array of EIP712 hashes and write the
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::207 => uint256 constant BasicOrder_basicOrderType_range = 0x18; // 24 values
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::303 => uint256 constant NoContract_error_length = 0x24; // 4 + 32 == 36
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::12 => // 2: no partial fills, only offerer or zone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::27 => // 2: no partial fills, only offerer or zone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::33 => // 4: no partial fills, anyone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::45 => // 8: no partial fills, anyone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::81 => // 20: no partial fills, anyone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::84 => // 21: partial fills supported, anyone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::87 => // 22: no partial fills, only offerer or zone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::90 => // 23: partial fills supported, only offerer or zone can execute
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::102 => // 2: provide ERC20 item to receive offered ERC721 item.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::108 => // 4: provide ERC721 item to receive offered ERC20 item.
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::123 => // 2: ERC721 items
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::129 => // 4: ERC721 items where a number of tokenIds are supported
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::57 => uint256 constant ERC20_transferFrom_length = 0x64; // 4 + 32 * 3 == 100
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::72 => uint256 constant ERC1155_safeTransferFrom_length = 0xc4; // 4 + 32 * 6 == 196
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::91 => uint256 constant ERC721_transferFrom_length = 0x64; // 4 + 32 * 3 == 100
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::99 => uint256 constant NoContract_error_length = 0x24; // 4 + 32 == 36
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::114 => // 4 + 32 * 5 == 164
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::129 => // 4 + 32 * 4 == 132
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IUniswapV3PoolState.sol::19 => /// is the lower 4 bits. Used as the denominator of a fraction of the swap fee, e.g. 4 means 1/4th of the swap fee.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::13 => /// @dev Credit to Remco Bloemen under MIT license https://xn--2-umb.com/21/muldiv
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::21 => // Compute the product mod 2**256 and mod 2**256 - 1
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::24 => // variables such that product = prod1 * 2**256 + prod0
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::42 => // Make sure the result is less than 2**256.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::76 => // to flip `twos` such that it is 2**256 / twos.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::83 => // Invert denominator mod 2**256
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::85 => // modulo 2**256 such that denominator * inv = 1 mod 2**256.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::87 => // correct for four bits. That is, denominator * inv = 1 mod 2**4
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::92 => inv *= 2 - denominator * inv; // inverse mod 2**8
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::97 => inv *= 2 - denominator * inv; // inverse mod 2**256
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::101 => // correct result modulo 2**256. Since the precoditions guarantee
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::102 => // that the outcome is less than 2**256, this is the final result.
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::190 => i = (l + r) / 2;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SafeCast.sol::25 => require(y < 2**255);
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/TickMath.sol::22 => /// @notice Calculates sqrt(1.0001^tick) * 2^96
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/TickMath.sol::241 => int256 log_sqrt10001 = log_2 * 255738958999603826347141; // 128.128 number
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolState.sol::19 => /// is the lower 4 bits. Used as the denominator of a fraction of the swap fee, e.g. 4 means 1/4th of the swap fee.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::13 => /// @dev Credit to Remco Bloemen under MIT license https://xn--2-umb.com/21/muldiv
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::20 => // Compute the product mod 2**256 and mod 2**256 - 1
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::23 => // variables such that product = prod1 * 2**256 + prod0
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::41 => // Make sure the result is less than 2**256.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::75 => // to flip `twos` such that it is 2**256 / twos.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::82 => // Invert denominator mod 2**256
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::84 => // modulo 2**256 such that denominator * inv = 1 mod 2**256.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::86 => // correct for four bits. That is, denominator * inv = 1 mod 2**4
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::91 => inv *= 2 - denominator * inv; // inverse mod 2**8
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::96 => inv *= 2 - denominator * inv; // inverse mod 2**256
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::100 => // correct result modulo 2**256. Since the precoditions guarantee
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::101 => // that the outcome is less than 2**256, this is the final result.
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::169 => i = (l + r) / 2;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SafeCast.sol::25 => require(y < 2**255);
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TickMath.sol::19 => /// @notice Calculates sqrt(1.0001^tick) * 2^96
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TickMath.sol::235 => int256 log_sqrt10001 = log_2 * 255738958999603826347141; // 128.128 number
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::520 => bytes32 /*itemHash*/,
2022-11-paraspace/paraspace-core/contracts/dependencies/yoga-labs/ApeCoinStaking.sol::798 => return (position.stakedAmount * rewards.rewardsPerHour * 24) / pool.stakedAmount;
2022-11-paraspace/paraspace-core/contracts/interfaces/IERC20WithPermit.sol::15 => * https://github.com/ethereum/EIPs/blob/8a34d644aacf0f9f8f00815307fd7dd5da07655f/EIPS/eip-2612.md
2022-11-paraspace/paraspace-core/contracts/interfaces/IPToken.sol::91 => * https://github.com/ethereum/EIPs/blob/8a34d644aacf0f9f8f00815307fd7dd5da07655f/EIPS/eip-2612.md


2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::247 => ) * 2**96) / 1E9
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::259 => ) * 2**96) / 1E9
2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol::271 => ) * 2**96) /
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::26 => this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::634 => * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::865 => // this feature: see https://github.com/ethereum/solidity/issues/4637
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::819 => * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::742 => * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::1168 => ContributorAmount[] memory /*_contributors*/,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::1169 => address /*_vrfCoordinator*/,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::1170 => address /*_linkTokenAddress*/,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::11 => this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::1142 => * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::500 => // https://gist.github.com/dievardump/483eb43bc6ed30b14f01e01842e3339b/
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::898 => * Assumes that the maximum token id cannot exceed 2**256 - 1 (max value of uint256).
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1428 => // `tokenId` has a maximum limit of 2**256.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1607 => // Counter overflow is incredibly unrealistic as tokenId would have to be 2**256.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1688 => // Counter overflow is incredibly unrealistic as `tokenId` would have to be 2**256.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2206 => uint256 bucket = index / 256;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2230 => uint256 bucket = index / 256;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2239 => uint256 bucket = index / 256;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2595 => // (a + b) / 2 can overflow, so we distribute.
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2596 => return (a / 2) + (b / 2) + (((a % 2) + (b % 2)) / 2);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2958 => * https://forum.openzeppelin.com/t/iterating-over-elements-on-enumerableset-in-openzeppelin-contracts/2296[forum post]
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3208 => address payable /*beneficiary*/,
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::152 => * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol::42 => *  (1+x)^n = 1+n*x+[n/2*(n-1)]*x^2+[n/6*(n-1)*(n-2)*x^3...
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol::8 => * , inspired by the OpenZeppelin upgradeability proxy pattern
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol::6 => * , inspired by the OpenZeppelin Initializable contract
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol::6 => * , inspired by the OpenZeppelin Initializable contract
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::269 => // 2, send cash part to xTokenAddress
2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol::288 => // 4, deposit bakc pool
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::24 => // Returns amount of stETH corresponding to 10**27 stETH shares.
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::25 => // The 10**27 is picked to provide the same precision as the ParaSpace
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol::26 => // liquidity index, which is in RAY (10**27).
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::23 => // Returns amount of stETH corresponding to 10**27 stETH shares.
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::24 => // The 10**27 is picked to provide the same precision as the ParaSpace
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol::25 => // liquidity index, which is in RAY (10**27).
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol::17 => * , inspired by the Openzeppelin ERC20 implementation
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol::26 => * , inspired by the Openzeppelin ERC721 implementation
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol::161 => //2 send incentive to caller
2022-11-paraspace/paraspace-core/contracts/ui/WalletBalanceProvider.sol::15 => * , influenced by https://github.com/wbobeirne/eth-balance-checker/blob/master/contracts/BalanceChecker.sol
2022-11-paraspace/paraspace-core/foundry_test/NFTFloorOracle.t.sol::46 => uint256 pivot = arr[uint256(left + (right - left) / 2)];
```
#### Tools used
Manual