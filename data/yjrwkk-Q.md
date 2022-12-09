## Non-critical

### Duplicate modifiers

- `onlyPool`

    - [MintableIncentivizedERC721.sol#L59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L59)
    - [IncentivizedERC20.sol#L41](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L41)
    - [AirdropFlashClaimReceiver.sol#L39](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol#L39)


- `onlyPoolAdmin`

    - [MintableIncentivizedERC721.sol#L45](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L45)
    - [PoolConfigurator.sol#LL32](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol#LL32)
    - [PriceOracleSentinel.sol#L21](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PriceOracleSentinel.sol#L21)
    - [IncentivizedERC20.sol#L27](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/IncentivizedERC20.sol#L27)


- `onlyAssetListingOrPoolAdmins`

    - [ParaSpaceOracle.sol#L34](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L34)
    - [PoolConfigurator.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolConfigurator.sol#L56)
    - [ERC721OracleWrapper.sol#L18](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ERC721OracleWrapper.sol#L18)

### Inconsistencies in comments

```console
$ cd 2022-11-paraspace/paraspace-core/contracts/
$ grep -P "^\s*// \w" -r . | wc -l
3688
$ grep -P "^\s*//\w" -r . | wc -l
133
```

Comments starting with `//` followed by a space should be more readable.

### Errors in documentation

Docs for:  
- [ParaVersionedInitializable.sol#L5](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaVersionedInitializable.sol#L5)
- [VersionedInitializable.sol#L5](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/VersionedInitializable.sol#L5)

are identical. Specifically, `ParaVersionedInitializable` says it is a `VersionedInitializable` (not in the scope, but used in several places in the scope).