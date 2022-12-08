# 1. MintableIncentivizedERC721 safeTransferFrom() does not call IERC721Receiver.onERC721Received()
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L320-L327
MintableIncentivizedERC721 claims to implement IERC721 and the documentation for _safeTransfer() states "Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients are aware of the ERC721 protocol to prevent tokens from being forever locked." However the implementation of safeTransfer() is the same as that for transfer() and no call to IERC721Receiver.onERC721Received() is performed.

# 2. NFTFloorOracle assets array can contain zeroes
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/NFTFloorOracle.sol#L301
NFTFloorOracle _removeAsset() just zeroes items in the assets array, so any clients reading this array may see an array containing som zero addrss values. To avoid these zeroes it would be necessary to shift array elements when an asset is removed. In addition, the assets array does not seem to be used directly by the implementation so potetially it could be removed.

# 3. NFTFloorOracle removeFeeder() does not explicitly restrict role
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/NFTFloorOracle.sol#L167
Most of the feeder and asset management functions of NFTFloorOracle use onlyRole(DEFAULT_ADMIN_ROLE) to restrict access, but removeFeeder() does not. It turns out that removeFeeder() calls _removeFeeder() which calls revokeRole() that ends up performing the appropriate access check. However, to make it clear that access control is enforced for removeFeeder() it is recommended to use the onlyRole(DEFAULT_ADMIN_ROLE) modifier directly or else add a comment describing how access control is being enforced.

# 4. NFTFloorOracle _addFeeder() calls _setupRole()
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/NFTFloorOracle.sol#L314
OpenZeppelin documentation for AccessControl states "This function should only be called from the constructor when setting up the initial roles for the system." Instead, _grantRole() or grantRole() could be used.

# 5. NFTFloorOracle _removeFeeder() condition `feederIndex >= 0 && feeders[feederIndex] == _feeder` should be changed
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/NFTFloorOracle.sol#L331
I separately reported a more serious issue with corruption of the feeders list from _removeFeeder(). But with regard to the if condition `feederIndex >= 0 && feeders[feederIndex] == _feeder` itself, the condition does not really make sense. `feederIndex >= 0` is always true because `feederIndex` is a `uint8`. And `feeders[feederIndex] == _feeder` is only false if the feeder storage is already corrupted. If desired this data consistency check could be placed in a require somewhere rather than used as a guard here.

# 6. NFTFloorOracle maxPriceDeviation is not time based
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/NFTFloorOracle.sol#L370
The maxPriceDeviation check in `_checkValidity()` prevents a feeder from submitting a price that has changed by too large or small of a ratio. However, if the intention is to prevent a malicious feeder from supplying excessively high or low values it dos not work because the feeder can simply call setPrice() repeatedly with increasingly larger/smaller values until reaching the desired number. To avoid this, a block number or time based constraint could be placed on the amount of price change to cap the rate of change.

# 7. NFTFloorOracle _checkValidity() inconsistent comment
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/NFTFloorOracle.sol#L369
The comment in this function states "config maxPriceDeviation as multiple directly(not percent) for simplicity". However, maxPriceDeviation appears to be expressed as a percent. It may make sense to reword the comment.

# 8. SeaportAdapter getBidOrderInfo() comment inconsistent with require() validation
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L72
The comment states "NOT criteria based and must be basic order" however there is no check on the CriteriaResolver as in getAskOrderInfo(), which by contrast does check absence of CriteriaResolvers.

# 9. PoolAddressesProvider setAddressAsProxy() enhance id validation
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L86
The require() in this function ensures id is not POOL to prevent inadvertently setting up POOL as a standard proxy since it instead uses the ParaProxy implementation. It may make sense to include other ids known not to be proxies (and for which setAddress() should be called instead of setAddressAsProxy()), for example id WETH would not be configured as a proxy here. This would prevent additional ids from being incorrectly set up as proxies.

# 10. MintableIncentivizedERC721 onlyPoolAdmin() should check _msgSender()
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L50
onlyPoolAdmin() should check _msgSender() instead of msg.sender for use with meta-transactions and consistency with the rest of the file, where _msgSender() is used.

# 11. MintableIncentivizedERC721 "@dev UserState" comment describes struct in another file.
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L64-L69
The "@dev UserState" discussion could be relocated to MintableERC721Logic where the UserState struct is located.

# 12. In MintableERC721Logic.sol, MintableERC721Data's allowances map is unused
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L40
MintableERC721Data supports tokenApprovals and operatorApprovals, but the allowances map seems to be unused.

# 13. MintableERC721Logic executeMintMultiple() !erc721Data.isUsedAsCollateral[tokenId] is unnecessary
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L231
It should not be necessary to check !erc721Data.isUsedAsCollateral[tokenId] since tokenId has just been minted.

# 14. MintableERC721Logic _beforeTokenTransfer() handling of zero cases unnecessary
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L457
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L469
The check `to == address(0)` is unnecessary because `_beforeTokenTransfer()` is only called from `executeTransfer()`, where `to` cannot be zero. It seems unlikely that `from` would be set to zero, but that is not actually enforced. Since minting is implemented in a separate function (executeMintMultiple()) it may make sense to require `from != address(0)` and also avoid the `from == address(0)` check in `_beforeTokenTransfer()`.