# QA Report
## Finding Summary
||Issue|Instances|
|-|-|-|
|[NC-01]|Inconsistent Leading Space In Comments|49|
|[NC-02]|Inconsistent Capitalization In Comments|48|
|[NC-03]|Long Lines (> 120 Characters)|18|
|[NC-04]|Order of Functions Not Compliant With Solidity Docs|18|
|[NC-05]|File Layout Out of Order|8|
|[NC-06]|`TODO` Left In Production Code|3|
|[NC-07]|Underscore Notation Not Used / Not Used Consistently|1|
|[NC-08]|Out of Place And Inconsistent *params* Comment|1|
|[NC-09]|Dead Comment Through Use of `////`|1|
|[NC-10]|Empty Comment|1|
|[NC-11]|Comment Used Over NatSpec|1|

### [NC-01] Inconsistent Leading Space In Comments

Most comments in the codebase start with a leading space. There are a few comments that void the general styling by ignoring a leading space. It is best to maintain consistant styling.

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [8](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L8), [10](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L10), [11](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L11), [13](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L13), [404](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L404), [408](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L408), [412](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412).
```solidity
8:	//we need to deploy 3 oracles at least
10:	//expirationPeriod at least the interval of client to feed data(currently 6h=21600s/12=1800 in mainnet)
11:	//we do not accept price lags behind to much
13:	//reject when price increase/decrease 1.5 times more than original value
404:	//first time just use the feeding value
408:	//use memory here so allocate with maximum length
412:	//aggeregate with price from all feeders
```

*/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol*
Links: [314](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L314).
```solidity
314:	vars.userGlobalDebt, //in base currency
```

*/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol*
Links: [140](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L140), [141](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L141), [344](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L344), [345](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L345), [405](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L405), [406](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L406).
```solidity
140:	//currently don't need to update state for erc721
141:	//reserve.updateState(reserveCache);
344:	//currently don't need to update state for erc721
345:	//reserve.updateState(reserveCache);
405:	//currently don't need to update state for erc721
406:	//reserve.updateState(reserveCache);
```

*/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol*
Links: [353](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L353), [573](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L573), [685](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L685), [794](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L794).
```solidity
353:	//add the current already borrowed amount to the amount requested to calculate the total collateral needed.
573:	//if collateral isn't enabled as collateral by user, it cannot be liquidated
```

*/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol*
Links: [155](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L155), [171](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L171), [214](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L214), [304](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L304).
```solidity
155:	//only partially withdraw need user's BAKC
171:	////transfer BAKC back for user
214:	//transfer BAKC back for user
304:	//transfer BAKC back for user
```

*/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol*
Links: [258](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L258).
```solidity
258:	//solhint-disable-next-line max-line-length
```

*/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol*
Links: [101](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L101), [103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L103), [119](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L119), [161](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L161), [176](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L176).
```solidity
101:	//1 unstake all position
103:	//1.1 unstake Main pool position
119:	//1.2 unstake bakc pool position
161:	//2 send incentive to caller
176:	//3 repay and supply
```

### [NC-02] Inconsistent Capitalization In Comments

Most comments in the codebase start with a non-capital letter. There are a few comments that void the general styling by ignoring starting capitalized. It is best to maintain consistant styling.

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [17](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L17), [19](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L19).
```solidity
17:	// Expiration Period for each feed price
19:	// Maximum deviation allowed between two consecutive oracle prices
```

*/paraspace-core/contracts/misc/ParaSpaceOracle.sol*
Links: [24](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L24).
```solidity
24:	// Map of asset price sources (asset => priceSource)
```

*/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol*
Links: [21](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L21), [24](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L24), [27](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L27), [30](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L30).
```solidity
21:	// Identifier of the ParaSpace Market
24:	// Map of registered addresses (identifier => registeredAddress)
27:	// Map of marketplace contracts (id => address)
30:	// Main identifiers
```

*/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol*
Links: [66](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L66), [145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L145), [232](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L232), [245](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L245), [385](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L385), [395](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L395). [450](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L450), [530](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L530), [536](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L536), [539](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L539), [549](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L549).
```solidity
66:	// See `IPool` for descriptions
145:	// Auction related
232:	// If the collateral being liquidated is equal to the user balance,
245:	// Transfer fee to treasury if it is non-zero
385:	// If the collateral being liquidated is equal to the user balance,
395:	// Transfer fee to treasury if it is non-zero
450:	// Burn the equivalent amount of xToken, sending the underlying to the liquidator
530:	// Transfers the debt asset being repaid to the xToken, where the liquidity is kept
536:	// Handle payment
539:	// Burn borrower's debt token
549:	// Update borrow & supply rate
```

*/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol*
Links: [189](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L189), [446](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L446).
```solidity
189:	// Once we encounter a listing using WETH, then we convert all our ethLeft to WETH
446:	// No re-entrancy because it sent to our contract address
```

*/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol*
Links: [29](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L29).
```solidity
29:	// See `IPool` for descriptions
```

*/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol*
Links: [34](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L34).
```solidity
34:	// See `IPool` for descriptions
```

*/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol*
Links: [43](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L43), [44](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L44), [47](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L47), [48](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L48).
```solidity
43:	// Factor to apply to "only-variable-debt" liquidity rate to get threshold for rebalancing, expressed in bps
44:	// A value of 0.9e4 results in 90%
47:	// Minimum health factor allowed under any circumstance
48:	// A value of 0.95e18 results in 0.95
```

*/paraspace-core/contracts/protocol/pool/PoolCore.sol*
Links: [168](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L168), [642](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L642).
```solidity
168:	// Need to accommodate ERC721 and ERC1155 here
642:	// Reduces the length of the reserves array by `droppedReservesCount`
```

*/paraspace-core/contracts/protocol/pool/PoolCore.sol*
Links: [792](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L792).
```solidity
792:	// This function is necessary when receive erc721 from looksrare
```

*/paraspace-core/contracts/protocol/tokenization/NToken.sol*
Links: [221](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L221).
```solidity
221:	// Intentionally left blank
```

*/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol*
Links: [33](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L33).
```solidity
33:	// Intentionally left blank
```

*/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol*
Links: [19](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L19), [21](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L21), [23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L23), [25](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L25), [27](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L27), [29](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L29), [31](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L31), [33](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L33). [35](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L35), [37](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L37), [39](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L39) [520](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L520), [526](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L526), [534](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L534), [549](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L549), [555](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L555), [563](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L563).
```solidity
19:	// Token name
21:	// Token symbol
23:	// Mapping from token ID to owner address
25:	// Mapping from owner to list of owned token IDs
27:	// Mapping from token ID to index of the owner tokens list
29:	// Array with all token ids, used for enumeration
31:	// Mapping from token id to position in the allTokens array
33:	// Map of users address and their state data (userAddress => userStateData)
35:	// Mapping from token ID to approved address
37:	// Mapping from owner to operator approvals
39:	// Map of allowances (delegator => delegatee => allowanceAmount)
520:	// To prevent a gap in from's tokens array, we store the last token in the index of the token to delete, and
526:	// When the token to delete is the last token, the swap operation is unnecessary
534:	// This also deletes the contents at the last position of the array
549:	// To prevent a gap in the tokens array, we store the last token in the index of the token to delete, and
555:	// When the token to delete is the last token, the swap operation is unnecessary. However, since this occurs so
563:	// This also deletes the contents at the last position of the array
```

### [NC-03] Long Lines (> 120 Characters)

Lines with greater length than 120 characters are used. The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-lengthhttps://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) suggests that all lines should be 120 characters or less in width.

#### Findings:

*/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol*
Links: [11](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L11).
```solidity
11:	import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

*paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol*
Links: [11](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L11).
```solidity
11:	import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

*paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol*
Links: [12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L12).
```solidity
12:	import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

*paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol*
Links: [7](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L7), [292](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L292), [338](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L338).
```solidity
7:	import {InitializableImmutableAdminUpgradeabilityProxy} from "../libraries/paraspace-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol";
292:	* @notice Internal function to update the implementation of a specific proxied component of the protocol that uses ParaProxy.
338:	* @dev It reverts if the registered address with the given id is not `InitializableImmutableAdminUpgradeabilityProxy`
```

*paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol*
Links: [94](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L94).
```solidity
94:	* @notice Function to end auction on an ERC721 of a position if its ERC721 Health Factor increases back to above `AUCTION_RECOVERY_HEALTH_FACTOR`.
```

*paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol*
Links: [157](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L157).
```solidity
157:	// if amount user is sending is less than payback amount (debt), update the payback amount to what the user is sending
```

*paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol*
Links: [603](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603), [655](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L655).
```solidity
603:	* @return The actual debt that is getting liquidated. If liquidation amount passed in by the liquidator is greater then the total user debt, then use the user total debt as the actual debt getting liquidated. If the user total debt is greater than the liquidation amount getting passed in by the liquidator, then use the liquidation amount the user is passing in.
655:	* @return The maximum amount that is possible to liquidate given all the liquidation constraints (user balance, close factor)
```

*paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol*
Links: [21](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L21).
```solidity
21:	import {AdvancedOrder, CriteriaResolver, Fulfillment} from "../../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

*paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol*
Links: [1137](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1137).
```solidity
1137:	0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
```

*paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol*
Links: [100](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L100).
```solidity
76:	* @notice Overrides the _transfer from NToken to withdraw all staked and pending rewards before transfer the asset
100:	* @notice Overrides the burn from NToken to withdraw all staked and pending rewards before burning the NToken on liquidation/withdraw
```

*paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol*
Links: [48](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L48), [93](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L93), [95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L95).
```solidity
48:	* @notice Withdraw staked ApeCoin from the BAYC pool.  If withdraw is total staked amount, performs an automatic claim.
93:	* @notice Withdraw staked ApeCoin from the Pair pool.  If withdraw is total staked amount, performs an automatic claim.
95:	* @dev if pairs have split ownership and BAKC is attempting a withdraw, the withdraw must be for the total staked amount
```

*paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol*
Links: [530](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L530).
```solidity
530:	erc721Data.ownedTokens[from][tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
```

*paraspace-core/contracts/ui/WPunkGateway.sol*
Links: [71](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L71).
```solidity
71:	* @dev supplies (deposits) WPunk into the reserve, using native Punk. A corresponding amount of the overlying asset (xTokens)
```

### [NC-04] Order of Functions Not Compliant With Solidity Docs

The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) suggests the following function ordering:  constructor, receive function (if exists), fallback function (if exists), external, public, internal, private.

#### Findings:

The following contracts are not compliant (examples are only to prove the functions are out of order NOT a full description): 

[NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol): public functions are positioned after external functions.
[ParaSpaceOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol): public functions are positioned after internal functions.
[UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol): external functions are positioned after public functions.
[PoolAddressesProvider.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol): external functions are positioned after public functions.
[GenericLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol): internal functions are positioned after private functions.
[MarketplaceLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol): external functions are positioned after internal functions.
[ValidationLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol): internal functions are positioned after private functions.
[PoolApeStaking.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol): external functions are positioned after internal functions.
[PoolCore.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol): constructor is positioned after internal functions.
[PoolMarketplace.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol): external functions are positioned after internal fucntions.
[PoolParameters.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol): constructor is positioned after internal functions.
[MintableIncentivizedERC721.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol): external functions are positioned after public functions.
[NToken.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol): constructor is positioned after internal functions.
[NTokenApeStaking.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol): external functions are positioned after public functions.
[NTokenBAYC.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol): external functions are positioned after internal functions.
[NTokenMAYC.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol): external functions are positioned after internal functions.
[NTokenUniswapV3.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol): recieve function is positioned after external functions.
[MintableERC721Logic.sol](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol): external functions are positioned after public functions.

### [NC-05] File Layout Out of Order

The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout) suggests the following contract element ordering:  Type declarations, State variables, Events, Modifiers, Functions.

#### Findings:

The following contracts are not compliant (examples are only to prove the functions are out of order NOT a full description): 

[X2Y2Adapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol): struct is located after the constructor.
[NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol): modifiers are located after functions, structs are after state variables.
[UniswapV3OracleWrapper.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol): structs are located after the constructor.
[LiquidationLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol): structs are located after events.
[MarketplaceLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol): structs are located after events.
[ValidationLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol): struct located after function declarations.
[MintableIncentivizedERC721.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol): modifiers are located before state variables.
[ApeStakingLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol): structs are located after state variables.

### [NC-06] `TODO` Left In Production Code

`TODO`s should not be in production code. Each `TODO` should either be discarded or implemented if needed prior to production.

#### Findings:

*/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol*
Links: [59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59).

```solidity
59:	makerAsk.price, // TODO: take minPercentageToAsk into account
```

*paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol*
Links: [238](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238).
```solidity
238:	// TODO using bit shifting for the 2^96
```

*paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol*
Links: [442](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442).
```solidity
442:	 // TODO: support PToken
```

### [NC-07] Underscore Notation Not Used / Not Used Consistently

Consider using underscore notation to help with contract readability (Ex. `23453` -> `23_453`).

#### Findings:

*/paraspace-core/contracts/misc/NFTFloorOracle.sol*
Links: [12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12).
```solidity
12:	uint128 constant EXPIRATION_PERIOD = 1800;
```

### [NC-08] Out of Place And Inconsistant *params* Comment

Param indicators should be in a [NatSpec](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html) comment above a function. The `getBidOrderInfo` has an out of place *params* comment inconsistant with all other functions in `/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol`.

#### Findings:

*/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol*
Links: [68](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L68)
```solidity
68:	bytes memory /*params*/
```

### [NC-09] Empty Comment

Empty comments can be removed and replaced with a newline for more readable style.

#### Findings:

*/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol*
Links: [202](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L202).
```solidity
202:	//
```

### [NC-10] Dead Comment Through Use of `////`

There is no need to comment a quadruple `/`. `////` can be changed to `//` to improve style or `///` if a NatSpec comment.

#### Findings:

*/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol*
Links: [171](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L171).
```solidity
171:	////transfer BAKC back for user
```

### [NC-11] Comment Used Over NatSpec

Comment headers for functions should be in [NatSpec](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html) format.

#### Findings:

[L792](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L792) can be made into a `@dev` tag [NatSpec](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html) comment.

*/paraspace-core/contracts/protocol/pool/PoolCore.sol*
Links: [792](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L792).
```solidity
792:	// This function is necessary when receive erc721 from looksrare
```