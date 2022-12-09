### QA 

### The order of the layout does not follow best practices.

- The best-practices for layout within a contract is the following order: state variables, events, modifiers, constructor and functions.

```solidity
file: /contracts/protocol/pool/PoolConfigurator.sol

// state variable coming after modifiers
69:    uint256 public constant CONFIGURATOR_REVISION = 0x1;
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol

```solidity
file: /contracts/protocol/tokenization/base/IncentivizedERC20.sol

// modifiers coming before state variables
27:     modifier onlyPoolAdmin() {
41:     modifier onlyPool() {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L27

```solidity
file: /contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

// modifiers coming before state variables
45:    modifier onlyPoolAdmin() {  
59:    modifier onlyPool() {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L45

```solidity
file: /contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

// struct coming after functions
77:    struct UnstakeAndRepayParams {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L77

```solidity
file: /contracts/protocol/tokenization/NToken.sol  

// function coming before constructor
35:    function getRevision() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L35

```solidity
file: /contracts/protocol/tokenization/PToken.sol

//function coming before constructor
45:    function getRevision() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PToken.sol


```solidity
file: /contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol

116:    struct CalcInterestRatesLocalVars {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol


```solidity
file: /contracts/protocol/pool/PoolApeStaking.sol

// struct coming after variables, should come before
224:     struct BorrowAndStakeLocalVar {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol


---
&nbsp;
### Functions are intercalating from public, external and internal when they should be ordered. Also some view functions are declared before non-view functions which is not a good pattern.

- Order of Functions: Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. Functions should be grouped according to their visibility and ordered: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Within a grouping, place the view and pure functions last.

```solidity
file: /contracts/protocol/pool/PoolConfigurator.sol

Some of the external, public and internal functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol

```solidity
file: /contracts/protocol/tokenization/base/IncentivizedERC20.sol

All the external and public are mixed. Internal functions are in the correct order. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol

```solidity
file: /contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

All the external, public and internal functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

```solidity
file: /contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol

External and public functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol

```solidity
file: /contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

External, public and internal functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

```solidity
External, public and internal functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol

```solidity
file: /contracts/protocol/tokenization/NTokenApeStaking.sol

External, public and internal functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```solidity
file: /contracts/protocol/tokenization/NTokenBAYC.sol

// this function should be the last one since it's the only internal function, the other ones are in the right order
109:    function POOL_ID() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L109

```solidity
file: contracts/protocol/tokenization/NTokenMAYC.sol

// this function should be the last one since it's the only internal function, the other ones are in the right order
109:    function POOL_ID() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

```solidity
file: /contracts/protocol/tokenization/NTokenMoonBirds.sol

// view and pure functions should come after non-view and non-pure.
36:    function getXTokenType() external pure override returns (XTokenType) {

// view and pure functions should come after non-view and non-pure.
111:    function nestingPeriod(uint256 tokenId)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

```solidity
file: /contracts/protocol/tokenization/NTokenUniswapV3.sol


// view and pure functions should go last inside their category
37:    function getXTokenType() external pure override returns (XTokenType) {

// should come right after the contructor
149:    receive() external payable {}

The rest of the functions are all mixed up between internal and external.

```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

```solidity
file: /contracts/protocol/tokenization/PToken.sol
 All the external, public and internal functions are mixed. Adding each line here would be massive and unnecessary.
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PToken.sol


```solidity
file: /contracts/protocol/tokenization/PTokenAToken.sol

// this internal function is coming before an external one
23:    function lastRebasingIndex() internal view override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenAToken.sol

```solidity
file: /contracts/protocol/tokenization/PTokenSApe.sol

All the external, public and internal functions are mixed. Adding each line here would be massive and unnecessary.

View and pure functions should come last in their groupings
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol

```solidity
file: /contracts/protocol/tokenization/PTokenStETH.sol

// this internal function should come last
23:    function lastRebasingIndex() internal view override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol

```solidity
file: /contracts/protocol/tokenization/RebasingDebtToken.sol

// the only function out of order is this external one, put it as the first function.
59:    function getScaledUserBalanceAndSupply(address user)

```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/RebasingDebtToken.sol


```solidity
file: /contracts/protocol/tokenization/VariableDebtToken.sol

All the external, public and internal functions are mixed. Adding each line here would be massive and unnecessary.

View and pure functions should come last in their groupings
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol


```solidity
file: /contracts/protocol/pool/PoolApeStaking.sol

// this internal functions is coming before external ones
55:    function getRevision() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

---
&nbsp;
### Netspec missing / misspelled

- Everything should be documented to devs and users for a better understanding

```solidity
file: /contracts/protocol/pool/PoolApeStaking.sol

// ADDRESSES_PROVIDER should be renamed to _addresses_provider since it's immutable not a constant
34:    IPoolAddressesProvider internal immutable ADDRESSES_PROVIDER;

//netspec missing
55:    function getRevision() internal pure virtual override returns (uint256) {

//netspec missing
224:    struct BorrowAndStakeLocalVar {

413:    function setSApeUseAsCollateral(address user) internal {

428:    function getUserHf(address user) internal view returns (uint256) {

443:    function checkSApeIsNotPaused(DataTypes.PoolStorage storage ps)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```solidity
file: /contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

96:     function name() public view override returns (string memory) {

100:    function symbol() external view override returns (string memory) {

104:    function balanceOf(address account)

290:    function _safeTransferFrom(

364:    function _mintMultiple(

384:    function _burnMultiple(address user, uint256[] calldata tokenIds)
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

```solidity
file: /contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

80:    function executeTransfer(

135:    function executeTransferCollateralizable(

153:    function executeSetIsUsedAsCollateral(

187:    function executeMintMultiple(

260:    function executeBurnMultiple(

340:    function executeApprove(

348:    function _approve(

357:    function executeApprovalForAll(

368:    function executeStartAuction(

386:    function executeEndAuction(

402:    function _checkBalanceLimit(

416:    function _exists(MintableERC721Data storage erc721Data, uint256 tokenId)

424:    function isAuctioned(
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

```solidity
file: /contracts/protocol/tokenization/NToken.sol

52:    function initialize(

95:    function _burn(

127:    function rescueERC20(

136:    function rescueERC721(

151:    function rescueERC1155(

168:    function executeAirdrop(

271:    function onERC721Received(

280:    function onERC1155Received(

295:    function onERC1155BatchReceived(

323:    function getAtomicPricingConfig() external view returns (bool) {

327:    function getXTokenType()
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol

```solidity
file: /contracts/protocol/tokenization/NTokenApeStaking.sol

36:    function initialize(

125:    function POOL_ID() internal pure virtual returns (uint256) {

130:    function initializeStakingData() internal {

136:    function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {

143:    function apeStakingDataStorage()
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```solidity
file: /contracts/protocol/tokenization/NTokenBAYC.sol

109:    function POOL_ID() internal pure virtual override returns (uint256) {

113:    function getXTokenType() external pure override returns (XTokenType) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol


```solidity
file: /contracts/protocol/tokenization/NTokenMAYC.sol

109:    function POOL_ID() internal pure virtual override returns (uint256) {

113:    function getXTokenType() external pure override returns (XTokenType) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol


```solidity
file: /contracts/protocol/tokenization/NTokenMoonBirds.sol

36:    function getXTokenType() external pure override returns (XTokenType) {

40:    function burn(

63:    function onERC721Received(
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol


```solidity
file: /contracts/protocol/tokenization/NTokenUniswapV3.sol

37:    function getXTokenType() external pure override returns (XTokenType) {

144:    function _safeTransferETH(address to, uint256 value) internal {

149:    receive() external payable {}
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol


```solidity
file: /contracts/protocol/tokenization/PToken.sol

339:    function getXTokenType()
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PToken.sol

```solidity
file: /contracts/protocol/tokenization/PTokenAToken.sol

31:    function getXTokenType() external pure override returns (XTokenType) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenAToken.sol

```solidity
file: /contracts/protocol/tokenization/PTokenSApe.sol

27:    constructor(IPool pool) PToken(pool) {

31:    function setNToken(address _nBAYC, address _nMAYC) external onlyPoolAdmin {

36:    function mint(

45:    function burn(

54:    function balanceOf(address user) public view override returns (uint256) {

61:    function scaledBalanceOf(address user)

70:    function transferUnderlyingTo(address, uint256)

79:    function transferOnLiquidation(

87:    function _transfer(

95:    function getXTokenType()
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenSApe.sol

```solidity
file: /contracts/protocol/tokenization/PTokenStETH.sol

16:    constructor(IPool pool) RebasingPToken(pool) {

30:    function getXTokenType() external pure override returns (XTokenType) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol

```solidity
file: /contracts/protocol/tokenization/RebasingDebtToken.sol

16:    constructor(IPool pool) VariableDebtToken(pool) {

105:    function _scaledBalanceOf(address user, uint256 rebasingIndex)

119:    function _scaledTotalSupply(uint256 rebasingIndex)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/RebasingDebtToken.sol


```solidity
file: /contracts/protocol/tokenization/StETHDebtToken.sol

15:    constructor(IPool pool) RebasingDebtToken(pool) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol


```solidity
file: /contracts/protocol/tokenization/VariableDebtToken.sol

158:    function allowance(address, address)

168:    function approve(address, uint256)

177:    function transferFrom(

185:    function increaseAllowance(address, uint256)

194:    function decreaseAllowance(address, uint256)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol


```solidity
file: /contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

50:    constructor(

66:    function getMaxPriceMultiplier() external view returns (uint256) {

70:    function getMinExpPriceMultiplier() external view returns (uint256) {

74:    function getMinPriceMultiplier() external view returns (uint256) {

78:    function getStepLinear() external view returns (uint256) {

82:    function getStepExp() external view returns (uint256) {

86:    function getTickLength() external view returns (uint256) {

90:    function calculateAuctionPriceMultiplier(

101:    function _calculateAuctionPriceMultiplierByTicks(uint256 ticks)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol


```solidity
file: /contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol
 
// comment says `constant` but was defined as `immutable`
// change OPTIMAL_USAGE_RATIO to _optimal_usage_ratio since it's immutable not a constant
     /**
     * @dev This constant represents the usage ratio at which the pool aims to obtain most competitive borrow rates.
     * Expressed in ray
     **/
30:    uint256 public immutable OPTIMAL_USAGE_RATIO;

// change MAX_EXCESS_USAGE_RATIO to _max_excess_usage_ration since it's immutable not a constant
37:    uint256 public immutable MAX_EXCESS_USAGE_RATIO;

// change ADDRESSES_PROVIDER to _addresses_provider since it's immutable not a constant
39:    IPoolAddressesProvider public immutable ADDRESSES_PROVIDER;

// netspec missing
116:    struct CalcInterestRatesLocalVars {

```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/DefaultReserveInterestRateStrategy.sol

---
&nbsp;

### Events not indexed

- indexed events are better for organization

```solidity
file: /contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

29:    event UnstakeApeIncentiveUpdated(uint256 oldValue, uint256 newValue);

```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

---
&nbsp;

### Observations

```solidity
file: /contracts/protocol/pool/PoolApeStaking.sol

// internal function missing underline
55:    function getRevision() internal pure virtual override returns (uint256) {

// internal function missing underline
413:    function setSApeUseAsCollateral(address user) internal {

// internal function missing underline
428:    function getUserHf(address user) internal view returns (uint256) {

// internal function missing underline
443:    function checkSApeIsNotPaused(DataTypes.PoolStorage storage ps)
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```solidity
file: /contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

// should have 1 blank line before declaration
29:    event UnstakeApeIncentiveUpdated(uint256 oldValue, uint256 newValue); 

// function name should be changed to _getRevision() to match the other internal ones
35:    function getRevision() internal pure virtual override returns (uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L29

```solidity
file: /contracts/protocol/tokenization/PToken.sol

// function name should be changed to _getRevision() to match the other internal ones
45:    function getRevision() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PToken.sol

```solidity
file: /contracts/protocol/tokenization/PTokenAToken.sol

// function name should be changed to _lastRebasingIndex() to match the other internal ones
23:    function lastRebasingIndex() internal view override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenAToken.sol

```solidity
file: /contracts/protocol/tokenization/PTokenStETH.sol

// function name should be _lastRebasingIndex()
23:    function lastRebasingIndex() internal view override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/PTokenStETH.sol

```solidity
file: /contracts/protocol/tokenization/RebasingDebtToken.sol

// function name should be _lastRebasingIndex()
132:    function lastRebasingIndex() internal view virtual returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/RebasingDebtToken.sol

```solidity
file: /contracts/protocol/tokenization/StETHDebtToken.sol

// function name should be _lastRebasingIndex()
22:    function lastRebasingIndex() internal view override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/StETHDebtToken.sol

```solidity
file: /contracts/protocol/tokenization/VariableDebtToken.sol

// function name should be _getRevision()
82:    function getRevision() internal pure virtual override returns (uint256) {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/VariableDebtToken.sol
