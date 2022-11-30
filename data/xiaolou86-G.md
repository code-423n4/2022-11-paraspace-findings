1. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow.

File: paraspace-core/contracts/misc/NFTFloorOracle.sol

229:         for (uint256 i = 0; i < _assets.length; i++) {

291:         for (uint256 i = 0; i < _assets.length; i++) {

321:         for (uint256 i = 0; i < _feeders.length; i++) {

413:         for (uint256 i = 0; i < feederSize; i++) {

421:                     validNum++;

442:             while (arr[uint256(i)] < pivot) i++;

449:                 i++;


File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

95:         for (uint256 i = 0; i < assets.length; i++) {

197:         for (uint256 i = 0; i < assets.length; i++) {


File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:         for (uint256 index = 0; index < tokenIds.length; index++) {

210:         for (uint256 index = 0; index < tokenIds.length; index++) {


File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38:         for (i = 0; i < params.nftTokenIds.length; i++) {

56:         for (i = 0; i < params.nftTokenIds.length; i++) {


File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

371:             for (uint256 index = 0; index < totalBalance; index++) {

466:         for (uint256 index = 0; index < totalBalance; index++) {


File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:         for (uint256 i = 0; i < marketplaceIds.length; i++) {

284:         for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:         for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:         for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {


File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58:         for (uint16 i = 0; i < params.reservesCount; i++) {

104:         for (uint256 i = 0; i < assets.length; i++) {


File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:             for (uint256 index = 0; index < params.tokenData.length; index++) {

195:         for (uint256 index = 0; index < params.tokenData.length; index++) {


File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

131:         for (uint256 index = 0; index < amount; index++) {

212:             for (uint256 index = 0; index < tokenIds.length; index++) {

465:             for (uint256 index = 0; index < tokenIds.length; index++) {

910:                 for (uint256 index = 0; index < tokenIds.length; index++) {

1034:         for (uint256 i = 0; i < params.nftTokenIds.length; i++) {


File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72:         for (uint256 index = 0; index < _nfts.length; index++) {

103:         for (uint256 index = 0; index < _nfts.length; index++) {

138:         for (uint256 index = 0; index < _nftPairs.length; index++) {

164:                 actualTransferAmount++;

172:         for (uint256 index = 0; index < actualTransferAmount; index++) {

199:         for (uint256 index = 0; index < _nftPairs.length; index++) {

215:         for (uint256 index = 0; index < _nftPairs.length; index++) {

279:         for (uint256 index = 0; index < _nfts.length; index++) {

289:         for (uint256 index = 0; index < _nftPairs.length; index++) {

305:         for (uint256 index = 0; index < _nftPairs.length; index++) {


File: paraspace-core/contracts/protocol/pool/PoolCore.sol

634:         for (uint256 i = 0; i < reservesListCount; i++) {

638:                 droppedReservesCount++;


File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

134:             ps._reservesCount++;


File: paraspace-core/contracts/protocol/tokenization/NToken.sol

106:             for (uint256 index = 0; index < tokenIds.length; index++) {

145:         for (uint256 i = 0; i < ids.length; i++) {


File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

107:         for (uint256 index = 0; index < tokenIds.length; index++) {


File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:             for (uint256 index = 0; index < tokenIds.length; index++) {

97:         for (uint256 index = 0; index < tokenIds.length; index++) {


File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493:         for (uint256 index = 0; index < tokenIds.length; index++) {


File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213:         for (uint256 index = 0; index < totalBalance; index++) {


File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:         for (uint256 index = 0; index < tokenData.length; index++) {

234:                 collateralizedTokens++;

280:         for (uint256 index = 0; index < tokenIds.length; index++) {

304:             balanceToBurn++;

313:                 burntCollateralizedTokens++;


File: paraspace-core/contracts/ui/WPunkGateway.sol

82:         for (uint256 i = 0; i < punkIndexes.length; i++) {

109:         for (uint256 i = 0; i < punkIndexes.length; i++) {

113:         for (uint256 i = 0; i < punkIndexes.length; i++) {

136:         for (uint256 i = 0; i < punkIndexes.length; i++) {

174:         for (uint256 i = 0; i < punkIndexes.length; i++) {



2. [GAS-8] ++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too).

Missing two cases in the following:

File: paraspace-core/contracts/misc/NFTFloorOracle.sol

443:             while (pivot < arr[uint256(j)]) j--;

450:                 j--;