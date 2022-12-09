# 1. Unused variables

Code Lines: 
- https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L71
- https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L135

The local variable `amountToWithdraw` only be summed in the loop but is not used anywhere.
```
        uint256 amountToWithdraw = 0;
        for (uint256 index = 0; index < _nfts.length; index++) {
            ...
            amountToWithdraw += _nfts[index].amount;
        }
```

# 2. NTokenMoonBirds.nestingOpen should be a external view function

Code Lines:

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L127-L129

NTokenMoonBirds.nestingOpen function just reads the nestingOpen status of the external IMoonBird contracts. It should be a view function. 