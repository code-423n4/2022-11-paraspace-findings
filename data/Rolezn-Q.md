## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Missing Checks for Address(0x0)  | 7 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Unused `receive()` Function Will Lock Ether In Contract  | 1 |
| [LOW&#x2011;3](#LOW&#x2011;3) | Missing Contract-existence Checks Before Low-level Calls | 1 |
| [LOW&#x2011;4](#LOW&#x2011;4) | Contracts are not using their OZ Upgradeable counterparts | 87 |
| [LOW&#x2011;5](#LOW&#x2011;5) | Low Level Calls With Solidity Version 0.8.14 Can Result In Optimiser Bug | 1 |
| [LOW&#x2011;6](#LOW&#x2011;6) | Missing parameter validation in `constructor` | 12 |
| [LOW&#x2011;7](#LOW&#x2011;7) | Prevent division by 0 | 2 |
| [LOW&#x2011;8](#LOW&#x2011;8) | Upgrade OpenZeppelin Contract Dependency | 2 |
| [LOW&#x2011;9](#LOW&#x2011;9) | The `nonReentrant` modifier should occur before all other modifiers | 25 |
| [LOW&#x2011;10](#LOW&#x2011;10) | `getPrice` and `combine` will not work if `expirationPeriod` == 0 | 2 |


Total: 140 contexts over 10 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Critical Changes Should Use Two-step Procedure | 30 |
| [NC&#x2011;2](#NC&#x2011;2) | Use a more recent version of Solidity | 32 |
| [NC&#x2011;3](#NC&#x2011;3) | Event Is Missing Indexed Fields | 2 |
| [NC&#x2011;4](#NC&#x2011;4) | Constants Should Be Defined Rather Than Using Magic Numbers | 1 |
| [NC&#x2011;5](#NC&#x2011;5) | Missing event for critical parameter change | 8 |
| [NC&#x2011;6](#NC&#x2011;6) | Implementation contract may not be initialized | 16 |
| [NC&#x2011;7](#NC&#x2011;7) | Use of Block.Timestamp | 2 |
| [NC&#x2011;8](#NC&#x2011;8) | Non-usage of specific imports | 20 |
| [NC&#x2011;9](#NC&#x2011;9) | Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant` | 3 |
| [NC&#x2011;10](#NC&#x2011;10) | Use `bytes.concat()` | 12 |
| [NC&#x2011;11](#NC&#x2011;11) | Open TODOs | 3 |
| [NC&#x2011;12](#NC&#x2011;12) | No need to initialize uints to zero | 10 |
| [NC&#x2011;13](#NC&#x2011;13) | Use `delete` to Clear Variables | 4 |
| [NC&#x2011;14](#NC&#x2011;14) | Duplicate imports  | 4 |
| [NC&#x2011;15](#NC&#x2011;15) | Missing NATSPEC documentation | 4 |
| [NC&#x2011;16](#NC&#x2011;16) | Insufficient NATSPEC documentation | 4 |

Total: 144 contexts over 14 issues

## Low Risk Issues

### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Missing Checks for Address(0x0) 

Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

#### <ins>Proof Of Concept</ins>


```solidity
70: function setAddress: address newAddress
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70

```solidity
142: function setPriceOracle: address newPriceOracle
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142

```solidity
158: function setACLManager: address newAclManager
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158

```solidity
170: function setACLAdmin: address newAclAdmin
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170

```solidity
182: function setPriceOracleSentinel: address newPriceOracleSentinel
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182

```solidity
224: function setProtocolDataProvider: address newDataProvider
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224

```solidity
235: function setWETH: address newWETH
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235



#### <ins>Recommended Mitigation Steps</ins>

Consider adding explicit zero-address validation on input parameters of address type.



### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Unused `receive()` Function Will Lock Ether In Contract 

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

#### <ins>Proof Of Concept</ins>


```solidity
149: receive() external payable {}
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149



#### <ins>Recommended Mitigation Steps</ins>

The function should call another function, otherwise it should revert



### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> Missing Contract-existence Checks Before Low-level Calls

Low-level calls return success if there is no code present at the specified address. 

#### <ins>Proof Of Concept</ins>


```solidity
145: (bool success, ) = to.call{value: value}(new bytes(0));
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L145




#### <ins>Recommended Mitigation Steps</ins>

In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`



### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Contracts are not using their OZ Upgradeable counterparts

The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

#### <ins>Proof of Concept</ins>

```solidity
4: import "../dependencies/openzeppelin/contracts/AccessControl.sol";
5: import "../dependencies/openzeppelin/upgradeability/Initializable.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L4-L5

```solidity
14: import {IERC20Detailed} from "../dependencies/openzeppelin/contracts/IERC20Detailed.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L14

```solidity
12: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
13: import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L12-L13

```solidity
12: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
13: import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L12-L13

```solidity
13: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
14: import {IERC1271} from "../../dependencies/openzeppelin/contracts/IERC1271.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L13-L14

```solidity
4: import {Ownable} from "../../dependencies/openzeppelin/contracts/Ownable.sol";
10: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L4-L10

```solidity
5: import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
6: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L5-L6

```solidity
4: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L4

```solidity
4: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
5: import {IERC721Enumerable} from "../../../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
6: import {Math} from "../../../dependencies/openzeppelin/contracts/Math.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L4-L6

```solidity
4: import {IERC20} from "../../../dependencies/openzeppelin/contracts//IERC20.sol";
8: import {Math} from "../../../dependencies/openzeppelin/contracts/Math.sol";
17: import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L4-L17

```solidity
4: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
16: import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";
17: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
18: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
26: import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L4-L26

```solidity
5: import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
6: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
7: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L5-L7

```solidity
4: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
5: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L4-L5

```solidity
4: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
5: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
6: import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
25: import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L4-L25

```solidity
4: import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L4

```solidity
22: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
23: import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L22-L23

```solidity
15: import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
16: import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";
25: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
26: import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L15-L26

```solidity
21: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
22: import {IERC721Receiver} from "../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L21-L22

```solidity
4: import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
5: import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
6: import {IERC1155} from "../../dependencies/openzeppelin/contracts/IERC1155.sol";
7: import {IERC721Metadata} from "../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
8: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
10: import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";
20: import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L4-L20

```solidity
7: import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
8: import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L7-L8

```solidity
4: import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
5: import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
6: import {IERC1155} from "../../dependencies/openzeppelin/contracts/IERC1155.sol";
7: import {IERC721Metadata} from "../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
8: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
10: import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L4-L10

```solidity
4: import {IERC20} from "../../dependencies/openzeppelin/contracts/IERC20.sol";
5: import {IERC721} from "../../dependencies/openzeppelin/contracts/IERC721.sol";
6: import {IERC1155} from "../../dependencies/openzeppelin/contracts/IERC1155.sol";
7: import {IERC721Metadata} from "../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
8: import {Address} from "../../dependencies/openzeppelin/contracts/Address.sol";
9: import {SafeERC20} from "../../dependencies/openzeppelin/contracts/SafeERC20.sol";
10: import {SafeCast} from "../../dependencies/openzeppelin/contracts/SafeCast.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L4-L10

```solidity
4: import {Context} from "../../../dependencies/openzeppelin/contracts/Context.sol";
5: import {Strings} from "../../../dependencies/openzeppelin/contracts/Strings.sol";
6: import {Address} from "../../../dependencies/openzeppelin/contracts/Address.sol";
7: import {IERC165} from "../../../dependencies/openzeppelin/contracts/IERC165.sol";
8: import {IERC721Metadata} from "../../../dependencies/openzeppelin/contracts/IERC721Metadata.sol";
9: import {IERC721Receiver} from "../../../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
10: import {IERC721Enumerable} from "../../../dependencies/openzeppelin/contracts/IERC721Enumerable.sol";
13: import {SafeCast} from "../../../dependencies/openzeppelin/contracts/SafeCast.sol";
21: import {ReentrancyGuard} from "../../../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L4-L21

```solidity
4: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
5: import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";
6: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";
10: import {Math} from "../../../dependencies/openzeppelin/contracts/Math.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L4-L10

```solidity
4: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
5: import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";
6: import {IERC20} from "../../../dependencies/openzeppelin/contracts/IERC20.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L4-L6

```solidity
4: import {OwnableUpgradeable} from "../dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol";
11: import {IERC721} from "../dependencies/openzeppelin/contracts/IERC721.sol";
12: import {IERC721Receiver} from "../dependencies/openzeppelin/contracts/IERC721Receiver.sol";
17: import {ReentrancyGuard} from "../dependencies/openzeppelin/contracts/ReentrancyGuard.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L4-L17



#### <ins>Recommended Mitigation Steps</ins>

Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.



### <a href="#Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Low Level Calls With Solidity Version Before 0.8.14 Can Result In Optimiser Bug

The project contracts in scope are using low level calls with solidity version before 0.8.14 which can result in optimizer bug.
https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

Simliar findings in Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#5-low-level-calls-with-solidity-version-0814-can-result-in-optimiser-bug

#### <ins>Proof Of Concept</ins>

POC can be found in the above medium reference url.

Functions that execute low level calls in contracts with solidity version under 0.8.14

```solidity
643: assembly {
         mstore(reservesList, sub(reservesListCount, droppedReservesCount))
     }
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L643





#### <ins>Recommended Mitigation Steps</ins>

Consider upgrading to at least solidity v0.8.15.



### <a href="#Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Missing parameter validation in `constructor`

Some parameters of constructors are not checked for invalid values.

#### <ins>Proof Of Concept</ins>

```solidity
53: address fallbackOracle
54: address baseCurrency

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L53-L54

```solidity
24: address _factory
25: address _manager
26: address _addressProvider

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L24-L26

```solidity
45: string memory marketId
45: address owner
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L45

```solidity
32: address apeCoinStaking

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L32

```solidity
16: address apeCoinStaking

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L16

```solidity
16: address apeCoinStaking

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L16

```solidity
44: address _punk
45: address _wpunk
46: address _pool

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L44-L46



#### <ins>Recommended Mitigation Steps</ins>

Validate the parameters.



### <a href="#Summary">[LOW&#x2011;7]</a><a name="LOW&#x2011;7"> Prevent division by 0

On several locations in the code precautions are not being taken for not dividing by 0, this will revert the code.

These functions can be called with 0 value in the input, this value is not checked for being bigger than 0, that means in some scenarios this can potentially trigger a division by zero.

#### <ins>Proof Of Concept</ins>


```solidity
349: return userTotalDebt / assetUnit
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L349

```solidity
531: return (balance / assetUnit
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L531



#### <ins>Recommended Mitigation Steps</ins>

Recommend making sure division by 0 won’t occur by checking the variables beforehand and handling this edge case.



### <a href="#Summary">[LOW&#x2011;8]</a><a name="LOW&#x2011;8"> Upgrade OpenZeppelin Contract Dependency

An outdated OZ version is used (which has known vulnerabilities, see: https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories).

#### <ins>Proof Of Concept</ins>


```solidity
    "@openzeppelin/contracts": "4.2.0"
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/package.json#L31

```solidity
    "@openzeppelin/contracts-upgradeable": "4.2.0"
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/package.json#L32



#### <ins>Recommended Mitigation Steps</ins>

Update OpenZeppelin Contracts Usage in package.json



### <a href="#Summary">[LOW&#x2011;9]</a><a name="LOW&#x2011;9"> The `nonReentrant` modifier should occur before all other modifiers

Currently the `nonReentrant` modifier is not the first to occur, it should occur before all other modifiers.

#### <ins>Proof Of Concept</ins>


https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L263

```solidity
79: function mint(
        address onBehalfOf,
        DataTypes.ERC721SupplyParams[] calldata tokenData
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L79

```solidity
87: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L87

```solidity
119: function transferOnLiquidation(
        address from,
        address to,
        uint256 value
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L119

```solidity
199: function transferUnderlyingTo(address target, uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L199

```solidity
214: function handleRepayment(address user, uint256 amount)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L214

```solidity
102: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102

```solidity
159: function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L159

```solidity
26: function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L26

```solidity
39: function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L39

```solidity
52: function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L52

```solidity
66: function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L66

```solidity
82: function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L82

```solidity
97: function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97

```solidity
26: function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26

```solidity
39: function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L39

```solidity
52: function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L52

```solidity
66: function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L66

```solidity
82: function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L82

```solidity
97: function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97

```solidity
40: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40

```solidity
123: function decreaseUniswapV3Liquidity(
        address user,
        uint256 tokenId,
        uint128 liquidityDecrease,
        uint256 amount0Min,
        uint256 amount1Min,
        bool receiveEthAsWeth
    ) external onlyPool nonReentrant {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123

```solidity
458: function setIsUsedAsCollateral(
        uint256 tokenId,
        bool useAsCollateral,
        address sender
    ) external virtual override onlyPool nonReentrant returns (bool) {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458

```solidity
474: function batchSetIsUsedAsCollateral(
        uint256[] calldata tokenIds,
        bool useAsCollateral,
        address sender
    )
        external
        virtual
        override
        onlyPool
        nonReentrant
        returns (
            uint256 oldCollateralizedBalance,
            uint256 newCollateralizedBalance
        )
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L474

```solidity
529: function startAuction(uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L529

```solidity
540: function endAuction(uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
    {
```



#### <ins>Recommended Mitigation Steps</ins>

Re-sort modifiers so that the `nonReentrant` modifier occurs first.


### <a href="#Summary">[LOW&#x2011;10]</a><a name="LOW&#x2011;10"> `getPrice` and `combine` will not work if `expirationPeriod` == 0

The following conditions will fail if `expirationPeriod` is set to 0. There is currently no limit that it cannot be set to 0.

#### <ins>Proof Of Concept</ins>

```solidity
243: require(
244:    (block.number - updatedAt) <= config.expirationPeriod,
245:    "NFTOracle: asset price expired"
246: );
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L243-L246

```solidity
419: if (diffBlock <= config.expirationPeriod) {
420:    validPriceList[validNum] = priceInfo.twap;
421:    validNum++;
422: }
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L419-L422



## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```solidity
175: function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L175

```solidity
183: function setPause(address _asset, bool _flag)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L183

```solidity
195: function setPrice(address _asset, uint256 _twap)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L195

```solidity
221: function setMultiplePrices(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L221

```solidity
66: function setAssetSources(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L66

```solidity
74: function setFallbackOracle(address fallbackOracle)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L74

```solidity
56: function setMarketId(string memory newMarketId)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56

```solidity
70: function setAddress(bytes32 id, address newAddress)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70

```solidity
81: function setAddressAsProxy(bytes32 id, address newImplementationAddress)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L81

```solidity
121: function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L121

```solidity
142: function setPriceOracle(address newPriceOracle)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142

```solidity
158: function setACLManager(address newAclManager) external override onlyOwner {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158

```solidity
170: function setACLAdmin(address newAclAdmin) external override onlyOwner {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170

```solidity
182: function setPriceOracleSentinel(address newPriceOracleSentinel)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182

```solidity
224: function setProtocolDataProvider(address newDataProvider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224

```solidity
235: function setWETH(address newWETH) external override onlyOwner {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235

```solidity
242: function setMarketplace(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242

```solidity
413: function setSApeUseAsCollateral(address user) internal {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L413

```solidity
378: function setUserUseERC20AsCollateral(address asset, bool useAsCollateral)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L378

```solidity
397: function setUserUseERC721AsCollateral(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L397

```solidity
151: function setReserveInterestRateStrategyAddress(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151

```solidity
166: function setReserveAuctionStrategyAddress(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166

```solidity
181: function setConfiguration(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181

```solidity
206: function setAuctionRecoveryHealthFactor(uint64 value)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206

```solidity
263: function setAuctionValidityTime(address user)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L263

```solidity
136: function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L136

```solidity
131: function setIncentivesController(IRewardController controller)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131

```solidity
142: function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142

```solidity
224: function setApprovalForAll(address operator, bool approved)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L224

```solidity
458: function setIsUsedAsCollateral(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.



### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Use a more recent version of Solidity

<a href="https://blog.soliditylang.org/2022/02/16/solidity-0.8.12-release-announcement/">0.8.12</a>: 
string.concat() instead of abi.encodePacked(<str>,<str>)

<a href="https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/">0.8.13</a>: 
Ability to use using for with a list of free functions

<a href="https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/">0.8.14</a>:

ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
Override Checker: Allow changing data location for parameters only when overriding external functions.

<a href="https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/">0.8.15</a>:

Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

<a href="https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/">0.8.16</a>:

Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

<a href="https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/">0.8.17</a>:

Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

#### <ins>Proof Of Concept</ins>


```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L2

```solidity
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L2



#### <ins>Recommended Mitigation Steps</ins>

Consider updating to a more recent solidity version.



### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Event Is Missing Indexed Fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). 

Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

#### <ins>Proof Of Concept</ins>


```solidity
event OracleConfigSet(uint128 expirationPeriod, uint128 maxPriceDeviation);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L63

```solidity
event UnstakeApeIncentiveUpdated(uint256 oldValue, uint256 newValue);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L29






### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Constants Should Be Defined Rather Than Using Magic Numbers

It is not clear what the value `30` stands for and what's the rational behind its value.

#### <ins>Proof Of Concept</ins>

```solidity
133: ApeStakingLogic.executeSetUnstakeApeIncentive(dataStorage, 30);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L133





### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Missing event for critical parameter change

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

#### <ins>Proof Of Concept</ins>


```solidity
151: function setReserveInterestRateStrategyAddress(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151

```solidity
166: function setReserveAuctionStrategyAddress(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166

```solidity
181: function setConfiguration(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181

```solidity
206: function setAuctionRecoveryHealthFactor(uint64 value)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206

```solidity
263: function setAuctionValidityTime(address user)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L263

```solidity
131: function setIncentivesController(IRewardController controller)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131

```solidity
142: function setBalanceLimit(uint64 limit) external onlyPoolAdmin {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142


```solidity
458: function setIsUsedAsCollateral(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458





### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```solidity
49: constructor(
        IPoolAddressesProvider provider,
        address[] memory assets,
        address[] memory sources,
        address fallbackOracle,
        address baseCurrency,
        uint256 baseCurrencyUnit
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L49

```solidity
23: constructor(
        address _factory,
        address _manager,
        address _addressProvider
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L23

```solidity
45: constructor(string memory marketId, address owner)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L45

```solidity
50: constructor(
        uint256 maxPriceMultiplier,
        uint256 minExpPriceMultiplier,
        uint256 minPriceMultiplier,
        uint256 stepLinear,
        uint256 stepExp,
        uint256 tickLength
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L50

```solidity
51: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L51

```solidity
64: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L64

```solidity
62: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L62

```solidity
89: constructor(IPoolAddressesProvider provider)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L89

```solidity
43: constructor(IPool pool, bool atomic_pricing)
        MintableIncentivizedERC721(
            pool,
            "NTOKEN_IMPL",
            "NTOKEN_IMPL",
            atomic_pricing
        )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L43

```solidity
32: constructor(IPool pool, address apeCoinStaking) NToken(pool, false)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L32

```solidity
16: constructor(IPool pool, address apeCoinStaking)
        NTokenApeStaking(pool, apeCoinStaking)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L16

```solidity
16: constructor(IPool pool, address apeCoinStaking)
        NTokenApeStaking(pool, apeCoinStaking)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L16

```solidity
32: constructor(IPool pool) NToken(pool, false)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L32

```solidity
33: constructor(IPool pool) NToken(pool, true)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L33

```solidity
83: constructor(
        IPool pool,
        string memory name_,
        string memory symbol_,
        bool atomic_pricing
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L83

```solidity
43: constructor(
        address _punk,
        address _wpunk,
        address _pool
    )
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/ui/WPunkGateway.sol#L43





### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
References: SWC ID: 116

#### <ins>Proof Of Concept</ins>


```solidity
778: .calculateAuctionPriceMultiplier(startTime, block.timestamp);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L778

```solidity
285: userConfig.auctionValidityTime = block.timestamp;
```


#### <ins>Recommended Mitigation Steps</ins>
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.



### <a href="#Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

A good example:
```solidity
import {OwnableUpgradeable} from "openzeppelin-contracts-upgradeable/contracts/access/OwnableUpgradeable.sol";
import {SafeTransferLib} from "solmate/utils/SafeTransferLib.sol";
import {SafeCastLib} from "solmate/utils/SafeCastLib.sol";
import {ERC20} from "solmate/tokens/ERC20.sol";
import {IProducer} from "src/interfaces/IProducer.sol";
import {GlobalState, UserState} from "src/Common.sol";
```

#### <ins>Proof Of Concept</ins>

```solidity
4: import "../dependencies/openzeppelin/contracts/AccessControl.sol";
5: import "../dependencies/openzeppelin/upgradeability/Initializable.sol";
6: import "./interfaces/INFTFloorOracle.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L4-L6

```solidity
10: import "../../../interfaces/INTokenApeStaking.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L10

```solidity
30: import "../../../interfaces/INTokenApeStaking.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L30

```solidity
4: import "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
5: import "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
7: import "../../interfaces/IPoolApeStaking.sol";
8: import "../../interfaces/IPToken.sol";
9: import "../../dependencies/yoga-labs/ApeCoinStaking.sol";
10: import "../../interfaces/IXTokenType.sol";
11: import "../../interfaces/INTokenApeStaking.sol";
20: import "../libraries/logic/BorrowLogic.sol";
21: import "../libraries/logic/SupplyLogic.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L4-L21

```solidity
12: import "../../interfaces/INTokenApeStaking.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L12

```solidity
7: import "../../../interfaces/IPool.sol";
11: import "./MintableERC721Logic.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L7

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L11



```solidity
7: import "../../../interfaces/IRewardController.sol";
8: import "../../libraries/types/DataTypes.sol";
9: import "../../../interfaces/IPool.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L7-L9



#### <ins>Recommended Mitigation Steps</ins>

Use specific imports syntax per solidity docs recommendation.



### <a href="#Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

#### <ins>Proof Of Concept</ins>

```solidity
70: bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L70

```solidity
16: bytes32 constant POOL_STORAGE_POSITION =
        bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16

```solidity
23: bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
        bytes32(
            uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23





### <a href="#Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Use `bytes.concat()`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

#### <ins>Proof Of Concept</ins>


```solidity
63: orderInfo.id = abi.encodePacked(makerAsk.r, makerAsk.s, makerAsk.v)
86: bytes memory data = abi.encodePacked(selector, params)

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L63

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L86



```solidity
99: bytes memory data = abi.encodePacked(selector, params)
115: bytes memory data = abi.encodePacked(selector, params)
99: bytes memory data = abi.encodePacked(selector, params)
115: bytes memory data = abi.encodePacked(selector, params)

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L99

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L115

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L99

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L115



```solidity
75: orderInfo.id = abi.encodePacked(order.r, order.s, order.v)
94: bytes memory data = abi.encodePacked(selector, params)

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L75

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L94



```solidity
1076: keccak256(abi.encodePacked(params.orderInfo.id)
1077: keccak256(abi.encodePacked(params.credit.orderId)
1116: abi.encodePacked(
                "Credit(address token,uint256 amount,bytes orderId)
1128: keccak256(abi.encodePacked(credit.orderId)

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1076

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1077

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1116

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1128






#### <ins>Recommended Mitigation Steps</ins>

Use `bytes.concat()` and upgrade to at least Solidity version 0.8.4 if required. 






### <a href="#Summary">[NC&#x2011;11]</a><a name="NC&#x2011;1"> Open TODOs

An open TODO is present. It is recommended to avoid open TODOs as they may indicate programming errors that still need to be fixed.

#### <ins>Proof Of Concept</ins>


```solidity
238: // TODO using bit shifting for the 2^96
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238

```solidity
59: makerAsk.price, // TODO: take minPercentageToAsk into account
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59

```solidity
442: // TODO: support PToken
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442






### <a href="#Summary">[NC&#x2011;12]</a><a name="NC&#x2011;12"> No need to initialize uints to zero

There is no need to initialize `uint` variables to zero as their default value is `0`

#### <ins>Proof Of Concept</ins>

```solidity
411: uint256 validNum = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L411

```solidity
125: uint256 price = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L125

```solidity
208: uint256 sum = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L208

```solidity
380: uint256 price = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L380

```solidity
71: uint256 amountToWithdraw = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L71

```solidity
135: uint256 amountToWithdraw = 0;
137: uint256 actualTransferAmount = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L135

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L137



```solidity
631: uint256 droppedReservesCount = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L631

```solidity
205: uint64 collateralizedTokens = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L205

```solidity
272: uint64 burntCollateralizedTokens = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272





### <a href="#Summary">[NC&#x2011;13]</a><a name="NC&#x2011;13"> Use `delete` to Clear Variables

`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at `x`.

The `delete` key better conveys the intention and is also more idiomatic. Consider replacing assignments of zero with `delete` statements.

#### <ins>Proof Of Concept</ins>

```solidity
380: price = 0;
410: downpayment = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L380

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L410



```solidity
589: vars.ethLeft = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L589

```solidity
123: reserve.accruedToTreasury = 0;

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L123





### <a href="#Summary">[NC&#x2011;14]</a><a name="NC&#x2011;14"> Duplicate imports

There are several occasions where there several imports of the same file

#### <ins>Proof Of Concept</ins>


```solidity
4: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";
18: import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L4

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L18



```solidity
5: import {Errors} from "../libraries/helpers/Errors.sol";
25: import {Errors} from "../libraries/helpers/Errors.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L5

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L25



```solidity
5: import {Errors} from "../libraries/helpers/Errors.sol";
28: import {Errors} from "../libraries/helpers/Errors.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L5

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L28



```solidity
5: import {Errors} from "../libraries/helpers/Errors.sol";
24: import {Errors} from "../libraries/helpers/Errors.sol";

```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L5

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L24


### <a href="#Summary">[NC&#x2011;15]</a><a name="NC&#x2011;15"> Missing NATSPEC documentation

#### <ins>Proof Of Concept</ins>

```solidity
25: function getAskOrderInfo(bytes memory params, address weth)
67: function getBidOrderInfo(
73: function matchAskWithTakerBid(
96: function matchBidWithTakerAsk(address, bytes calldata)
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

```solidity
25: function getAskOrderInfo(bytes memory params, address weth)
60: function getBidOrderInfo(
93: function matchAskWithTakerBid(
109: function matchBidWithTakerAsk(address, bytes calldata)
124: function isBasicOrder(AdvancedOrder memory advancedOrder)
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

```solidity
31: function getAskOrderInfo(bytes memory params, address weth)
79: function getBidOrderInfo(
88: function matchAskWithTakerBid(
104: function matchBidWithTakerAsk(address, bytes calldata)
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

```solidity
261: function getFeederSize() public view returns (uint256) {
265: function _whenNotPaused(address _asset) internal view {
270: function _isAssetExisted(address _asset) internal view returns (bool) {
274: function _isFeederExisted(address _feeder) internal view returns (bool) {
278: function _addAsset(address _asset)
296: function _removeAsset(address _asset)
307: function _addFeeder(address _feeder)
326: function _removeFeeder(address _feeder)
351: function _checkValidity(address _asset, uint256 _twap)
376: function _finalizePrice(address _asset, uint256 _twap) internal {
388: function _addRawValue(address _asset, uint256 _twap) internal {
397: function _combine(address _asset, uint256 _twap)
432: function _quickSort(
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

```solidity
138: function getTokenPrice(address asset, uint256 tokenId)
155: function getTokensPrices(address asset, uint256[] calldata tokenIds)
172: function getTokensPricesSum(address asset, uint256[] calldata tokenIds)
218: function _onlyAssetListingOrPoolAdmins() internal view {
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol


https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```solidity
399: function getLtvAndLTForUniswapV3(
450: function _getUserBalanceForUniswapV3(
535: function _getAssetPrice(address oracle, address currentReserveAddress)
543: function _getTokenPrice(
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

```solidity
63: function executeBuyWithCredit(
160: function executeBatchBuyWithCredit(
229: function executeAcceptBidWithCre
267: function executeBatchAcceptBidWithCredit(
552: function _checkAllowance(address token, address operator) internal {
559: function _cache(DataTypes.ExecuteMarketplaceParams memory params)
569: function _refundETH(uint256 ethLeft) internal {
575: function _depositETH(
593: function _transferAndCollateralize(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```solidity
214: function executeGetAssetLtvAndLT(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol


```solidity
134: function executeSupplyERC721Base(
335: function executeWithdrawERC721(
396: function executeDecreaseUniswapV3Liquidity(
```
https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

```solidity
189: function validateWithdrawERC721(
442: function validateSetUseERC721AsCollateral(
801: function validateEndAuction(
1065: function validateBuyWithCredit(
1071: function validateAcceptBidWithCredit(
1092: function verifyCreditSignature(
1110: function hashCredit(DataTypes.Credit memory credit)
1133: function getDomainSeparator() internal view returns (bytes32) {
1155: function validateForUniswapV3(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

```solidity
90: function calculateAuctionPriceMultiplier(
101: function _calculateAuctionPriceMultiplierByTicks(uint256 ticks)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

```solidity
413: function setSApeUseAsCollateral(address user) internal {
428: function getUserHf(address user) internal view returns (uint256) {
443: function checkSApeIsNotPaused(DataTypes.PoolStorage storage ps)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

```solidity
237: function decreaseUniswapV3Liquidity(
397: function setUserUseERC721AsCollateral(
793: function onERC721Received(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolCore.sol

```solidity
251: function getAssetLtvAndLT(address asset, uint256 tokenId)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol

```solidity
290: function _safeTransferFrom(
364: function _mintMultiple(
384: function _burnMultiple(address user, uint256[] calldata tokenIds)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

```solidity
95: function _burn(
127: function rescueERC20(
136: function rescueERC721(
151: function rescueERC1155(
168: function executeAirdrop(
271: function onERC721Received(
280: function onERC1155Received(
295: function onERC1155BatchReceived(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NToken.sol

```solidity
130: function initializeStakingData() internal {
136: function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

```solidity
144: function _safeTransferETH(address to, uint256 value) internal {
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

```solidity
80: function executeTransfer(
135: function executeTransferCollateralizable(
153: function executeSetIsUsedAsCollateral(
187: function executeMintMultiple(
260: function executeBurnMultiple(
340: function executeApprove(
348: function _approve(
357: function executeApprovalForAll(
368: function executeStartAuction(
386: function executeEndAuction(
402: function _checkBalanceLimit(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

### <a href="#Summary">[NC&#x2011;15]</a><a name="NC&#x2011;15"> Insufficient NATSPEC documentation

#### <ins>Proof Of Concept</ins>

Missing `@param` for `expirationPeriod` and `maxPriceDeviation`

```solidity
175: function setConfig(uint128 expirationPeriod, uint128 maxPriceDeviation)
```

Missing `@param` for `_asset` and `_flag`

```solidity
183: setPause(address _asset, bool _flag)
```

Missing `@param` for `_twaps`

```solidity
183: function setMultiplePrices(
```
Missing `@notice`

```solidity
236: getPrice(address _asset)
252: getLastUpdateTime(address _asset)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

Missing `@param` for `params` and `vars`

```solidity
362: function _getUserBalanceForERC721(
```

Missing `@param` for `reserve`, `xTokenAddress` and `assetPrice`

```solidity
362: function _getUserBalanceForERC721(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

Missing `@param` for `reservesData`

```solidity
564: function _supplyNewCollateral(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

Missing `@return` for `uint256`

```solidity
114: function _buyWithCredit(
```

Missing `@return` for `uint256` and `uin256`

```solidity
376: function _delegateToPool(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

Missing `@param` for `user`, `ps` and `oracle`

```solidity
166: function executeGetUserAccountData(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

Missing `@param` for `assetType`

```solidity
63: function validateSupply(
```

Missing `@param` for `reservesData`

```solidity
590: function validateLiquidateERC721(
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

Missing `@return` for `uint256`

```solidity
182: function getUserApeStakingAmount(address user)
```

https://github.com/code-423n4/2022-11-paraspace/tree/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol