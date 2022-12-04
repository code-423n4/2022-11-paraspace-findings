# 1. [G-1] For loops: increments in for loop can be uncheck to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

    File paraspace-core/contracts/misc/NFTFloorOracle.sol, line 229:     for (uint256 i = 0; i < _assets.length; i++)
    File paraspace-core/contracts/misc/NFTFloorOracle.sol, line 291:     for (uint256 i = 0; i < _assets.length; i++)
    File paraspace-core/contracts/misc/NFTFloorOracle.sol, line 321:     for (uint256 i = 0; i < _feeders.length; i++)
    File paraspace-core/contracts/misc/NFTFloorOracle.sol, line 413:     for (uint256 i = 0; i < feederSize; i++)

    File paraspace-core/contracts/misc/ParaSpaceOracle.soll, line 95:     for (uint256 i = 0; i < assets.length; i++)
    File paraspace-core/contracts/misc/ParaSpaceOracle.soll, line 197:     for (uint256 i = 0; i < assets.length; i++)

    File paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol, line 193:     for (uint256 index = 0; index < tokenIds.length; index++)
    File paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol, line 210:     for (uint256 index = 0; index < tokenIds.length; index++)

    File paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol, line 38:     for (i = 0; i < params.nftTokenIds.length; i++)
    File paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol, line 56:     for (i = 0; i < params.nftTokenIds.length; i++)

    File paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol, line 371:     for (uint256 index = 0; index < totalBalance; index++)
    File paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol, line 466:     for (uint256 index = 0; index < totalBalance; index++)

    File paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol, line 177:     for (uint256 i = 0; i < marketplaceIds.length; i++)
    File paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol, line 284:     for (uint256 i = 0; i < marketplaceIds.length; i++)
    File paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol, line 382:     for (uint256 i = 0; i < params.orderInfo.consideration.length; i++)
    File paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol, line 470:     for (uint256 i = 0; i < params.orderInfo.offer.length; i++)

    File paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol, line 58:     for (uint16 i = 0; i < params.reservesCount; i++)
    File paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol, line 104:     for (uint256 i = 0; i < assets.length; i++)

    File paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol, line 184:     for (uint256 index = 0; index < params.tokenData.length; index++)
    File paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol, line 185:     for (uint256 index = 0; index < params.tokenData.length; index++)

    File paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol, line 131:     for (uint256 index = 0; index < amount; index++)
    File paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol, line 465:     for (uint256 index = 0; index < tokenIds.length; index++)
    File paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol, line 910:     for (uint256 index = 0; index < tokenIds.length; index++)
    File paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol, line 1034:     for (uint256 i = 0; i < params.nftTokenIds.length; i++)

    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 72:     for (uint256 index = 0; index < _nfts.length; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 103:     for (uint256 index = 0; index < _nfts.length; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 172:     for (uint256 index = 0; index < actualTransferAmount; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 199:     for (uint256 index = 0; index < _nftPairs.length; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 215:     for (uint256 index = 0; index < _nftPairs.length; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 279:     for (uint256 index = 0; index < _nfts.length; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 289:     for (uint256 index = 0; index < _nftPairs.length; index++)
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 305:     for (uint256 index = 0; index < _nftPairs.length; index++)

    File paraspace-core/contracts/protocol/pool/PoolCore.sol, line 634:     for (uint256 i = 0; i < reservesListCount; i++)

    File paraspace-core/contracts/protocol/tokenization/NToken.sol, line 106:     for (uint256 index = 0; index < tokenIds.length; index++)
    File paraspace-core/contracts/protocol/tokenization/NToken.sol, line 145:     for (uint256 i = 0; i < ids.length; i++)

    File paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol, line 107:     for (uint256 index = 0; index < tokenIds.length; index++)

    File paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol, line 51:     for (uint256 index = 0; index < tokenIds.length; index++)
    File paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol, line 97:     for (uint256 index = 0; index < tokenIds.length; index++)

    File paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol, line 493:     for (uint256 index = 0; index < tokenIds.length; index++)

    File paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol, line 213:     for (uint256 index = 0; index < totalBalance; index++)

    File paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol, line 207:     for (uint256 index = 0; index < tokenData.length; index++)
    File paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol, line 280:     for (uint256 index = 0; index < tokenIds.length; index++)

    File paraspace-core/contracts/ui/WPunkGateway.sol, line 82:     for (uint256 i = 0; i < punkIndexes.length; i++)
    File paraspace-core/contracts/ui/WPunkGateway.sol, line 109:     for (uint256 i = 0; i < punkIndexes.length; i++)
    File paraspace-core/contracts/ui/WPunkGateway.sol, line 113:     for (uint256 i = 0; i < punkIndexes.length; i++)
    File paraspace-core/contracts/ui/WPunkGateway.sol, line 136:     for (uint256 i = 0; i < punkIndexes.length; i++)
    File paraspace-core/contracts/ui/WPunkGateway.sol, line 174:     for (uint256 i = 0; i < punkIndexes.length; i++)

The code would go from:

    for (uint256 i; i < numIterations; i++) { 
      ...
    }

to:

    for (uint256 i; i < numIterations;) { 
      ...
      unchecked { ++i; }  
    }

# 2. [G-2] Variables: "Constant" expressions are expressions, not constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

If the variable was immutable instead: the calculation would only be done once at deploy time (in the constructor), and then the result would be saved and read directly at runtime rather than being recalculated.

Instances include:

    File paraspace-core/contracts/misc/NFTFloorOracle.sol, line 70:             bytes32 public constant UPDATER_ROLE = keccak256("UPDATER_ROLE");

    File paraspace-core/contracts/protocol/pool/PoolStorage.sol, line 16:             bytes32 constant POOL_STORAGE_POSITION =
        bytes32(uint256(keccak256("paraspace.proxy.pool.storage")) - 1);

    File araspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol, line 23:             bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =
        bytes32(
            uint256(keccak256("paraspace.proxy.ntoken.apestaking.storage")) - 1
        );  

Change these expressions from `constant` to `immutable` and implement the calculation in the constructor, or hardcode these values in the constants and add a comment to say how the value was calculated.

# 3. [G-3] <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

There are 4 instances of this issue:

    File paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol, line 149 - 150:
                    token0Amount += positionData.tokensOwed0;
                    token1Amount += positionData.tokensOwed1;

    File paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol, line 169, 176, 178, 185, 189, 231, 233, 235, 237, 238, 380, 479, 496.
    File paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol, line 397.
    File paraspace-core/contracts/protocol/pool/PoolApeStaking.sol, line 77, 166.
    File paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol, line 215, 257.

