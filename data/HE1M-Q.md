## No.1
In the fallback of `ParaProxy `, it is better to check the length of `msg.data` to be `>= 4 bytes`. For example, if a user sends `0xaabbcc`, the `msg.sig` will be automatically changed to `0xaabbcc00`. And if this selector is available, the corresponding facet will be delegate-called. By having the data length check in fallback function, the callers will be noticed that your contract is used incorrectly.
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol#L41

## No. 2
When an NFT is used as collateral and the borrower's account health factor goes below 1, the NFT will be auctioned. If the borrower pays back the borrowed amount until the health factor goes above `auctionRecoveryHealthFactor` (which is 1.5), the auction will be ended by calling `endAuction`.
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L512

During ending the auction, the mapping `erc721Data.auctions[tokenId]` will be deleted.
```
function executeEndAuction(
        MintableERC721Data storage erc721Data,
        IPool POOL,
        uint256 tokenId
    ) external {
        require(
            isAuctioned(erc721Data, POOL, tokenId),
            Errors.AUCTION_NOT_STARTED
        );
        require(
            _exists(erc721Data, tokenId),
            "ERC721: endAuction for nonexistent token"
        );
        delete erc721Data.auctions[tokenId];
    }
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L386

So, the function `isAuctioned` will return false:
```
function isAuctioned(
        MintableERC721Data storage erc721Data,
        IPool POOL,
        uint256 tokenId
    ) public view returns (bool) {
        return
            erc721Data.auctions[tokenId].startTime >
            POOL
                .getUserConfiguration(erc721Data.owners[tokenId])
                .auctionValidityTime;
    }
```
https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L424

There is another function that can end an auction without deleting the mapping, just by setting the validity time to the current timestamp.
```
function setAuctionValidityTime(address user)
        external
        virtual
        override
        nonReentrant
    {
        DataTypes.PoolStorage storage ps = poolStorage();

        require(user != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
        DataTypes.UserConfigurationMap storage userConfig = ps._usersConfig[
            user
        ];
        (, , , , , , uint256 erc721HealthFactor) = PoolLogic
            .executeGetUserAccountData(
                user,
                ps,
                ADDRESSES_PROVIDER.getPriceOracle()
            );
        require(
            erc721HealthFactor > ps._auctionRecoveryHealthFactor,
            Errors.ERC721_HEALTH_FACTOR_NOT_ABOVE_THRESHOLD
        );
        userConfig.auctionValidityTime = block.timestamp;
    }
```
https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L263

This function says that any auction before the validity time, is not valid anymore. But, it does not delete the mentioned mapping.

So, there will be a situation that before calling the function `endAuction`, someone calls the function `setAuctionValidityTime`. So, the first function call `endAuction` will be reverted. So, the auction start time will be smaller than validity time (results in returning false when the function `isAuctioned` is called), but the mapping is not deleted. 

It is better to delete the mapping `erc721Data.auctions[tokenId]` in the function `setAuctionValidityTime` if its start time is `>0`.