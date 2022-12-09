## Valid Approved users will not be allowed to transfer

Contract:
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L412

Impact:
The `transferFrom` allows an approved user to transfer NFT on behalf of owner. But seems like this feature is broken since one of its internal call requires caller to be owner only which means the call would fail

Steps:
1. Approved User calls the [`transferFrom`](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L253) function

```
function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external virtual override nonReentrant {
        //solhint-disable-next-line max-line-length
        require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );

        _transfer(from, to, tokenId);
    }
```

2. This makes call to `_transfer` function. Since this is an abstract contract which is implemented by `NToken` contract. Now `NToken` has overridden its own `_transfer` function

```
function _transfer(
        address from,
        address to,
        uint256 tokenId,
        bool validate
    ) internal virtual {
        address underlyingAsset = _underlyingAsset;

        uint256 fromBalanceBefore = collateralizedBalanceOf(from);
        uint256 toBalanceBefore = collateralizedBalanceOf(to);
        bool isUsedAsCollateral = _transferCollateralizable(from, to, tokenId);
...
}
```

3. This calls `_transferCollateralizable` which internally calls `executeTransfer` function

```
unction _transferCollateralizable(
        address from,
        address to,
        uint256 tokenId
    ) internal virtual returns (bool isUsedAsCollateral_) {
        isUsedAsCollateral_ = MintableERC721Logic
            .executeTransferCollateralizable(
                _ERC721Data,
                POOL,
                ATOMIC_PRICING,
                from,
                to,
                tokenId
            );
    }

function executeTransferCollateralizable(
        ...
    ) external returns (bool isUsedAsCollateral_) {
        ...

        executeTransfer(erc721Data, POOL, ATOMIC_PRICING, from, to, tokenId);
    }
```

4. The `executeTransfer` function checks whether caller is owner itself. Since caller is an approved user and not owner so this call fails

```
function executeTransfer(
        MintableERC721Data storage erc721Data,
        IPool POOL,
        bool ATOMIC_PRICING,
        address from,
        address to,
        uint256 tokenId
    ) public {
        require(
            erc721Data.owners[tokenId] == from,
            "ERC721: transfer from incorrect owner"
        );
...
}
```

Recommendation:
Either remove the owner requirement or b) if this is meant to be called only by owner then remove the approved user allow

## Centralization Risk

Contract:
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131

Impact:
It seems the `poolAdmin` holds too much power including changing reward controller, rescue tokens etc. This can allow poolAdmin to impact all users by changing the config or draining the contract. In this example we will see one example for setIncentivesController

Steps:
1. PoolAdmin calls `setIncentivesController` and set `rewardController` to zero
2. This causes Users will stop getting incentives on their stakes. So if User decides to burn then the reward incentives are gone permanently

Recommendation:
Keep the `poolAdmin` as multiSig and behind timelock to prevent immediate changes

## Asset is not removed properly

Contract:
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L296

Impact:
Asset are deleted directly leaving blanks in middle of `assets`. This could have unintended effects based on how `assets` is being utlized

Steps:
1. Assume there were 2 assets {A,B}
2. `removeAsset` function is called to remove asset `A`
3. This internally calls `_removeAsset` function

```
function _removeAsset(address _asset)
        internal
        onlyWhenAssetExisted(_asset)
    {
        uint8 assetIndex = assetFeederMap[_asset].index; = assetFeederMap[A].index = 0
        delete assets[assetIndex]; = delete assets[0];
        delete assetPriceMap[_asset];
        delete assetFeederMap[_asset];
        emit AssetRemoved(_asset);
    }
```

4. Since assets[0] is directly deleted and not swapped with last array value so assets would look like below:

```
assets[0] = {}
assets[1] = {B}
```

Recommendation:
Instead of directly deleting the asset, swap deleted asset index with last index asset and then pop last element. Sample example is `_removeFeeder`

## Support for IERC165 interface id is missed

Contract:
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L572

Impact:
Contract fails to support a valid interface which could lead to failure of genuine calls

Steps:

1. Observe the [supportsInterface](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L572)

```
function supportsInterface(bytes4 interfaceId)
        external
        view
        virtual
        override(IERC165)
        returns (bool)
    {
        return
            interfaceId == type(IERC721Enumerable).interfaceId ||
            interfaceId == type(IERC721Metadata).interfaceId;
    }
```

2. Observe that support for IERC165 interface id is missing

Recommendation:
Kindly revise the function as below:

```
function supportsInterface(bytes4 interfaceId)
        external
        view
        virtual
        override(IERC165)
        returns (bool)
    {
        return
            interfaceId == type(IERC721Enumerable).interfaceId ||
            interfaceId == type(IERC721Metadata).interfaceId ||
            interfaceId == type(IERC165).interfaceId;
    }
```