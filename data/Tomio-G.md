1.
Title: Expression for `constant` values such as a call to `keccak256()`, should use `immutable` rather than `constant`

Proof of Concept:
[NFTFloorOracle.sol#L70](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L70)
[PoolStorage.sol#L16](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16)
[NTokenApeStaking.sol#L23NTokenApeStaking.sol#L23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23)

Recommended Mitigation Steps:
Change from `constant` to `immutable`
reference: [here](https://github.com/ethereum/solidity/issues/9232)
________________________________________________________________________

2.
Title: Reduce the size of error messages (Long revert Strings)

Impact:
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

Proof of Concept:
[NFTFloorOracle.sol#L356](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L356)
[MintableIncentivizedERC721.sol#L197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L197)
[MintableIncentivizedERC721.sol#L261](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L261)
[MintableIncentivizedERC721.sol#L298](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L298)
[MintableIncentivizedERC721.sol#L356](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L356)
[MintableIncentivizedERC721.sol#L620](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L620)

Recommended Mitigation Steps:
Consider shortening the revert strings to fit in 32 bytes
________________________________________________________________________

3.
Title: Gas improvement on returning value

Proof of Concept:
[ParaSpaceOracle.sol#L125](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L125)

Recommended Mitigation Steps:
by set `price` in returns L#119 and delete L#125 can save gas

```
function getAssetPrice(address asset)
        public
        view
        override
        returns (uint256 price) //@audit-info: set here
    { 

```
________________________________________________________________________

4.
Title: Using multiple `require` instead `&&` can save gas

Proof of Concept:
[PoolParameters.sol#L216-L220](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-L220)

Recommended Mitigation Steps:
Change to:

```
	require(value > MIN_AUCTION_HEALTH_FACTOR, Errors.INVALID_AMOUNT );
	require(value <= MAX_AUCTION_HEALTH_FACTOR, Errors.INVALID_AMOUNT );
```
________________________________________________________________________

5.
Title: Comparison operators

Proof of Concept:
[NToken.sol#L176](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L176)

Recommended Mitigation Steps:
Change from `>= 4` to `> 3` can save gas

```
	require(airdropParams.length > 3, Errors.INVALID_AIRDROP_PARAMETERS);
```
________________________________________________________________________

6.
Title: Consider delete empty block or emit something

impact:
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

Proof of Concept:
[NTokenUniswapV3.sol#L149](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L149)
________________________________________________________________________

7. Struct optimization

Proof of Concept:
[ApeStakingLogic.sol#L26-L28](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L26-L28)

Recommended Mitigation Steps:
Use elementary types or a userr-defined type instead can save gas
________________________________________________________________________

8.
Title: `function getUserTotalStakingAmount()` gas improvement on returning value

Proof of Concept:
[ApeStakingLogic.sol#L212](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L212)

Recommended Mitigation Steps:
by set `totalAmount` in returns L#210 and delete L#212 can save gas

```
function getUserTotalStakingAmount(
        mapping(address => UserState) storage userState,
        mapping(address => mapping(uint256 => uint256)) storage ownedTokens,
        address user,
        uint256 poolId,
        ApeCoinStaking _apeCoinStaking
    ) external view returns (uint256 totalAmount) { //@audit-info: set here
```
________________________________________________________________________