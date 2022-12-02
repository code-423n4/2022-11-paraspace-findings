 ### Gas Optimization 1

file `protocol/pool/PoolApeStaking.sol`,  function `withdrawBAKC`:

Lets look at the for-loop starting on 138.
Here we always have an initial `require` followed by a bunch of logic. If e.g. the 10th `_nftPair` fails this `require`, then we did all of the logic for the previous 9 `_nftPairs` in vein.
So to save gas, we should initially loop over all `_nftPairs` and do only the `require` checks, and afterwards again start the `for`-loop with the corresponding logic.

```
for (uint256 index = 0; index < _nftPairs.length; index++) {
require(
    INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
        msg.sender,
    Errors.NOT_THE_OWNER
);
}

for (uint256 index = 0; index < _nftPairs.length; index++) {
(uint256 stakedAmount, ) = nTokenApeStaking
    .getApeStaking()
    .nftPosition(
        ApeStakingLogic.BAKC_POOL_ID,
        _nftPairs[index].bakcTokenId
    );

if (_nftPairs[index].amount == 0) {
    _nftPairs[index].amount = stakedAmount;
}
//only partially withdraw need user's BAKC
if (_nftPairs[index].amount != stakedAmount) {
    bakcContract.safeTransferFrom(
        msg.sender,
        xTokenAddress,
        _nftPairs[index].bakcTokenId
    );
    transferredTokenIds[actualTransferAmount] = _nftPairs[index]
        .bakcTokenId;
    actualTransferAmount++;
}
amountToWithdraw += _nftPairs[index].amount;
}
```