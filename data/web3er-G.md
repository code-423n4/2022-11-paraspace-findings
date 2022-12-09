# Issue1-Redundant modifier onlyWhenFeederExisted Usage
## Affected Code
contracts/misc/NFTFloorOracle.sol 
```
/// @notice Allows owner to remove feeder.
/// @param _feeder feeder to remove
function removeFeeder(address _feeder)
    external
    onlyWhenFeederExisted(_feeder)
{
    _removeFeeder(_feeder);
}

function _removeFeeder(address _feeder)
    internal
    onlyWhenFeederExisted(_feeder)
{
    uint8 feederIndex = feederPositionMap[_feeder].index;
    if (feederIndex >= 0 && feeders[feederIndex] == _feeder) {
        feeders[feederIndex] = feeders[feeders.length - 1];
        feeders.pop();
    }
    delete feederPositionMap[_feeder];
    revokeRole(UPDATER_ROLE, _feeder);
    emit FeederRemoved(_feeder);
}
```
## Description
There is no need to call the `onlyWhenFeederNotExisted` modifier twice to judge whether the feeder exists which will waste gas.
## Advice
It is recommended to remove the modifier `onlyWhenFeederExisted` of function `_removeFeeder`.
# Issue2-No Need to Explicitly Write the Return Statement
## Affected Code
contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
```
function executeMintMultiple(
    MintableERC721Data storage erc721Data,
    bool ATOMIC_PRICING,
    address to,
    DataTypes.ERC721SupplyParams[] calldata tokenData
)
    external
    returns (
        uint64 oldCollateralizedBalance,
        uint64 newCollateralizedBalance
    )
{
    require(to != address(0), "ERC721: mint to the zero address");
    uint64 oldBalance = erc721Data.userState[to].balance;
    oldCollateralizedBalance = erc721Data
        .userState[to]
        .collateralizedBalance;
    uint256 oldTotalSupply = erc721Data.allTokens.length;
    uint64 collateralizedTokens = 0;

    ......

    newCollateralizedBalance =
        oldCollateralizedBalance +
        collateralizedTokens;
    erc721Data
        .userState[to]
        .collateralizedBalance = newCollateralizedBalance;

    ......

    return (oldCollateralizedBalance, newCollateralizedBalance);
}
```
```
function executeBurnMultiple(
    MintableERC721Data storage erc721Data,
    IPool POOL,
    address user,
    uint256[] calldata tokenIds
)
    external
    returns (
        uint64 oldCollateralizedBalance,
        uint64 newCollateralizedBalance
    )
{
    uint64 burntCollateralizedTokens = 0;
    uint64 balanceToBurn;
    uint256 oldTotalSupply = erc721Data.allTokens.length;
    uint256 oldBalance = erc721Data.userState[user].balance;
    oldCollateralizedBalance = erc721Data
        .userState[user]
        .collateralizedBalance;

    ......

    erc721Data.userState[user].balance -= balanceToBurn;
    newCollateralizedBalance =
        oldCollateralizedBalance -
        burntCollateralizedTokens;
    erc721Data
        .userState[user]
        .collateralizedBalance = newCollateralizedBalance;

    ......

    return (oldCollateralizedBalance, newCollateralizedBalance);
}
```
## Description
In function `executeMintMultiple` and function `executeBurnMultiple`, the returned variable is specified in the function signature, but the return statement is still explicitly displayed, which will increase gas consumption.
## Advice
It is recommended not to explicitly write the return statement when the returned variable is specified in the function signature.
# Issue3-Not Used Variable amountToWithdraw
## Affected Code
contracts/protocol/pool/PoolApeStaking.sol
```
function withdrawApeCoin(
    address nftAsset,
    ApeCoinStaking.SingleNft[] calldata _nfts
) external nonReentrant {
    DataTypes.PoolStorage storage ps = poolStorage();
    checkSApeIsNotPaused(ps);

    DataTypes.ReserveData storage nftReserve = ps._reserves[nftAsset];
    address xTokenAddress = nftReserve.xTokenAddress;
    INToken nToken = INToken(xTokenAddress);

    uint256 amountToWithdraw = 0;
    for (uint256 index = 0; index < _nfts.length; index++) {
        require(
            nToken.ownerOf(_nfts[index].tokenId) == msg.sender,
            Errors.NOT_THE_OWNER
        );
        amountToWithdraw += _nfts[index].amount;
    }

    INTokenApeStaking(nftReserve.xTokenAddress).withdrawApeCoin(
        _nfts,
        msg.sender
    );

    require(
        getUserHf(msg.sender) >
            DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,
        Errors.HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD
    );
}
```
## Description
In function `withdrawApeCoin`, the variable `amountToWithdraw` is initialized and updated, but never used then.
## Advice
It is recommended to remove the variable `amountToWithdraw` to save gas.