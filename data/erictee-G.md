### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
contracts/protocol/libraries/logic/ValidationLogic.sol:L1124                abi.encode(

contracts/protocol/libraries/logic/ValidationLogic.sol:L1136                abi.encode(

```

### [G-02] Empty blocks should be removed or emit something


#### Impact
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.


#### Findings:
```
contracts/misc/marketplaces/SeaportAdapter.sol:L23    constructor() {}

contracts/misc/marketplaces/LooksRareAdapter.sol:L23    constructor() {}

contracts/misc/marketplaces/X2Y2Adapter.sol:L24    constructor() {}

contracts/protocol/tokenization/NTokenBAYC.sol:L18    {}

contracts/protocol/tokenization/NTokenUniswapV3.sol:L149    receive() external payable {}

contracts/protocol/tokenization/NToken.sol:L50    {}

contracts/protocol/tokenization/NTokenMAYC.sol:L18    {}

```

### [G-03] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/protocol/configuration/PoolAddressesProvider.sol:L158    function setACLManager(address newAclManager) external override onlyOwner {

contracts/protocol/configuration/PoolAddressesProvider.sol:L170    function setACLAdmin(address newAclAdmin) external override onlyOwner {

contracts/protocol/configuration/PoolAddressesProvider.sol:L235    function setWETH(address newWETH) external override onlyOwner {

contracts/protocol/tokenization/NTokenApeStaking.sol:L136    function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {

contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol:L142    function setBalanceLimit(uint64 limit) external onlyPoolAdmin {

```

### [G-04] ```X += Y``` costs more gas than ```X = X + Y``` for state variables.


#### Impact
Consider changing ```X += Y``` to ```X = X + Y``` to save gas.


#### Findings:
```
contracts/misc/UniswapV3OracleWrapper.sol:L149        token0Amount += positionData.tokensOwed0;

contracts/misc/UniswapV3OracleWrapper.sol:L150        token1Amount += positionData.tokensOwed1;

contracts/misc/UniswapV3OracleWrapper.sol:L211            sum += getTokenPrice(tokenIds[index]);
        
contracts/protocol/libraries/logic/GenericLogic.sol:L380                    totalValue += _getTokenPrice(

contracts/protocol/libraries/logic/GenericLogic.sol:L479                totalValue += tokenPrice;

contracts/protocol/libraries/logic/GenericLogic.sol:L496                totalLTV += tmpLTV * tokenPrice;

contracts/protocol/libraries/logic/MarketplaceLogic.sol:L397            price += item.startAmount;

contracts/protocol/pool/PoolApeStaking.sol:L77            amountToWithdraw += _nfts[index].amount;

contracts/protocol/pool/PoolApeStaking.sol:L166            amountToWithdraw += _nftPairs[index].amount;

contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:L215            totalAmount += getTokenIdStakingAmount(

contracts/protocol/tokenization/libraries/ApeStakingLogic.sol:L257            apeStakedAmount += bakcStakedAmount;

```

### [G-05] Revert message greater than 32 bytes


#### Impact
Keep revert message lower than or equal to 32 bytes to save gas.


#### Findings:
```
contracts/misc/NFTFloorOracle.sol:L225        require(

contracts/misc/NFTFloorOracle.sol:L356        require(_twap > 0, "NFTOracle: price should be more than 0");

contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:L88        require(

contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:L92        require(to != address(0), "ERC721: transfer to the zero address");

contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:L377        require(

contracts/protocol/tokenization/libraries/MintableERC721Logic.sol:L395        require(

```

### [G-06] ```++i```/```i++``` should be ```unchecked{++i}```/```unchecked{i++}``` when it is not possible for them to overflow, as is the case when used in for- and while-loops.


#### Impact
The ```unchecked``` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.


#### Findings:
```
contracts/misc/ParaSpaceOracle.sol:L95        for (uint256 i = 0; i < assets.length; i++) {

contracts/misc/ParaSpaceOracle.sol:L197        for (uint256 i = 0; i < assets.length; i++) {

contracts/misc/NFTFloorOracle.sol:L229        for (uint256 i = 0; i < _assets.length; i++) {

contracts/misc/NFTFloorOracle.sol:L291        for (uint256 i = 0; i < _assets.length; i++) {

contracts/misc/NFTFloorOracle.sol:L321        for (uint256 i = 0; i < _feeders.length; i++) {

contracts/misc/NFTFloorOracle.sol:L413        for (uint256 i = 0; i < feederSize; i++) {

contracts/ui/WPunkGateway.sol:L82        for (uint256 i = 0; i < punkIndexes.length; i++) {

contracts/ui/WPunkGateway.sol:L109        for (uint256 i = 0; i < punkIndexes.length; i++) {

contracts/ui/WPunkGateway.sol:L113        for (uint256 i = 0; i < punkIndexes.length; i++) {

contracts/ui/WPunkGateway.sol:L136        for (uint256 i = 0; i < punkIndexes.length; i++) {

contracts/ui/WPunkGateway.sol:L174        for (uint256 i = 0; i < punkIndexes.length; i++) {

contracts/protocol/libraries/logic/ValidationLogic.sol:L1034        for (uint256 i = 0; i < params.nftTokenIds.length; i++) {

contracts/protocol/libraries/logic/MarketplaceLogic.sol:L177        for (uint256 i = 0; i < marketplaceIds.length; i++) {

contracts/protocol/libraries/logic/MarketplaceLogic.sol:L284        for (uint256 i = 0; i < marketplaceIds.length; i++) {

contracts/protocol/libraries/logic/MarketplaceLogic.sol:L382        for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

contracts/protocol/libraries/logic/MarketplaceLogic.sol:L470        for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {

contracts/protocol/libraries/logic/PoolLogic.sol:L58        for (uint16 i = 0; i < params.reservesCount; i++) {

contracts/protocol/libraries/logic/PoolLogic.sol:L104        for (uint256 i = 0; i < assets.length; i++) {

contracts/protocol/pool/PoolCore.sol:L634        for (uint256 i = 0; i < reservesListCount; i++) {

contracts/protocol/tokenization/NToken.sol:L145        for (uint256 i = 0; i < ids.length; i++) {

```