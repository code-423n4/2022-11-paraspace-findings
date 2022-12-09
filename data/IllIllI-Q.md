## Summary


### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | Misleading variable naming/documentation | 1 |
| [L&#x2011;02] | Wrong interest during leap years | 1 |
| [L&#x2011;03] | Fallback oracle may break with future NFTs | 1 |
| [L&#x2011;04] | `latestAnswer()` is deprecated | 1 |
| [L&#x2011;05] | `tokenURI()` does not follow EIP-721 | 2 |
| [L&#x2011;06] | Owner can renounce while system is paused | 1 |
| [L&#x2011;07] | Empty `receive()`/`payable fallback()` function does not authenticate requests | 1 |
| [L&#x2011;08] | `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()` | 1 |
| [L&#x2011;09] | Use `Ownable2Step` rather than `Ownable` | 1 |
| [L&#x2011;10] | Open TODOs | 3 |
| [L&#x2011;11] | Centralization risks | 2 |

Total: 13 instances over 10 issues

### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | EIP-1967 storage slots should use the `eip1967.proxy` prefix | 2 |
| [N&#x2011;02] | Input addresse to `_safeTransferETH()` should be payable | 1 |
| [N&#x2011;03] | Functions that must be overridden should be virtual, with no body | 2 |
| [N&#x2011;04] | Inconsistent safe transfer library used | 1 |
| [N&#x2011;05] | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 1 |
| [N&#x2011;06] | Import declarations should import specific identifiers, rather than the whole file | 20 |
| [N&#x2011;07] | Missing `initializer` modifier on constructor | 1 |
| [N&#x2011;08] | Duplicate import statements | 9 |
| [N&#x2011;09] | The `nonReentrant` `modifier` should occur before all other modifiers | 25 |
| [N&#x2011;10] | `override` function arguments that are unused should have the variable name removed or commented out to avoid compiler warnings | 2 |
| [N&#x2011;11] | `2**<n> - 1` should be re-written as `type(uint<n>).max` | 3 |
| [N&#x2011;12] | `constant`s should be defined rather than using magic numbers | 16 |
| [N&#x2011;13] | Numeric values having to do with time should use time units for readability | 1 |
| [N&#x2011;14] | Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability | 1 |
| [N&#x2011;15] | Events that mark critical parameter changes should contain both the old and the new value | 3 |
| [N&#x2011;16] | Use a more recent version of solidity | 4 |
| [N&#x2011;17] | Use a more recent version of solidity | 16 |
| [N&#x2011;18] | Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant` | 1 |
| [N&#x2011;19] | Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`) | 1 |
| [N&#x2011;20] | Inconsistent spacing in comments | 33 |
| [N&#x2011;21] | Lines are too long | 2 |
| [N&#x2011;22] | Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables | 3 |
| [N&#x2011;23] | NatSpec is incomplete | 39 |
| [N&#x2011;24] | Not using the named return variables anywhere in the function is confusing | 18 |
| [N&#x2011;25] | Duplicated `require()`/`revert()` checks should be refactored to a modifier or function | 32 |
| [N&#x2011;26] | Consider using `delete` rather than assigning zero to clear values | 2 |
| [N&#x2011;27] | Contracts should have full test coverage | 1 |
| [N&#x2011;28] | Large or complicated code bases should implement fuzzing tests | 1 |
| [N&#x2011;29] | Typos | 3 |

Total: 244 instances over 29 issues


## Low Risk Issues

### [L&#x2011;01]  Misleading variable naming/documentation
The `recieveEthAsWeth` argument, if true, will do what the comment says: convert WETH to ETH, even though the variable name says the opposite. There is no way to know which one is right, without reading the code, which will lead to problems for callers of the external version of the function, `decreaseUniswapV3Liquidity()`

*There is 1 instance of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

41       /**
42        * @notice A function that decreases the current liquidity.
43        * @param tokenId The id of the erc721 token
44        * @param liquidityDecrease The amount of liquidity to remove of LP
45        * @param amount0Min The minimum amount to remove of token0
46        * @param amount1Min The minimum amount to remove of token1
47        * @param receiveEthAsWeth If convert weth to ETH
48        * @return amount0 The amount received back in token0
49        * @return amount1 The amount returned back in token1
50        */
51       function _decreaseLiquidity(
52           address user,
53           uint256 tokenId,
54           uint128 liquidityDecrease,
55           uint256 amount0Min,
56           uint256 amount1Min,
57           bool receiveEthAsWeth
58:      ) internal returns (uint256 amount0, uint256 amount1) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L41-L58

### [L&#x2011;02]  Wrong interest during leap years
The `calculateLinearInterest()` function uses `SECONDS_PER_YEAR`, which is a constant, so the constant will have the wrong value during leap years. While the function is out of scope, it's eventually called by `PoolCore:getNormalizedIncome()`, which _is_ in scope.

*There is 1 instance of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/libraries/math/MathUtils.sol

23       function calculateLinearInterest(uint256 rate, uint40 lastUpdateTimestamp)
24           internal
25           view
26           returns (uint256)
27       {
28           //solium-disable-next-line
29           uint256 result = rate *
30               (block.timestamp - uint256(lastUpdateTimestamp));
31           unchecked {
32               result = result / SECONDS_PER_YEAR;
33           }
34   
35           return WadRayMath.RAY + result;
36:      }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/math/MathUtils.sol#L23-L36

### [L&#x2011;03]  Fallback oracle may break with future NFTs
In order for the fallback oracle to fall back to the Bend DAO oracle, the NFT in question must say that it in fact `supportsInterface(0x80ac58cd)`. The EIP-721 standard says that the implementer MUST do this, and both Openzeppelin's and Solmate's impelemntations do, but in the future the ParaSpace protocol may want to support a token that does not. I believe this deserves low rather than non-critical, because the damage won't be known until one of the other oracles fail. The fallback oracle is supposed to be the last resort before the protocol is unable to price/liquidate anything, so if the issue isn't caught before then, things could get stuck at the worst possible time. I would suggest to `require()` that the NFT supports the NFT at the point where it's [added](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L278-L286), to avoid having to event think about this edge case in the future

*There is 1 instance of this issue:*
```solidity
File: /paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol

35           try IERC165(asset).supportsInterface(INTERFACE_ID_ERC721) returns (
36               bool supported
37           ) {
38               if (supported == true) {
39                   return INFTOracle(BEND_DAO).getAssetPrice(asset);
40               }
41:          } catch {}

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceFallbackOracle.sol#L35-L41

### [L&#x2011;04]  `latestAnswer()` is deprecated
Use `latestRoundData()` instead so that you can tell whether the answer is stale or not. The `latestAnswer()` function returns zero if it is unable to fetch data, which may be the case if ChainLink stops supporting this API. The API and its deprecation message no longer even appear on the ChainLink website, so it is dangerous to continue using it.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

128:              price = uint256(source.latestAnswer());

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L128

### [L&#x2011;05]  `tokenURI()` does not follow EIP-721
The [EIP](https://eips.ethereum.org/EIPS/eip-721) states that `tokenURI()` "Throws if `_tokenId` is not a valid NFT", which the code below does not do. If the NFT has not yet been minted, `tokenURI()` should revert

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

178       function tokenURI(uint256)
179           external
180           view
181           virtual
182           override
183           returns (string memory)
184       {
185           return "";
186:      }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L178-L186

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

313       function tokenURI(uint256 tokenId)
314           public
315           view
316           virtual
317           override
318           returns (string memory)
319       {
320           return IERC721Metadata(_underlyingAsset).tokenURI(tokenId);
321:      }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L313-L321

### [L&#x2011;06]  Owner can renounce while system is paused
The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

183       function setPause(address _asset, bool _flag)
184           external
185           onlyRole(DEFAULT_ADMIN_ROLE)
186       {
187           assetFeederMap[_asset].paused = _flag;
188           emit AssetPaused(_asset, _flag);
189:      }

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L183-L189

### [L&#x2011;07]  Empty `receive()`/`payable fallback()` function does not authenticate requests
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. `require(msg.sender == address(weth))`). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

149:      receive() external payable {}

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149

### [L&#x2011;08]  `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
Use `abi.encode()` instead which will pad items to 32 bytes, which will [prevent hash collisions](https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode) (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). "Unless there is a compelling reason, `abi.encode` should be preferred". If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` [instead](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739).

If all arguments are strings and or bytes, `bytes.concat()` should be used instead

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

1115          bytes32 typeHash = keccak256(
1116              abi.encodePacked(
1117                  "Credit(address token,uint256 amount,bytes orderId)"
1118              )
1119:         );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1115-L1119

### [L&#x2011;09]  Use `Ownable2Step` rather than `Ownable`
[`Ownable2Step`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/3d7a93876a2e5e1d7fe29b5a0e96e222afdc4cfa/contracts/access/Ownable2Step.sol#L31-L56) and [`Ownable2StepUpgradeable`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/25aabd286e002a1526c345c8db259d57bdf0ad28/contracts/access/Ownable2StepUpgradeable.sol#L47-L63) prevent the contract ownership from mistakenly being transferred to an address that cannot handle it (e.g. due to a typo in the address), by requiring that the recipient of the owner permissions actively accept via a contract call of its own.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

19    contract WPunkGateway is
20        ReentrancyGuard,
21        IWPunkGateway,
22        IERC721Receiver,
23:       OwnableUpgradeable

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L23

### [L&#x2011;10]  Open TODOs
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

*There are 3 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

59:               makerAsk.price, // TODO: take minPercentageToAsk into account

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L59

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

238:          // TODO using bit shifting for the 2^96

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L238

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

442:          // TODO: support PToken

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L442


### [L&#x2011;11]  Centralization risks
The admin can set whatever price they wish, causing anyone with NFT collateral to be liquidatable. The admin can also set a WETH address that just steals the funds. These operations should have more checks for market conditions before being allowed.

```solidity
File: /paraspace-core/contracts/misc/NFTFloorOracle.sol   #1

195      function setPrice(address _asset, uint256 _twap)
196          public
197          onlyRole(UPDATER_ROLE)
198          onlyWhenAssetExisted(_asset)
199          whenNotPaused(_asset)
200      {
201          bool dataValidity = false;
202          if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) {
203              _finalizePrice(_asset, _twap);
204              return;
205:         }
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L195-L205

```solidity
File: /paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol   #2

235      function setWETH(address newWETH) external override onlyOwner {
236          address oldWETH = _addresses[WETH];
237          _addresses[WETH] = newWETH;
238          emit WETHUpdated(oldWETH, newWETH);
239:     }
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235-L239


## Non-critical Issues

### [N&#x2011;01]  EIP-1967 storage slots should use the `eip1967.proxy` prefix
Using the prefix makes it easier to confirm that you are following the standard, and not altering the behavior in some incompatable way

*There are 2 instances of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/pool/PoolStorage.sol

/// @audit use `eip1967.proxy.paraspace.pool.storage`
17:          bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L17

```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit use `eip1967.proxy.paraspace.ntoken.apestaking.storage`
25:              uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L25

### [N&#x2011;02]  Input addresse to `_safeTransferETH()` should be payable
While it doesn't affect any contract operation, it adds a degree of type safety during compilation, and is done by ``OpenZeppelin``(https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol#L60-L65)

*There is 1 instance of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

144:     function _safeTransferETH(address to, uint256 value) internal {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L144

### [N&#x2011;03]  Functions that must be overridden should be virtual, with no body

*There are 2 instances of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

126:         // should be overridden

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L126

```solidity
File: /paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

185:         return "";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L185

### [N&#x2011;04]  Inconsistent safe transfer library used
Most places in the code use `GPv2SafeERC20`, but this one uses `SafeERC20`. All areas should use the same libraries

*There is 1 instance of this issue:*
```solidity
File: /paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

16:  import {SafeERC20} from "../../../dependencies/openzeppelin/contracts/SafeERC20.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L16

### [N&#x2011;05]  Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions
See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

19    contract WPunkGateway is
20        ReentrancyGuard,
21        IWPunkGateway,
22        IERC721Receiver,
23:       OwnableUpgradeable

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L19-L23

### [N&#x2011;06]  Import declarations should import specific identifiers, rather than the whole file
Using import declarations of the form `import {<identifier_name>} from "some/file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation

*There are 20 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

4:    import "../dependencies/openzeppelin/contracts/AccessControl.sol";

5:    import "../dependencies/openzeppelin/upgradeability/Initializable.sol";

6:    import "./interfaces/INFTFloorOracle.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L4

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

10:   import "../../../interfaces/INTokenApeStaking.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L10

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

30:   import "../../../interfaces/INTokenApeStaking.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L30

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

4:    import "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";

5:    import "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";

7:    import "../../interfaces/IPoolApeStaking.sol";

8:    import "../../interfaces/IPToken.sol";

9:    import "../../dependencies/yoga-labs/ApeCoinStaking.sol";

10:   import "../../interfaces/IXTokenType.sol";

11:   import "../../interfaces/INTokenApeStaking.sol";

20:   import "../libraries/logic/BorrowLogic.sol";

21:   import "../libraries/logic/SupplyLogic.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L4

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

7:    import "../../../interfaces/IPool.sol";

11:   import "./MintableERC721Logic.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L7

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

7:    import "../../../interfaces/IRewardController.sol";

8:    import "../../libraries/types/DataTypes.sol";

9:    import "../../../interfaces/IPool.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L7

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

12:   import "../../interfaces/INTokenApeStaking.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L12

### [N&#x2011;07]  Missing `initializer` modifier on constructor
OpenZeppelin [recommends](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5) that the `initializer` modifier be applied to constructors in order to avoid potential griefs, [social engineering](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/4), or exploits. Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

55:   contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L55

### [N&#x2011;08]  Duplicate import statements

*There are 9 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

11:   import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L11

```solidity
File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

11:   import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L11

```solidity
File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

12:   import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L12

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

18:   import {IERC721} from "../../../dependencies/openzeppelin/contracts/IERC721.sol";

21:   import {AdvancedOrder, CriteriaResolver, Fulfillment} from "../../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L18

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

25:   import {Errors} from "../libraries/helpers/Errors.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L25

```solidity
File: paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

28:   import {Errors} from "../libraries/helpers/Errors.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L28

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

24:   import {Errors} from "../libraries/helpers/Errors.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L24

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

12:   import {IMoonBird} from "../../dependencies/erc721-collections/IMoonBird.sol";

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L12

### [N&#x2011;09]  The `nonReentrant` `modifier` should occur before all other modifiers
This is a best-practice to protect against reentrancy in other modifiers

*There are 25 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

462:      ) external virtual override onlyPool nonReentrant returns (bool) {

483:          nonReentrant

534:          nonReentrant

545:          nonReentrant

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L462

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

106:      ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

162:          nonReentrant

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L106

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

29:           nonReentrant

42:           nonReentrant

55:       ) external onlyPool nonReentrant {

69:           nonReentrant

85:       ) external onlyPool nonReentrant {

100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

29:           nonReentrant

42:           nonReentrant

55:       ) external onlyPool nonReentrant {

69:           nonReentrant

85:       ) external onlyPool nonReentrant {

100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

44:       ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L44

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

82:       ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

91:       ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

123:      ) external onlyPool nonReentrant {

204:          nonReentrant

219:          nonReentrant

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L82

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

130:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L130

### [N&#x2011;10]  `override` function arguments that are unused should have the variable name removed or commented out to avoid compiler warnings

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

214:      function handleRepayment(address user, uint256 amount)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L214

### [N&#x2011;11]  `2**<n> - 1` should be re-written as `type(uint<n>).max`
Earlier versions of solidity can use `uint<n>(-1)` instead. Expressions not including the `- 1` can often be re-written to accomodate the change (e.g. by using a `>` rather than a `>=`, which will also save some gas)

*There are 3 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

247:                  ) * 2**96) / 1E9

259:                  ) * 2**96) / 1E9

271:                  ) * 2**96) /

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L247

### [N&#x2011;12]  `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 16 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit 100
366:              ? (_twap * 100) / _priorTwap

/// @audit 100
367:              : (_priorTwap * 100) / _twap;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L366

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

/// @audit 18
245:                      ((oracleData.token0Price * (10**18)) /

/// @audit 96
/// @audit 1E9
247:                  ) * 2**96) / 1E9

/// @audit 96
/// @audit 1E9
259:                  ) * 2**96) / 1E9

/// @audit 96
271:                  ) * 2**96) /

/// @audit 9
273:                          (9 +

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

/// @audit 0x0
143:                  ) == address(0x0),

/// @audit 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f
1137:                     0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")

/// @audit 0x88d989289235fb06c18e3c2f7ea914f41f773e86fb0073d632539f566f4df353
1138:                     0x88d989289235fb06c18e3c2f7ea914f41f773e86fb0073d632539f566f4df353, // keccak256("ParaSpace")

/// @audit 0x722c0e0c80487266e8c6a45e3a1a803aab23378a9c32e6ebe029d4fad7bfc965
1139:                     0x722c0e0c80487266e8c6a45e3a1a803aab23378a9c32e6ebe029d4fad7bfc965, // keccak256(bytes("1.1")),

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L143

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit 30
133:          ApeStakingLogic.executeSetUnstakeApeIncentive(dataStorage, 30);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L133

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit 4
176:          require(airdropParams.length >= 4, Errors.INVALID_AIRDROP_PARAMETERS);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L176

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

/// @audit 30
34:           _ERC721Data.balanceLimit = 30;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L34

### [N&#x2011;13]  Numeric values having to do with time should use time units for readability
There are [units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units) for seconds, minutes, hours, days, and weeks, and since they're defined, they should be used

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit 1800
12:   uint128 constant EXPIRATION_PERIOD = 1800;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12

### [N&#x2011;14]  Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

21:       uint256 internal constant Q128 = 0x100000000000000000000000000000000;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L21

### [N&#x2011;15]  Events that mark critical parameter changes should contain both the old and the new value
This should especially be done if the new value is not required to be different from the old value

*There are 3 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit setPause()
188:          emit AssetPaused(_asset, _flag);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L188

```solidity
File: paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

/// @audit updatePoolImpl()
112:          emit PoolUpdated(implementationParams, _init, _calldata);

/// @audit setMarketplace()
255:          emit MarketplaceUpdated(id, marketplace, adapter, operator, paused);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L112

### [N&#x2011;16]  Use a more recent version of solidity
Use a solidity version of at least 0.8.12 to get `string.concat()` to be used instead of `abi.encodePacked(<str>,<str>)`

*There are 4 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L2

```solidity
File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L2

```solidity
File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L2

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L2

### [N&#x2011;17]  Use a more recent version of solidity
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions

*There are 16 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol#L2

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L2

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L2

```solidity
File: paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L2

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L2

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L2

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L2

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L2

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L2

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

2:    pragma solidity 0.8.10;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L2

### [N&#x2011;18]  Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`
While it **doesn't save any gas** because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

70:       bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L70

### [N&#x2011;19]  Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`)
While the compiler knows to optimize away the exponentiation, it's still better coding practice to use idioms that do not require compiler optimization, if they exist

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

245:                      ((oracleData.token0Price * (10**18)) /

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245

### [N&#x2011;20]  Inconsistent spacing in comments
Some lines use `// x` and some use `//x`. The instances below point out the usages that don't follow the majority, within each file

*There are 33 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

105:          //still need to grant update_role to admin for emergency call

361:          //first price is always valid

404:          //first time just use the feeding value

408:          //use memory here so allocate with maximum length

412:          //aggeregate with price from all feeders

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L105

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

100:          //userCollateral from collateralReserve

102:          //userGlobalCollateral from all reserves

104:          //userDebt from liquadationReserve

106:          //userGlobalDebt from all reserves

108:          //actualLiquidationAmount to repay based on collateral

110:          //actualCollateral allowed to liquidate

112:          //liquidationBonusRate from reserve config

114:          //user health factor

116:          //liquidation protocol fee to be sent to treasury

118:          //liquidation funds payer

120:          //collateral P|N Token

122:          //auction strategy

124:          //liquidation asset reserve id

126:          //whether auction is enabled

128:          //liquidation reserve cache

314:              vars.userGlobalDebt, //in base currency

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L100

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

34:       // See `IPool` for descriptions

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L34

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

353:          //add the current already borrowed amount to the amount requested to calculate the total collateral needed.

355:              vars.amountInBaseCurrency).percentDiv(vars.currentLtv); //LTV is calculated in percentage

573:          //if collateral isn't enabled as collateral by user, it cannot be liquidated

685:          //if collateral isn't enabled as collateral by user, it cannot be liquidated

794:          //if collateral isn't enabled as collateral by user, it cannot be auctioned

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L353

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

155:              //only partially withdraw need user's BAKC

171:          ////transfer BAKC back for user

214:          //transfer BAKC back for user

304:          //transfer BAKC back for user

337:          //7 checkout ape balance

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L155

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

258:          //solhint-disable-next-line max-line-length

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L258

### [N&#x2011;21]  Lines are too long
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

603:       * @return The actual debt that is getting liquidated. If liquidation amount passed in by the liquidator is greater then the total user debt, then use the user total debt as the actual debt getting liquidated. If the user total debt is greater than the liquidation amount getting passed in by the liquidator, then use the liquidation amount the user is passing in.

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

1137:                     0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f, // keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1137

### [N&#x2011;22]  Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables
If the variable needs to be different based on which class it comes from, a `view`/`pure` _function_ should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

*There are 3 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

78:           IPool POOL;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L78

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

426:          IPool POOL,

404:          bool ATOMIC_PRICING,

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L426

### [N&#x2011;23]  NatSpec is incomplete

*There are 39 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit Missing: '@param _twaps'
218       /// @notice Allows owner to set new price on PriceInformation and updates the
219       /// internal Median cumulativePrice.
220       /// @param _assets The nft contract to set a floor price for
221       function setMultiplePrices(
222           address[] calldata _assets,
223           uint256[] calldata _twaps
224:      ) external onlyRole(UPDATER_ROLE) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L218-L224

```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

/// @audit Missing: '@return'
111        * @param positionData The specified position data
112        */
113       function getLiquidityAmountFromPositionData(
114           UinswapV3PositionData memory positionData
115:      ) public pure returns (uint256 token0Amount, uint256 token1Amount) {

/// @audit Missing: '@return'
142        * @param positionData The specified position data
143        */
144       function getLpFeeAmountFromPositionData(
145           UinswapV3PositionData memory positionData
146:      ) public view returns (uint256 token0Amount, uint256 token1Amount) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L111-L115

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

/// @audit Missing: '@param params'
/// @audit Missing: '@param vars'
356       /**
357        * @notice Calculates total xToken balance of the user in the based currency used by the price oracle
358        * @dev For gas reasons, the xToken balance is calculated by fetching `scaledBalancesOf` normalized debt, which
359        * is cheaper than fetching `balanceOf`
360        * @return totalValue The total xToken balance of the user normalized to the base currency of the price oracle
361        **/
362       function _getUserBalanceForERC721(
363           DataTypes.CalculateUserAccountDataParams memory params,
364           CalculateUserAccountDataVars memory vars
365:      ) private view returns (uint256 totalValue) {

/// @audit Missing: '@param reserve'
/// @audit Missing: '@param xTokenAddress'
/// @audit Missing: '@param assetPrice'
504       /**
505        * @notice Calculates total xToken balance of the user in the based currency used by the price oracle
506        * @dev For gas reasons, the xToken balance is calculated by fetching `scaledBalancesOf` normalized debt, which
507        * is cheaper than fetching `balanceOf`
508        * @param user The address of the user
509        * @param assetUnit The value representing one full unit of the asset (10^decimals)
510        * @return The total xToken balance of the user normalized to the base currency of the price oracle
511        **/
512       function _getUserBalanceForERC20(
513           address user,
514           DataTypes.ReserveData storage reserve,
515           address xTokenAddress,
516           uint256 assetUnit,
517           uint256 assetPrice
518:      ) private view returns (uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L356-L365

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

/// @audit Missing: '@param reservesData'
558       /**
559        * @notice Supply new collateral for taking out of borrower's another collateral
560        * @param userConfig The user configuration that track the supplied/borrowed assets
561        * @param params The additional parameters needed to execute the liquidation function
562        * @param vars the executeLiquidateERC20() function local vars
563        */
564       function _supplyNewCollateral(
565           mapping(address => DataTypes.ReserveData) storage reservesData,
566           DataTypes.UserConfigurationMap storage userConfig,
567           DataTypes.ExecuteLiquidateParams memory params,
568:          ExecuteLiquidateLocalVars memory vars

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L558-L568

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

/// @audit Missing: '@return'
112        * @param params The additional parameters needed to execute the buyWithCredit function
113        */
114       function _buyWithCredit(
115           mapping(address => DataTypes.ReserveData) storage reservesData,
116           mapping(uint256 => address) storage reservesList,
117           DataTypes.UserConfigurationMap storage userConfig,
118           DataTypes.ExecuteMarketplaceParams memory params
119:      ) internal returns (uint256) {

/// @audit Missing: '@return'
374        * @param vars The marketplace local vars for caching storage values for future reads
375        */
376       function _delegateToPool(
377           DataTypes.ExecuteMarketplaceParams memory params,
378           MarketplaceLocalVars memory vars
379:      ) internal returns (uint256, uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L112-L119

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

/// @audit Missing: '@param user'
/// @audit Missing: '@param ps'
/// @audit Missing: '@param oracle'
155       /**
156        * @notice Returns the user account data across all the reserves
157        * @return totalCollateralBase The total collateral of the user in the base currency used by the price feed
158        * @return totalDebtBase The total debt of the user in the base currency used by the price feed
159        * @return availableBorrowsBase The borrowing power left of the user in the base currency used by the price feed
160        * @return currentLiquidationThreshold The liquidation threshold of the user
161        * @return ltv The loan to value of The user
162        * @return healthFactor The current health factor of the user
163        * @return erc721HealthFactor The current erc721 health factor of the user
164        **/
165       function executeGetUserAccountData(
166           address user,
167           DataTypes.PoolStorage storage ps,
168           address oracle
169       )
170           external
171           view
172           returns (
173               uint256 totalCollateralBase,
174               uint256 totalDebtBase,
175               uint256 availableBorrowsBase,
176               uint256 currentLiquidationThreshold,
177               uint256 ltv,
178               uint256 healthFactor,
179:              uint256 erc721HealthFactor

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L155-L179

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

/// @audit Missing: '@param assetType'
58        /**
59         * @notice Validates a supply action.
60         * @param reserveCache The cached data of the reserve
61         * @param amount The amount to be supplied
62         */
63        function validateSupply(
64            DataTypes.ReserveCache memory reserveCache,
65            uint256 amount,
66:           DataTypes.AssetType assetType

/// @audit Missing: '@param reservesData'
584       /**
585        * @notice Validates the liquidation action.
586        * @param userConfig The user configuration mapping
587        * @param collateralReserve The reserve data of the collateral
588        * @param params Additional parameters needed for the validation
589        */
590       function validateLiquidateERC721(
591           mapping(address => DataTypes.ReserveData) storage reservesData,
592           DataTypes.UserConfigurationMap storage userConfig,
593           DataTypes.ReserveData storage collateralReserve,
594:          DataTypes.ValidateLiquidateERC721Params memory params

/// @audit Missing: '@return'
700        * @param oracle The price oracle
701        */
702       function validateHealthFactor(
703           mapping(address => DataTypes.ReserveData) storage reservesData,
704           mapping(uint256 => address) storage reservesList,
705           DataTypes.UserConfigurationMap memory userConfig,
706           address user,
707           uint256 reservesCount,
708           address oracle
709:      ) internal view returns (uint256, bool) {

/// @audit Missing: '@param reservesData'
/// @audit Missing: '@param asset'
/// @audit Missing: '@param tokenId'
940       /**
941        * @notice Validates a transfer action.
942        * @param reserve The reserve object
943        */
944       function validateTransferERC721(
945           mapping(address => DataTypes.ReserveData) storage reservesData,
946           DataTypes.ReserveData storage reserve,
947           address asset,
948:          uint256 tokenId

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L58-L66

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit Missing: '@param atomic_pricing'
77        /**
78         * @dev Constructor.
79         * @param pool The reference to the main Pool contract
80         * @param name_ The name of the token
81         * @param symbol_ The symbol of the token
82         */
83        constructor(
84            IPool pool,
85            string memory name_,
86            string memory symbol_,
87:           bool atomic_pricing

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L77-L87

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit Missing: '@return'
202        * @param _apeCoinStaking ApeCoinStaking contract address
203        */
204       function getUserTotalStakingAmount(
205           mapping(address => UserState) storage userState,
206           mapping(address => mapping(uint256 => uint256)) storage ownedTokens,
207           address user,
208           uint256 poolId,
209           ApeCoinStaking _apeCoinStaking
210:      ) external view returns (uint256) {

/// @audit Missing: '@return'
229        * @param tokenId specified the tokenId for the position
230        */
231       function getTokenIdStakingAmount(
232           uint256 poolId,
233           ApeCoinStaking _apeCoinStaking,
234           uint256 tokenId
235:      ) public view returns (uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L202-L210

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit Missing: '@param erc721Data'
/// @audit Missing: '@param length'
478       /**
479        * @dev Private function to add a token to this extension's ownership-tracking data structures.
480        * @param to address representing the new owner of the given token ID
481        * @param tokenId uint256 ID of the token to be added to the tokens list of the given address
482        */
483       function _addTokenToOwnerEnumeration(
484           MintableERC721Data storage erc721Data,
485           address to,
486           uint256 tokenId,
487:          uint256 length

/// @audit Missing: '@param erc721Data'
/// @audit Missing: '@param length'
493       /**
494        * @dev Private function to add a token to this extension's token tracking data structures.
495        * @param tokenId uint256 ID of the token to be added to the tokens list
496        */
497       function _addTokenToAllTokensEnumeration(
498           MintableERC721Data storage erc721Data,
499           uint256 tokenId,
500:          uint256 length

/// @audit Missing: '@param erc721Data'
/// @audit Missing: '@param userBalance'
506       /**
507        * @dev Private function to remove a token from this extension's ownership-tracking data structures. Note that
508        * while the token is not assigned a new owner, the `_ownedTokensIndex` mapping is _not_ updated: this allows for
509        * gas optimizations e.g. when performing a transfer operation (avoiding double writes).
510        * This has O(1) time complexity, but alters the order of the _ownedTokens array.
511        * @param from address representing the previous owner of the given token ID
512        * @param tokenId uint256 ID of the token to be removed from the tokens list of the given address
513        */
514       function _removeTokenFromOwnerEnumeration(
515           MintableERC721Data storage erc721Data,
516           address from,
517           uint256 tokenId,
518:          uint256 userBalance

/// @audit Missing: '@param erc721Data'
/// @audit Missing: '@param length'
539       /**
540        * @dev Private function to remove a token from this extension's token tracking data structures.
541        * This has O(1) time complexity, but alters the order of the _allTokens array.
542        * @param tokenId uint256 ID of the token to be removed from the tokens list
543        */
544       function _removeTokenFromAllTokensEnumeration(
545           MintableERC721Data storage erc721Data,
546           uint256 tokenId,
547:          uint256 length

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L478-L487

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit Missing: '@param apeCoinStaking'
28        /**
29         * @dev Constructor.
30         * @param pool The address of the Pool contract
31         */
32:       constructor(IPool pool, address apeCoinStaking) NToken(pool, false) {

/// @audit Missing: '@return'
180        * @param user user address
181        */
182       function getUserApeStakingAmount(address user)
183           external
184           view
185:          returns (uint256)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L28-L32

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

/// @audit Missing: '@param _apeRecipient'
92        /**
93         * @notice Withdraw staked ApeCoin from the Pair pool.  If withdraw is total staked amount, performs an automatic claim.
94         * @param _nftPairs Array of Paired BAYC NFT's with staked amounts
95         * @dev if pairs have split ownership and BAKC is attempting a withdraw, the withdraw must be for the total staked amount
96         */
97        function withdrawBAKC(
98            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99            address _apeRecipient
100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L92-L100

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

/// @audit Missing: '@param _apeRecipient'
92        /**
93         * @notice Withdraw staked ApeCoin from the Pair pool.  If withdraw is total staked amount, performs an automatic claim.
94         * @param _nftPairs Array of Paired MAYC NFT's with staked amounts
95         * @dev if pairs have split ownership and BAKC is attempting a withdraw, the withdraw must be for the total staked amount
96         */
97        function withdrawBAKC(
98            ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
99            address _apeRecipient
100:      ) external onlyPool nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L92-L100

```solidity
File: paraspace-core/contracts/protocol/tokenization/NToken.sol

/// @audit Missing: '@param atomic_pricing'
39        /**
40         * @dev Constructor.
41         * @param pool The address of the Pool contract
42         */
43        constructor(IPool pool, bool atomic_pricing)
44            MintableIncentivizedERC721(
45                pool,
46                "NTOKEN_IMPL",
47                "NTOKEN_IMPL",
48                atomic_pricing
49:           )

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L39-L49

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

/// @audit Missing: '@param user'
41        /**
42         * @notice A function that decreases the current liquidity.
43         * @param tokenId The id of the erc721 token
44         * @param liquidityDecrease The amount of liquidity to remove of LP
45         * @param amount0Min The minimum amount to remove of token0
46         * @param amount1Min The minimum amount to remove of token1
47         * @param receiveEthAsWeth If convert weth to ETH
48         * @return amount0 The amount received back in token0
49         * @return amount1 The amount returned back in token1
50         */
51        function _decreaseLiquidity(
52            address user,
53            uint256 tokenId,
54            uint128 liquidityDecrease,
55            uint256 amount0Min,
56            uint256 amount1Min,
57            bool receiveEthAsWeth
58:       ) internal returns (uint256 amount0, uint256 amount1) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L41-L58

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

/// @audit Missing: '@param punkIndexes'
119       /**
120        * @notice Implements the acceptBidWithCredit feature. AcceptBidWithCredit allows users to
121        * accept a leveraged bid on ParaSpace NFT marketplace. Users can submit leveraged bid and pay
122        * at most (1 - LTV) * $NFT
123        * @dev The nft receiver just needs to do the downpayment
124        * @param marketplaceId The marketplace identifier
125        * @param payload The encoded parameters to be passed to marketplace contract (selector eliminated)
126        * @param credit The credit that user would like to use for this purchase
127        * @param referralCode The referral code used
128        */
129       function acceptBidWithCredit(
130           bytes32 marketplaceId,
131           bytes calldata payload,
132           DataTypes.Credit calldata credit,
133           uint256[] calldata punkIndexes,
134           uint16 referralCode
135:      ) external nonReentrant {

/// @audit Missing: '@param punkIndexes'
157       /**
158        * @notice Implements the batchAcceptBidWithCredit feature. AcceptBidWithCredit allows users to
159        * accept a leveraged bid on ParaSpace NFT marketplace. Users can submit leveraged bid and pay
160        * at most (1 - LTV) * $NFT
161        * @dev The nft receiver just needs to do the downpayment
162        * @param marketplaceIds The marketplace identifiers
163        * @param payloads The encoded parameters to be passed to marketplace contract (selector eliminated)
164        * @param credits The credits that the makers have approved to use for this purchase
165        * @param referralCode The referral code used
166        */
167       function batchAcceptBidWithCredit(
168           bytes32[] calldata marketplaceIds,
169           bytes[] calldata payloads,
170           DataTypes.Credit[] calldata credits,
171           uint256[] calldata punkIndexes,
172           uint16 referralCode
173:      ) external nonReentrant {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L119-L135

### [N&#x2011;24]  Not using the named return variables anywhere in the function is confusing
Consider changing the variable to be an unnamed one

*There are 18 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit price
236       function getPrice(address _asset)
237           external
238           view
239           override
240:          returns (uint256 price)

/// @audit timestamp
252       function getLastUpdateTime(address _asset)
253           external
254           view
255           override
256:          returns (uint256 timestamp)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L236-L240

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

/// @audit totalCollateralBase
/// @audit totalDebtBase
/// @audit availableBorrowsBase
/// @audit currentLiquidationThreshold
/// @audit ltv
/// @audit healthFactor
/// @audit erc721HealthFactor
226       function getUserAccountData(address user)
227           external
228           view
229           virtual
230           override
231           returns (
232               uint256 totalCollateralBase,
233               uint256 totalDebtBase,
234               uint256 availableBorrowsBase,
235               uint256 currentLiquidationThreshold,
236               uint256 ltv,
237               uint256 healthFactor,
238:              uint256 erc721HealthFactor

/// @audit ltv
/// @audit lt
251       function getAssetLtvAndLT(address asset, uint256 tokenId)
252           external
253           view
254           virtual
255           override
256:          returns (uint256 ltv, uint256 lt)

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L226-L238

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

/// @audit oldCollateralizedBalance
/// @audit newCollateralizedBalance
364       function _mintMultiple(
365           address to,
366           DataTypes.ERC721SupplyParams[] calldata tokenData
367       )
368           internal
369           virtual
370           returns (
371               uint64 oldCollateralizedBalance,
372:              uint64 newCollateralizedBalance

/// @audit oldCollateralizedBalance
/// @audit newCollateralizedBalance
384       function _burnMultiple(address user, uint256[] calldata tokenIds)
385           internal
386           virtual
387           returns (
388               uint64 oldCollateralizedBalance,
389:              uint64 newCollateralizedBalance

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L364-L372

```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

/// @audit nesting
/// @audit current
/// @audit total
111       function nestingPeriod(uint256 tokenId)
112           external
113           view
114           returns (
115               bool nesting,
116               uint256 current,
117:              uint256 total

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L111-L117

### [N&#x2011;25]  Duplicated `require()`/`revert()` checks should be refactored to a modifier or function
The compiler will inline the function, which will avoid `JUMP` instructions usually associated with functions

*There are 32 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

102:          revert(Errors.CALL_MARKETPLACE_FAILED);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L102

```solidity
File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

84:           require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

87            require(
88                orderInfo.consideration.length > 0,
89                Errors.INVALID_MARKETPLACE_ORDER
90:           );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84

```solidity
File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

110:          revert(Errors.CALL_MARKETPLACE_FAILED);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol#L110

```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

169:          revert(Errors.ORACLE_PRICE_NOT_READY);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L169

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

279           require(
280               marketplaceIds.length == payloads.length &&
281                   payloads.length == credits.length,
282               Errors.INCONSISTENT_PARAMS_LENGTH
283:          );

294               require(
295                   vars.orderInfo.taker == onBehalfOf,
296                   Errors.INVALID_ORDER_TAKER
297:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279-L283

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

160:          require(amount != 0, Errors.INVALID_AMOUNT);

168           require(
169               xToken.getXTokenType() != XTokenType.PTokenSApe,
170               Errors.SAPE_NOT_ALLOWED
171:          );

185:          require(isActive, Errors.RESERVE_INACTIVE);

186:          require(!isPaused, Errors.RESERVE_PAUSED);

434           require(
435               reserveAssetType == DataTypes.AssetType.ERC20,
436               Errors.INVALID_ASSET_TYPE
437:          );

456           require(
457               reserveAssetType == DataTypes.AssetType.ERC721,
458               Errors.INVALID_ASSET_TYPE
459:          );

1059          require(
1060              assetType == DataTypes.AssetType.ERC20,
1061              Errors.INVALID_ASSET_TYPE
1062:         );

636           require(
637               vars.collateralReserveActive && vars.principalReserveActive,
638               Errors.RESERVE_INACTIVE
639:          );

640           require(
641               !vars.collateralReservePaused && !vars.principalReservePaused,
642               Errors.RESERVE_PAUSED
643:          );

645           require(
646               params.priceOracleSentinel == address(0) ||
647                   params.healthFactor <
648                   MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD ||
649                   IPriceOracleSentinel(params.priceOracleSentinel)
650                       .isLiquidationAllowed(),
651               Errors.PRICE_ORACLE_SENTINEL_CHECK_FAILED
652:          );

686           require(
687               vars.isCollateralEnabled,
688               Errors.COLLATERAL_CANNOT_BE_AUCTIONED_OR_LIQUIDATED
689:          );

757           require(
758               vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,
759               Errors.INVALID_ASSET_TYPE
760:          );

826           require(
827               IAuctionableERC721(params.xTokenAddress).isAuctioned(
828                   params.tokenId
829               ),
830               Errors.AUCTION_NOT_STARTED
831:          );

819           require(
820               IERC721(params.xTokenAddress).ownerOf(params.tokenId) ==
821                   params.user,
822               Errors.NOT_THE_OWNER
823:          );

824:          require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

825:          require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

950:          require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

1074:         require(!params.marketplace.paused, Errors.MARKETPLACE_PAUSED);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L160

```solidity
File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

112           require(
113               getUserHf(msg.sender) >
114                   DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
115               Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
116:          );

200               require(
201                   INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
202                       msg.sender,
203                   Errors.NOT_THE_OWNER
204:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L112-L116

```solidity
File: paraspace-core/contracts/protocol/pool/PoolCore.sol

726           require(
727               msg.sender == ps._reserves[asset].xTokenAddress,
728               Errors.CALLER_NOT_XTOKEN
729:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L726-L729

```solidity
File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

172:          require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

173           require(
174               ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
175               Errors.ASSET_NOT_LISTED
176:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172

```solidity
File: paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

296           require(
297               _isApprovedOrOwner(_msgSender(), tokenId),
298               "ERC721: transfer caller is not owner nor approved"
299:          );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L296-L299

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

167               require(
168                   !isAuctioned(erc721Data, POOL, tokenId),
169                   Errors.TOKEN_IN_AUCTION
170:              );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L167-L170

### [N&#x2011;26]  Consider using `delete` rather than assigning zero to clear values
The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

589:              vars.ethLeft = 0;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L589

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

123:                  reserve.accruedToTreasury = 0;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L123

### [N&#x2011;27]  Contracts should have full test coverage
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

*There is 1 instance of this issue:*
```
- What is the overall line coverage percentage provided by your tests?: 85
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/README.md?plain=1#L98

### [N&#x2011;28]  Large or complicated code bases should implement fuzzing tests
Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement [fuzzing tests](https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05). Fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.

*There is 1 instance of this issue:*
```
- How many contracts are in scope?: 32
- Total SLoC for these contracts?: 8408
- How many external imports are there?: 12
- How many separate interfaces and struct definitions are there for the contracts within scope?: 44 interfaces, 35 struct definitions
- Does most of your code generally use composition or inheritance?: Yes
- How many external calls?: 306
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/README.md?plain=1#L92-L97

### [N&#x2011;29]  Typos

*There are 3 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit aggeregate
412:          //aggeregate with price from all feeders

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol

/// @audit tsatr
34:        * @notice Function to tsatr auction on an ERC721 of a position if its ERC721 Health Factor drops below 1.

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

/// @audit computated
213:       * due selfdestructs or transfer punk to pre-computated contract address before deployment.

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L213

___

## Excluded findings
These findings are excluded from awards calculations because there are publicly-available automated tools that find them. The valid ones appear here for completeness

## Summary

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | `safeApprove()` is deprecated | 1 |
| [L&#x2011;02] | Missing checks for `address(0x0)` when assigning values to `address` state variables | 4 |

Total: 5 instances over 2 issues

### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | Return values of `approve()` not checked | 2 |
| [N&#x2011;02] | `public` functions not called by the contract should be declared `external` instead | 3 |
| [N&#x2011;03] | `constant`s should be defined rather than using magic numbers | 2 |
| [N&#x2011;04] | Event is missing `indexed` fields | 8 |

Total: 15 instances over 4 issues



## Low Risk Issues

### [L&#x2011;01]  `safeApprove()` is deprecated
[Deprecated](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/bfff03c0d2a59bcd8e2ead1da9aed9edf0080d05/contracts/token/ERC20/utils/SafeERC20.sol#L38-L45) in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead. The function may currently work, but if a bug is found in this version of OpenZeppelin, and the version that you're forced to upgrade to no longer has this function, you'll encounter unnecessary delays in porting and testing replacement contracts.

*There is 1 instance of this issue:*
```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

/// @audit (valid but excluded finding)
555:              IERC20(token).safeApprove(operator, type(uint256).max);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L555

### [L&#x2011;02]  Missing checks for `address(0x0)` when assigning values to `address` state variables

*There are 4 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

/// @audit (valid but excluded finding)
58:           BASE_CURRENCY = baseCurrency;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L58

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

/// @audit (valid but excluded finding)
48:           punk = _punk;

/// @audit (valid but excluded finding)
49:           wpunk = _wpunk;

/// @audit (valid but excluded finding)
50:           pool = _pool;

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L48

## Non-critical Issues

### [N&#x2011;01]  Return values of `approve()` not checked
Not all `IERC20` implementations `revert()` when there's a failure in `approve()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

/// @audit (valid but excluded finding)
45:           _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);

/// @audit (valid but excluded finding)
46:           _apeCoin.approve(address(POOL), type(uint256).max);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L45

### [N&#x2011;02]  `public` functions not called by the contract should be declared `external` instead
Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents' functions and change the visibility from `external` to `public`.

*There are 3 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
97        function initialize(
98            address _admin,
99            address[] memory _feeders,
100           address[] memory _assets
101:      ) public initializer {

/// @audit (valid but excluded finding)
261:      function getFeederSize() public view returns (uint256) {

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L97-L101

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol

/// @audit (valid but excluded finding)
52        function executeBorrow(
53            mapping(address => DataTypes.ReserveData) storage reservesData,
54            mapping(uint256 => address) storage reservesList,
55            DataTypes.UserConfigurationMap storage userConfig,
56:           DataTypes.ExecuteBorrowParams memory params

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L52-L56

### [N&#x2011;03]  `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 2 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

/// @audit 18 - (valid but excluded finding)
255:                              (18 +

/// @audit 18 - (valid but excluded finding)
267:                              (18 +

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L255

### [N&#x2011;04]  Event is missing `indexed` fields
Index event fields make the field more quickly accessible [to off-chain tools](https://ethereum.stackexchange.com/questions/40396/can-somebody-please-explain-the-concept-of-event-indexing) that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*There are 8 instances of this issue:*
```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

/// @audit (valid but excluded finding)
58:       event AssetPaused(address indexed asset, bool paused);

/// @audit (valid but excluded finding)
63:       event OracleConfigSet(uint128 expirationPeriod, uint128 maxPriceDeviation);

/// @audit (valid but excluded finding)
64        event AssetDataSet(
65            address indexed asset,
66            uint256 twap,
67            uint256 lastUpdatedBlock
68:       );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L58

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

/// @audit (valid but excluded finding)
38        event BuyWithCredit(
39            bytes32 indexed marketplaceId,
40            DataTypes.OrderInfo orderInfo,
41            DataTypes.Credit credit
42:       );

/// @audit (valid but excluded finding)
44        event AcceptBidWithCredit(
45            bytes32 indexed marketplaceId,
46            DataTypes.OrderInfo orderInfo,
47            DataTypes.Credit credit
48:       );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L38-L42

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

/// @audit (valid but excluded finding)
30:       event MintedToTreasury(address indexed reserve, uint256 amountMinted);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L30

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

/// @audit (valid but excluded finding)
29:       event UnstakeApeIncentiveUpdated(uint256 oldValue, uint256 newValue);

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L29

```solidity
File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

/// @audit (valid but excluded finding)
74        event ApprovalForAll(
75            address indexed owner,
76            address indexed operator,
77            bool approved
78:       );

```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L74-L78







