
# QA
# Low
## Single-step process for critical ownership transfer/renounce is risky
### Impact
Given that `WPunkGateway.sol` and `PoolAddressesProvider.sol` are derived from `Ownable` and `OwnableUpgradeable`, the ownership management of these contracts defaults to `Ownable`’s `transferOwnership()` and `renounceOwnership()` methods which are not overridden here. 

Such critical address transfer/renouncing in one-step is very risky because it is irrecoverable from any mistakes

Scenario: If an incorrect address, e.g. for which the private key is not known, is used accidentally then it prevents the use of all the `onlyOwner()` functions forever, which includes the changing of various critical addresses and parameters. This use of incorrect address may not even be immediately apparent given that these functions are probably not used immediately. 

When noticed, due to a failing `onlyOwner()` function call, it will force the redeployment of these contracts and require appropriate changes and notifications for switching from the old to new address. This will diminish trust in the protocol and incur a significant reputational damage.

### Github Permalinks
- `Ownable`
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L20
contract PoolAddressesProvider is Ownable, IPoolAddressesProvider {

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L23
    OwnableUpgradeable




### Proof of Concept
See similar High Risk severity finding from Trail-of-Bits Audit of Hermez.
https://github.com/trailofbits/publications/blob/master/reviews/hermez.pdf
See similar Medium Risk severity finding from Trail-of-Bits Audit of Uniswap V3:
https://github.com/Uniswap/v3-core/blob/main/audits/tob/audit.pdf

### Recommended steps
Recommend overriding the inherited methods to null functions and use separate functions for a two-step address change:
1. Approve a new address as a `pendingOwner`
2. A transaction from the `pendingOwner` address claims the pending ownership change.

This mitigates risk because if an incorrect address is used in step (1) then it can be fixed by re-approving the correct address. Only after a correct address is used in step (1) can step (2) happen and complete the address/ownership change.

Also, consider adding a time-delay for such sensitive actions. And at a minimum, use a multisig owner address and not an EOA.


## Upgradeable contract is missing a __gap[50] storage variable to allow for new storage variables in later versions
### Impact 
For upgradeable contracts, there must be storage gap to "allow developers to freely add new state variables in the future without compromising the storage compatibility with existing deployments" (quote OpenZeppelin). Otherwise it may be very difficult to write new implementation code. Without storage gap, the variable in child contract might be overwritten by the upgraded base contract if new variables are added to the base contract. This could have unintended and very serious consequences to the child contracts, potentially causing loss of user fund or cause the contract to malfunction completely.

See:
https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps
For a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

### Proof of Concept
In the following context of the upgradeable contracts they are expected to use gaps for avoiding collision:

- `OwnableUpgradeable` in `WPunkGateway.sol`

However, none of these contracts contain storage gap. The storage gap is essential for upgradeable contract because "It allows us to freely add new state variables in the future without compromising the storage compatibility with existing deployments". Refer to the bottom part of this article:

https://docs.openzeppelin.com/contracts/4.x/upgradeable

If a contract inheriting from a base contract contains additional variable, then the base contract cannot be upgraded to include any additional variable, because it would overwrite the variable declared in its child contract. This greatly limits contract upgradeability.

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L4
import {OwnableUpgradeable} from "../dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol";

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L23
    OwnableUpgradeable


### Tools Used
Manual analysis

### Recommended Mitigation Steps
Recommend adding appropriate storage gap at the end of upgradeable contracts such as the below. 
Please reference OpenZeppelin upgradeable contract templates.

`uint256[50] private __gap;`



## Prevent div by 0
### Impact
On several locations in the code precautions are being taken to not divide by `0`, this should be done as a division by `0` would revert the code.

### Proof of Concept
Navigate to the following contracts,
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L349
                return userTotalDebt / assetUnit;

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L531
            return (balance / assetUnit);

### Recommended Mitigation Steps
Recommend making sure division by `0` won’t occur by checking the variables beforehand and handling this edge case.


## Missing checks for address(0x0) when assigning values to `address` state or `immutable` variables 
### Summary
Zero address should be checked for state variables, immutable variables. A zero address can lead into problems.
### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L28-L30
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L51-L53
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L64-L66
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L61-L64
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L89-L91
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L32-L34
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L16-L18
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L16-L18
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L92
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L50
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L53

### Mitigation
Check zero address before assigning or using it





## Missing safety checks on setters
### Summary
Zero address should be checked for some function parameters as setters for roles, mints, withdrawals... 

A zero address can lead into serious problems as locking eth or incorrect functioning.

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L109-L112
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70-L78
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224-L256

### Mitigation
Check zero address before assigning or using it




## `block.timestamp` used as time proxy 
### Summary
Risk of using `block.timestamp` for time should be considered. 
### Details
`block.timestamp` is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
- Timestamp used for calculations
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L802
                    block.timestamp

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L778
                .calculateAuctionPriceMultiplier(startTime, block.timestamp);

- Timestamp used as proxy of time
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L380
        assetPriceMapEntry.updatedTimestamp = block.timestamp;

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L285
        userConfig.auctionValidityTime = block.timestamp;

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L69
                        deadline: block.timestamp

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L382
            startTime: block.timestamp

### Mitigation
- Consider the risk of using `block.timestamp` as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision


## Front run initializer
### Summary
The initialize function that initializes important contract state can be called by anyone.
### Details
The attacker can initialize the contract before the legitimate deployer, hoping that the victim continues to use the same contract.

In the best case for the victim, they notice it and have to redeploy their contract costing gas.

### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97
    function initialize(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NToken.sol#L52
    function initialize(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L36
    function initialize(

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L57
    function initialize() external initializer {

### Mitigation
Use the constructor to initialize non-proxied contracts.

For initializing proxy contracts deploy contracts using a factory contract that immediately calls initialize after deployment or make sure to call it immediately after deployment and verify the transaction succeeded.




## Variable shadows another variable
###  Summary 
Name shadowing where two or more variables/functions share the same name could be confusing to developers and/or reviewers
###  Details 
Use of `isUsedAsCollateral` as local variable  shadows `MintableIncentivizedERC721.isUsedAsCollateral`

###  Github Permalinks 
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/tokenization/NToken.sol#L242-L250

###  Mitigation
Consider replacing `isUsedAsCollateral` variable in the function to `_isUsedAsCollateral` or a similar substitution


## `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccack256()`
### NOTE
This finding wasn't found at C4udit / Publicly Known Issues (L-1):
https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba

### Summary
If you are dealing with more than one dynamic data type, `abi.encodePacked()` can lead to collisions when used with a hash function.

Use `abi.encode()` instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). “Unless there is a compelling reason, `abi.encode` should be preferred”. If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` instead.

### Details
abi.encodePacked will only use the only use the minimal required memory to encode the data. E.g. an address will only use 20 bytes and for dynamic arrays only the elements will be stored without length. For more info see the Solidity docs for packed mode.

For the input of the keccak method it is important that you can ensure that the resulting bytes of the encoding are unique. So if you always encode the same types and arrays always have the same length then there is no problem. But if you switch the parameters that you encode or encode multiple dynamic arrays you might have conflicts.

https://ethereum.stackexchange.com/questions/119583/when-to-use-abi-encode-abi-encodepacked-or-abi-encodewithsignature-in-solidity

### Github Permalinks

https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1115-L1119
        bytes32 typeHash = keccak256(
            abi.encodePacked(
                "Credit(address token,uint256 amount,bytes orderId)"
            )
        );
### Mitigation
Change `abi.encodePacked` to `abi.encode` when data collision may happen





# Informational
## Remove tautological code
### Summary
feederIndex is a uint8 and therefore will always be >= 0

### Github Permalink

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L331
        if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {

### Mitigation
Remove tautological code



## Constants should be defined rather than using magic numbers
### NOTE
These weren't found at C4udit / Publicly Known Issues (NC-4):
https://gist.github.com/Picodes/d39514dc40b7dee4fe0ff32768569cba

### Summary
Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.

### Details
Values are hardcoded and would be more readable and maintainable if declared as a constant

### Github Permalinks
- numbers
`100` 
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L367-L368

`96` 
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L247
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L259

`1E9`
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L247
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L259
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L271

`30`
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L34

- not numbers
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1137-L1139


### Mitigation
Replace magic hardcoded numbers with declared constants.






## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots

### Github Permalink
- constructor struct
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L23-L29
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23-L46
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L223-L230

- struct in the middle of functions
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1149
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L478-L492
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L224-L245
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L77-L84

- function before constructor
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/protocol/tokenization/NToken.sol#L34-L37

- functions before modifier
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L96-L135

- modifier in the middle of variable declaration
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L45-L62

### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.15/style-guide.html




## Missing Natspec 
### Summary 
Missing Natspec and regular comments affect readability and maintainability of a codebase. 

### Details 
Contracts has partial or full lack of comments

### Github Permalinks 
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L96-L112
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L163-L288
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L571-L623

 ### Mitigation
 - Add `@param` descriptors
 - Add `@return` descriptors



## Extra amount of / in folder path
### Summary
There is one extra / in contracts//IERC20
### Github Permalink
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L4
import {IERC20} from "../../../dependencies/openzeppelin/contracts//IERC20.sol";
### Recommendation
Remove unnecesary character

## Open TODOs   
### Summary
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
### Details
The code includes a `TODO` that affects readability and focus on the readers/auditors of the contracts
### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59
            makerAsk.price, // TODO: take minPercentageToAsk into account

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238
        // TODO using bit shifting for the 2^96

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442
        // TODO: support PToken


### Mitigation
Solve and remove the `TODO` 


## Inconsistent spacing in comments
Some lines use `// x` and some use `//x`. The instances below point out the usages that don't follow the majority, within each file
### Details

`uint256 public genericDeclaration; //generic comment without space`
But following the style of the other comments would be:
`uint256 public genericDeclaration; // generic comment with space`]]
### Github Permalinks


https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L8
//we need to deploy 3 oracles at least

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L10
//expirationPeriod at least the interval of client to feed data(currently 6h=21600s/12=1800 in mainnet)

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L11
//we do not accept price lags behind to much

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L13
//reject when price increase/decrease 1.5 times more than original value

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L361
        //first price is always valid

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L404
        //first time just use the feeding value

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L408
        //use memory here so allocate with maximum length

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412
        //aggeregate with price from all feeders

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L100
        //userCollateral from collateralReserve

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L102
        //userGlobalCollateral from all reserves

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L104
        //userDebt from liquadationReserve

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L106
        //userGlobalDebt from all reserves

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L108
        //actualLiquidationAmount to repay based on collateral

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L110
        //actualCollateral allowed to liquidate

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L112
        //liquidationBonusRate from reserve config

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L114
        //user health factor

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L116
        //liquidation protocol fee to be sent to treasury

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L118
        //liquidation funds payer

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L120
        //collateral P|N Token

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L122
        //auction strategy

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L124
        //liquidation asset reserve id

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L126
        //whether auction is enabled

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L128
        //liquidation reserve cache

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L314
            vars.userGlobalDebt, //in base currency

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L140
        //currently don't need to update state for erc721

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L141
        //reserve.updateState(reserveCache);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L344
        //currently don't need to update state for erc721

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L345
        //reserve.updateState(reserveCache);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L405
        //currently don't need to update state for erc721

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L406
        //reserve.updateState(reserveCache);

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L353
        //add the current already borrowed amount to the amount requested to calculate the total collateral needed.

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L355
            vars.amountInBaseCurrency).percentDiv(vars.currentLtv); //LTV is calculated in percentage

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L573
        //if collateral isn't enabled as collateral by user, it cannot be liquidated

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L685
        //if collateral isn't enabled as collateral by user, it cannot be liquidated

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L794
        //if collateral isn't enabled as collateral by user, it cannot be auctioned

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L155
            //only partially withdraw need user's BAKC

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L214
        //transfer BAKC back for user

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L304
        //transfer BAKC back for user

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L337
        //7 checkout ape balance

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L101
        //1 unstake all position

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L103
            //1.1 unstake Main pool position

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L119
            //1.2 unstake bakc pool position

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L161
        //2 send incentive to caller

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L176
        //3 repay and supply

### Mitigation
Use only `// x`


## Maximum line length exceeded
### Summary
Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details 
Lines that exceed the 120 character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.16/style-guide.html#maximum-line-length
### Github Permalinks 
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L11

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L11

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L12

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L7

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L292

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L338

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L94

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L44

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L157

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L276

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L655

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L21

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L106

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L260

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1137

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolCore.sol#L72

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L100

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L48

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L93

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L95

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L48

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L93

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L95

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L530

https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/ui/WPunkGateway.sol#L71


### Mitigation
Reduce line length to less than 120 at least to improve maintainability and readability of the code 


## Naming convention of state variable non constant
### Summary
Only constants are suggested to use style `CONSTANTS_WITH_UNDERSCORES`, other variables are suggested to use `camelCase`
### Github Permalinks
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L28-L29
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L22
https://github.com/code-423n4/2022-11-paraspace/blob/154c657d927c66fc8a45a9a6df881ec937323be8/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L18-L20
https://github.com/code-423n4/2022-11-paraspace/blob/c01a980e5d6e15b2993b912c3569ed8b5236ff33/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L55

### Mitigation
Rename to `camelCase`







