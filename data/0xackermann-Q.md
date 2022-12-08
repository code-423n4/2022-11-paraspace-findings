### Non Critical

1. Naming convention: 
- Line: [IUniswapV3PositionInfoProvider.sol#L4](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/interfaces/IUniswapV3PositionInfoProvider.sol#L4)
Simple refactoring from `UinswapV3PositionData` to `UniswapV3PositionData` to show professionalism. 
Affected code: [UniswapV3OracleWrapper](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol)
- Line: [AuctionLogic.sol#L34](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/AuctionLogic.sol#L34)
    
    Use `start` instead of `tsatr` to remain professionalism.
    
2. `isFirstBorrowing` refactoring
    
     Line: [BorrowLogic#L81](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L81)
    
    The variable `isFirstBorrowing` should be named as `isBorrowing`.
    
    The calculation of `isFirstBorrowing` is from function `_mintScaled`  [ScaledBalanceTokenBaseERC20#L91-L113](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/ScaledBalanceTokenBaseERC20.sol#L91-L113), 
    
    ```solidity
    function _mintScaled(
        address caller,
        address onBehalfOf,
        uint256 amount,
        uint256 index
    ) internal returns (bool) {
        uint256 amountScaled = amount.rayDiv(index);
        require(amountScaled != 0, Errors.INVALID_MINT_AMOUNT);
    
        uint256 scaledBalance = super.balanceOf(onBehalfOf);
        uint256 balanceIncrease = scaledBalance.rayMul(index) -
            scaledBalance.rayMul(_userState[onBehalfOf].additionalData);
    
        _userState[onBehalfOf].additionalData = index.toUint128();
    
        _mint(onBehalfOf, amountScaled.toUint128());
    
        uint256 amountToMint = amount + balanceIncrease;
        emit Transfer(address(0), onBehalfOf, amountToMint);
        emit Mint(caller, onBehalfOf, amountToMint, balanceIncrease, index);
    
        return (scaledBalance == 0);
    }
    ```
    
    We can see if the user borrows, then repays in full and borrows again. This will not be the first time the user is borrowing. 
    
3. Return names are not being used in functions
    
    Instances:
    
    - [MintableIncentivizedERC721#L370-L373](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L370-L373)
    - [MintableIncentivizedERC721#L387-390](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L387-L390)
    - [NFTFloorOracle#L240](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L240https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/tmp/NFTFloorOracle.sol#L240)
    - [NFTFloorOracle#L256](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L256)
    - [NTokenMoonBirds#L114-L118](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L114-L118)
    - [PoolLogic#L218](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L218)
    - [PoolParameters#L231-239](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L231-L239)
    - [PoolParameters#L256](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L256)
