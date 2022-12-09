## Summary
### Low Risk
|      | Issue                                                                              |
|------|------------------------------------------------------------------------------------|
| L-01 | Add constructor initializers                                                       |
| L-02 | upgradeable contracts should use the upgradeable variant of OpenZeppelin Contracts |
| L-03 | `MintableIncentivizedERC721` does not implement `ERC721.safeTransferFrom` properly |
| L-04 | `PoolParameters.rescueToken` does not rescue `ERC1155` tokens                      |
| L-05 | Vulnerable dependency of OpenZeppelin                                              |

### Non-critical
|      | Issue                     |
|------|---------------------------|
| N-01 | Typos                     |
| N-02 | Misleading parameter name |
| N-03 | Open TODOs                |

## Low
### [L‑01] Add constructor initializers

As per OpenZeppelin’s (OZ) recommendation, “The guidelines are now to make it impossible for anyone to run initialize on an implementation contract, by adding an **empty constructor with the initializer modifier**. So the implementation contract gets initialized automatically upon deployment.”


https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L43-L55
```solidity
43:     constructor(
44:         address _punk,
45:         address _wpunk,
46:         address _pool
47:     ) { //@audit - missing initializer
48:         punk = _punk;
49:         wpunk = _wpunk;
50:         pool = _pool;
51: 
52:         Punk = IPunks(punk);
53:         WPunk = IWrappedPunks(wpunk);
54:         Pool = IPool(pool);
55:     }
```

### [L‑02] upgradeable contracts should use the upgradeable variant of OpenZeppelin Contracts


Contracts deployed as a proxied contract should use the Upgradeable variants of OpenZeppelin Contracts. Some are using the non-upgradeable versions.

Otherwise, the constructor functions of the parent contracts, which may change storage at deploy time, won't work for clone instances.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L55
```solidity
//@audit - Initializable and AccessControl should be upgradeable versions
55: contract NFTFloorOracle is Initializable, AccessControl, INFTFloorOracle {
```

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol
```solidity
19: contract WPunkGateway is
20:     ReentrancyGuard, //@audit - should be upgradeable version
21:     IWPunkGateway,
22:     IERC721Receiver,
23:     OwnableUpgradeable
```


### [L‑03] `MintableIncentivizedERC721` does not implement `ERC721.safeTransferFrom` properly

`MintableIncentivizedERC721` is described as:

```solidity
27:  * @notice Basic ERC721 implementation
```

which will be used as a parent contract for `NTokens`.

It however does not implement `safeTransferFrom` as per `EIP-721`. This function should:

```
When transfer is complete, this function
///  checks if `_to` is a smart contract (code size > 0). If so, it calls
///  `onERC721Received` on `_to` and throws if the return value is not
///  `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`.
```

But looking at the [implementation](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L326), it simply performs the `_transfer`, without performing these checks:

```solidity
320: function _safeTransfer(
321:         address from,
322:         address to,
323:         uint256 tokenId,
324:         bytes memory
325:     ) internal virtual {
326:         _transfer(from, to, tokenId);
327:     }
```

`NToken`, which inherits `MintableIncentivizedERC721`, does not override this function. Neither do all the NToken implementations.

### Recommendation

Add the required check to `MintableIncentivizedERC721._safeTransfer`.

```diff
320: function _safeTransfer(
321:         address from,
322:         address to,
323:         uint256 tokenId,
324:         bytes memory
325:     ) internal virtual {
326:         _transfer(from, to, tokenId);
+            require(_checkOnERC721Received(from, to, tokenId, _data), "ERC721: transfer to non ERC721Receiver implementer");
327:     }
```

### [L‑04] `PoolParameters.rescueToken` does not rescue `ERC1155` tokens

This function rescues and transfer tokens locked in this contract.

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L196
```solidity
196:     function rescueTokens( //@audit - does not work with ERC1155
197:         DataTypes.AssetType assetType,
198:         address token,
199:         address to,
200:         uint256 amountOrTokenId
201:     ) external virtual override onlyPoolAdmin {
202:         PoolLogic.executeRescueTokens(assetType, token, to, amountOrTokenId);
203:     }
```

Looking at `PoolLogic.executeRescueTokens`, the function does not handle `ERC1155` tokens. This means such tokens inadvertently sent to `PoolParameters` would be locked forever.

```solidity
82:     function executeRescueTokens(
83:         DataTypes.AssetType assetType,
84:         address token,
85:         address to,
86:         uint256 amountOrTokenId
87:     ) external {
88:         if (assetType == DataTypes.AssetType.ERC20) {
89:             IERC20(token).safeTransfer(to, amountOrTokenId);
90:         } else if (assetType == DataTypes.AssetType.ERC721) {
91:             IERC721(token).safeTransferFrom(address(this), to, amountOrTokenId);
92:         }
93:     }
```


### Recommendation

Amend the function so that it handles ERC1155. This will also make the function consistent with the rest of the protocol, as `NToken` has a [ERC1155 rescue function](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L151).
Note that you should also then amend `AssetType` to include `ERC1155`.

```diff
82:     function executeRescueTokens(
83:         DataTypes.AssetType assetType,
84:         address token,
85:         address to,
86:         uint256 amountOrTokenId
+           uint256 amount,
87:     ) external {
88:         if (assetType == DataTypes.AssetType.ERC20) {
89:             IERC20(token).safeTransfer(to, amountOrTokenId);
90:         } else if (assetType == DataTypes.AssetType.ERC721) {
91:             IERC721(token).safeTransferFrom(address(this), to, amountOrTokenId);
+           } else if (assetType == DataTypes.AssetType.ERC1155) {
+               IERC1155(token).safeTransferFrom(address(this), to, amountOrTokenId, amount);
92:         }
93:     }
```

### [L‑05] Vulnerable dependency of OpenZeppelin


As per the package.json file, the project is using the 4.2.0 version of OpenZeppelin.


```json
31:     "@openzeppelin/contracts": "4.2.0",
32:     "@openzeppelin/contracts-upgradeable": "4.2.0",
```

This version is not the latest, and present the following vulnerabilities:

https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-4h98-2769-gh6h
https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-7grf-83vw-6f5x
https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-4g63-c64m-25w9
https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-qh9x-gcfh-pcrw
https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-9c22-pwxw-p6hx
https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-5vp3-v4hc-gx76

### Recommendation

Use the latest non vulnerable [version](https://github.com/OpenZeppelin/openzeppelin-contracts): `4.8.0`

## Non-critical

### [N‑01] Typos

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34
```solidity
@audit - start
34: Function to tsatr auction
```

### [N‑02] Misleading parameter name

`NToken.transferOnLiquidation()` has a `value` parameter.
This actually corresponds to the `tokenId` getting transferred, so the current naming is misleading

https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L122
```solidity
119:     function transferOnLiquidation(
120:         address from,
121:         address to,
122:         uint256 value
123:     ) external onlyPool nonReentrant {
124:         _transfer(from, to, value, false);
125:     }
```

### [N‑03] Open TODOs
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/interfaces/INToken.sol#L97
```solidity
97: // TODO are we using the Treasury at all? Can we remove?
```



