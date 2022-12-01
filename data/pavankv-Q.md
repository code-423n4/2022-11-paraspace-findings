1. use Block.timestamp:-
Description

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

code snippet:-
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L380

recommendation:-
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.
Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number. 

2. Update latest version of pragma:-

code snippet:-
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L2
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L2
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L2

3. Tight the struct variables :-

code snippet:-
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L43

Change :-
struct FeederPosition {
// index in feeder list
    uint256 index;
    // if feeder registered or not
    bool registered;
    
}

b)
code snippet:-
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L40

change to :-
struct PairOracleData {
        uint256 token0Price;
        uint8 token0Decimal;
        uint8 token1Decimal;
        uint160 sqrtPriceX96;
        uint256 token1Price;
    }
4. FRONT-RUNNABLE INITIALIZERS :-
There is no access control for initializers function. Allowing any user to initialize the contract. By front-running the contract deployers to initialize the contract, the incorrect parameters may be supplied, leaving the contract needing to be redeployed. 

code snippet:-
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L75

Recommendation:-
Setting the owner in the contract's constructor to the msg.sender and adding the onlyOwner modifier to all initializers

reference :-
https://github.com/code-423n4/2022-01-trader-joe-findings/issues/8
