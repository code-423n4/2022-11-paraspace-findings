#  [L-01] Use the latest version of OpenZeppelin

To prevent any issues in the future (e.g. using solely hardhat to compile and deploy the contracts), upgrade the used OZ packages within the package.json to the latest versions. Currently the project is using 4.2.0. 

Upgrade it to stable 4.8.0 (which is the newest)

Lines of code:

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/package.json#L31


# [N-01] The nonReentrant modifier should occur before all other modifiers

This is a best-practice to protect against reentrancy in other modifiers. 

function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant
    {

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26

 function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant
    {

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L39

function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L52

 function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant
    {

- https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L66


# [N-02] Constants should be defined rather than using magic numbers

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name.

C4Audit didn't find the below ones:

File: NFTFloorOracle.sol

  ? (_twap * 100) / _priorTwap
   : (_priorTwap * 100) / _twap;

Lines of code:

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L366
- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L367


# [N-03] 10 ** 18 can be changed to 1e18 for readability

File: UniswapV3OracleWrapper.sol

(oracleData.token0Price * (10**18)) 

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L245

(10 **
                            (18 +
                                oracleData.token1Decimal -
                                oracleData.token0Decimal)))

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L254

 (10 **
                            (18 +
                                oracleData.token0Decimal -
                                oracleData.token1Decimal)))

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L266

# [N-04] Typo in comments


File: LiquidationLogic.sol

 //userDebt from liquadationReserve  - `liquidationReserve`

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L104

File: NFTFloorOrcale.sol

/// aggeregate prices  - `aggregate`
 //aggeregate with price - `aggregate`

- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L53
- https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412

