## [NAZ-L1] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`NFTFloorOracle.sol#L175`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L175), [`NFTFloorOracle.sol#L183`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L183), [`PoolAddressesProvider.sol#L70`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70), [`PoolAddressesProvider.sol#L329`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L329), [`PoolAddressesProvider.sol#L142`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142), [`PoolAddressesProvider.sol#L158`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158), [`PoolAddressesProvider.sol#L170`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170), [`PoolAddressesProvider.sol#L182`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182), [`PoolAddressesProvider.sol#L224`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224), [`PoolAddressesProvider.sol#L235`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235), [`PoolAddressesProvider.sol#L242`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242), [`PoolParameters.sol#L151`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151), [`PoolParameters.sol#L166`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166), [`PoolParameters.sol#L181`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181), [`PoolParameters.sol#L206`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206), [`MintableIncentivizedERC721.sol#L131`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131), [`MintableIncentivizedERC721.sol#L142`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L2] Missing Time locks
**Severity**: Low 
**Context**: [`NFTFloorOracle.sol#L139`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L139), [`NFTFloorOracle.sol#L148`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L148), [`NFTFloorOracle.sol#L158`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L158), [`NFTFloorOracle.sol#L167`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167), [`NFTFloorOracle.sol#L175`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L175), [`NFTFloorOracle.sol#L183`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L183), [`NFTFloorOracle.sol#L195`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L195), [`PoolAddressesProvider.sol#L56`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56), [`PoolAddressesProvider.sol#L70`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70), [`PoolAddressesProvider.sol#L81`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L81), [`PoolAddressesProvider.sol#L105`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L105), [`PoolAddressesProvider.sol#L121`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L121), [`PoolAddressesProvider.sol#L142`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142), [`PoolAddressesProvider.sol#L158`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158), [`PoolAddressesProvider.sol#L170`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170), [`PoolAddressesProvider.sol#L182`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182), [`PoolAddressesProvider.sol#L224`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224), [`PoolAddressesProvider.sol#L235`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235), [`PoolAddressesProvider.sol#L242`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242), [`PoolParameters.sol#L139`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139), [`PoolParameters.sol#L151`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151), [`PoolParameters.sol#L166`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166), [`PoolParameters.sol#L181`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181), [`PoolParameters.sol#L206`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206), [`MintableIncentivizedERC721.sol#L131`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131), [`MintableIncentivizedERC721.sol#L142`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142)

**Description**:
When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert: (1) users and give them a chance to engage/exit protocol if they are not agreeable to the changes (2) team in case of compromised owner(s) and give them a chance to perform incident response.

**Recommendation**:
Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorized owners have no time for any planned incident response.


## [NAZ-L3] Value Range Validity for Setters
**Severity** Low
**Context**: [`NFTFloorOracle.sol#L343`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L343), [`MintableIncentivizedERC721.sol#L142`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142)

**Description**:
These functions doesn't have any checks to ensure that the variables being set is within some kind of value range.

**Recommendation**:
Each variable input parameter updated should have it's own value range checks to ensure their validity.


## [NAZ-L4] Missing Zero-address Validation
**Severity**: Low
**Context**: [`NFTFloorOracle.sol#L278`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L278), [`NFTFloorOracle.sol#L307`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L307), [`PoolAddressesProvider.sol#L70`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70), [`PoolAddressesProvider.sol#L142`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142), [`PoolAddressesProvider.sol#L158`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158), [`PoolAddressesProvider.sol#L170`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170), [`PoolAddressesProvider.sol#L182`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182), [`PoolAddressesProvider.sol#L224`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224), [`PoolAddressesProvider.sol#L235`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235), [`PoolAddressesProvider.sol#L242`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242), [`MintableIncentivizedERC721.sol#L131`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L5] `receive()` Function Should Emit An Event
**Severity**: Low
**Context**: [`PoolCore.sol#L802`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L802), [`NTokenUniswapV3.sol#L149`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149)

**Description**:
Consider emitting an event inside this function with `msg.sender` and `msg.value` as the parameters. This would make it easier to track incoming ether transfers.

**Recommendation**:
Consider adding events to the `receive()` functions. 


## [NAZ-L6] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`PoolCore.sol#L75`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L75), [`WPunkGateway.sol#L57`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L57)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N1] Commented Out Code
**Severity**: Informational
**Context**: [`LiquidationLogic.sol#L306`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L306)

**Description**:
There is commented code that makes the code messy and unneeded. 

**Recommendation**:
Consider removing the commented out code.


## [NAZ-N2] Code Contains Empty Blocks
**Severity**: Informational
**Context**: [`LooksRareAdapter.sol#L23`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L23), [`SeaportAdapter.sol#L23`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L23), [`X2Y2Adapter.sol#L24`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L24), [`NToken.sol#L50`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L50), [`NTokenBAYC.sol#L18`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L18), [`NTokenMAYC.sol#L18`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L18), [`NTokenUniswapV3.sol#L149`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149)

**Description**:
It's best practice that when there is an empty block, to add a comment in the block explaining why it's empty.

**Recommendation**:
Consider adding `/* Comment on why */` to the empty blocks.


## [NAZ-N3] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`SeaportAdapter.sol#L124`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L124), [`NFTFloorOracle.sol#L81`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L81), [`ParaSpaceOracle.sol#L22`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L22), [`ParaSpaceOracle.sol#L25`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L25), [`ParaSpaceOracle.sol#L28-L29`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L28-L29), [`UniswapV3OracleWrapper.sol#L18-L21`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L18-L21), [`PoolAddressesProvider.sol#L31-L38`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L31-L38), [`LiquidationLogic.sol#L50`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L50), [`ValidationLogic.sol#L189`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L189), [`PoolApeStaking.sol#L34-L35`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L34-L35), [`PoolApeStaking.sol#L55`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L55), [`PoolApeStaking.sol#L413`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413), [`PoolApeStaking.sol#L428`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L428), [`PoolApeStaking.sol#L443`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L443), [`PoolCore.sol#L54`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L54), [`PoolCore.sol#L56`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L56), [`PoolCore.sol#L657`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L657), [`PoolCore.sol#L668`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L668), [`PoolMarketplace.sol#L55-L56`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L55-L56), [`PoolMarketplace.sol#L66`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L66), [`PoolParameters.sol#L48-L51`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L48-L51), [`PoolParameters.sol#L93`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L93), [`PoolStorage.sol#L19`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L19), [`MintableIncentivizedERC721.sol#L72-L73`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L72-L73), [`NToken.sol#L35`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L35), [`NToken.sol#L189`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L189), [`NTokenApeStaking.sol#L125`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L125), [`NTokenApeStaking.sol#L130`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L130), [`NTokenApeStaking.sol#L143`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L143), [`NTokenBAYC.sol#L109`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L109), [`NTokenMAYC.sol#L109`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L109), [`WPunkGateway.sol#L28-L30`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L28-L30)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N4] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`LooksRareAdapter.sol#L73`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L73), [`SeaportAdapter.sol#L93`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L93), [`X2Y2Adapter.sol#L26`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L26), [`NFTFloorOracle.sol#L70`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L70), [`ParaSpaceOracle.sol#L115`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L115), [`UniswapV3OracleWrapper.sol#L33-L40`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L33-L40), [`PoolAddressesProvider.sol#L56`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56), [`AuctionLogic.sol#L27`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L27), [`BorrowLogic.sol#L127`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L127), [`GenericLogic.sol#L399`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L399), [`LiquidationLogic.sol#L99-L148`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L99-L148), [`MarketplaceLogic.sol#L50`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L50), [`SupplyLogic.sol#L167`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L167), [`ValidationLogic.sol#L189`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L189), [`ValidationLogic.sol#L225`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L225), [`ValidationLogic.sol#L478-L492`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L478-L492), [`ValidationLogic.sol#L1146`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1146), [`PoolApeStaking.sol#L60`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L60), [`PoolApeStaking.sol#L224`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L224), [`PoolCore.sol#L64`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L64), [`PoolMarketplace.sol#L71`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L71), [`PoolParameters.sol#L89`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L89), [`MintableIncentivizedERC721.sol#L71-L75`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L71-L75), [`NToken.sol#L43`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L43), [`NTokenApeStaking.sol#L71`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L71), [`NTokenBAYC.sol#L113`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L113), [`NTokenMAYC.sol#L113`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L113), [`NTokenMoonBirds.sol#L40`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40), [`NTokenUniswapV3.sol#L123`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123), [`MintableERC721Logic.sol#L135`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L135), [`ApeStakingLogic.sol#L77`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L77)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N5] Use Underscores for Number Literals
**Severity**: Informational
**Context**: [`NFTFloorOracle.sol#L12`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12)

**Description**:
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

**Recommendation**:
Consider using underscores for number literals to improve its readability.


## [NAZ-N6] TODOs Left In The Code*
**Severity**: Informational
**Context**: [`LooksRareAdapter.sol#L59`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59), [`UniswapV3OracleWrapper.sol#L238`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238), [`MarketplaceLogic.sol#L442`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442), 

**Description**:
There should never be any TODOs in the code when deploying.

**Recommendation**:
Consider finishing the TODOs before deploying.


## [NAZ-N7] Spelling Errors
**Severity**: Informational
**Context**: [`AuctionLogic.sol#L34 (tsatr => start)`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34), [`ApeStakingLogic.sol#L59 (undate => update)`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L59)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.


## [NAZ-N8] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N9] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.