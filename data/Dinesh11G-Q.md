### 1st BUG
Unsafe ERC20 Operation(s)

#### Impact
Issue Information: 
ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

To circumvent ERC20's approve functions race-condition vulnerability use OpenZeppelin's SafeERC20 library's safe{Increase|Decrease}Allowance functions.

In case the vulnerability is of no danger for your implementation, provide enough documentation explaining the reasonings.

Example
🤦 Bad:

IERC20(token).transferFrom(msg.sender, address(this), amount);
🚀 Good (using OpenZeppelin's SafeERC20):

import {SafeERC20} from "openzeppelin/token/utils/SafeERC20.sol";

// ...

IERC20(token).safeTransferFrom(msg.sender, address(this), amount);
🚀 Good (using require):

bool success = IERC20(token).transferFrom(msg.sender, address(this), amount);
require(success, "ERC20 transfer failed");

#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerNonCompliantERC721.sol::37 => IERC721(collection).transferFrom(from, to, tokenId);
2022-11-paraspace/paraspace-core/contracts/dependencies/weth/WETH9.sol::43 => payable(msg.sender).transfer(wad);
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::187 => payable(msg.sender).transfer(amountEth);
2022-11-paraspace/paraspace-core/contracts/mocks/tests/MockAirdropProject.sol::50 => erc20Token.transfer(to, erc20Bonus);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::252 => payable(msg.sender).transfer(msg.value - price);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/BAYC.sol::2250 => payable(msg.sender).transfer(balance);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1931 => payable(msg.sender).transfer(balance);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::496 => payable(msg.sender).transfer(msg.value.sub(salePrice));
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::498 => beneficiary.transfer(salePrice);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/WrappedPunk/WPunk.sol::68 => proxy.transfer(address(_punkContract), punkIndex),
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::45 => _apeCoin.approve(address(_apeCoinStaking), type(uint256).max);
2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol::46 => _apeCoin.approve(address(POOL), type(uint256).max);
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::42 => WETH.approve(pool, type(uint256).max);
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::81 => pWETH.transferFrom(msg.sender, address(this), amountToWithdraw);
2022-11-paraspace/paraspace-core/contracts/ui/WETHGateway.sol::172 => pWETH.transferFrom(msg.sender, address(this), amountToWithdraw);
```
#### Tools used
Manual




### 2nd BUG
Unspecific Compiler Version Pragma

#### Impact
Issue Information: 
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

Example
🤦 Bad:

pragma solidity ^0.8.0;
🚀 Good:

pragma solidity 0.8.4;
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/chainlink/AggregatorInterface.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/CurrencyManager.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/ExecutionManager.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/LooksRareExchange.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/RoyaltyFeeManager.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/TransferSelectorNFT.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyAnyItemFromCollectionForFixedPrice.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyAnyItemInASetForFixedPrice.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyDutchAuction.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyPrivateSale.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/executionStrategies/StrategyStandardSaleForFixedPrice.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/ICurrencyManager.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/IExecutionManager.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/IExecutionStrategy.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/ILooksRareExchange.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/IOwnable.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/IRoyaltyFeeManager.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/IRoyaltyFeeRegistry.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/ITransferManagerNFT.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/ITransferSelectorNFT.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/interfaces/IWETH.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/OrderTypes.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeRegistry.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/royaltyFeeHelpers/RoyaltyFeeSetter.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC1155.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerERC721.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/looksrare/contracts/transferManagers/TransferManagerNonCompliantERC721.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMath.sol::2 => pragma solidity >=0.8.4;
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathSD59x18.sol::2 => pragma solidity >=0.8.4;
2022-11-paraspace/paraspace-core/contracts/dependencies/math/PRBMathUD60x18.sol::2 => pragma solidity >=0.8.4;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ECDSA.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155Holder.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC1155Receiver.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Enumerable.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ERC721Holder.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155MetadataURI.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC1155Receiver.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC20WithPermit.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC721.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC721Enumerable.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/IERC721Metadata.sol::4 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Math.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/Pausable.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/ReentrancyGuard.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/AddressUpgradeable.sol::4 => pragma solidity ^0.8.1;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/ContextUpgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/IERC20Upgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/Initializable.sol::4 => pragma solidity ^0.8.2;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/OwnableUpgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/PausableUpgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/ReentrancyGuardUpgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/draft-IERC20PermitUpgradeable.sol::4 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/Seaport.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/Conduit.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/ConduitController.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/lib/ConduitConstants.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/lib/ConduitEnums.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/conduit/lib/ConduitStructs.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelper.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/helpers/TransferHelperStructs.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/AbridgedTokenInterfaces.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/AmountDerivationErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ConduitControllerInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ConduitInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ConsiderationEventsAndErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ConsiderationInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/CriteriaResolutionErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/EIP1271Interface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/FulfillmentApplicationErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/IERC721Receiver.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ImmutableCreate2FactoryInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ReentrancyErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/SeaportInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/SignatureVerificationErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/TokenTransferrerErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/TransferHelperErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/TransferHelperInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ZoneInteractionErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/interfaces/ZoneInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/AmountDeriver.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Assertions.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/BasicOrderFulfiller.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Consideration.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationBase.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationConstants.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationEnums.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ConsiderationStructs.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CounterManager.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/CriteriaResolution.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Executor.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/FulfillmentApplier.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/GettersAndDerivers.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/LowLevelHelpers.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderCombiner.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderFulfiller.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/OrderValidator.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ReentrancyGuard.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/SignatureVerification.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrer.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/TokenTransferrerConstants.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/Verifiers.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/lib/ZoneInteraction.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZone.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/PausableZoneController.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneControllerInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneEventsAndErrors.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/seaport/contracts/zones/interfaces/PausableZoneInterface.sol::2 => pragma solidity ^0.8.7;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IERC721Permit.sol::2 => pragma solidity >=0.7.5;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IMulticall.sol::2 => pragma solidity >=0.7.5;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/INonfungiblePositionManager.sol::2 => pragma solidity >=0.7.5;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IPeripheryImmutableState.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IPeripheryPayments.sol::2 => pragma solidity >=0.7.5;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IPoolInitializer.sol::2 => pragma solidity >=0.7.5;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IUniswapV3Factory.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/IUniswapV3PoolState.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/LiquidityAmounts.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/PoolAddress.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/BitMath.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FixedPoint128.sol::2 => pragma solidity >=0.4.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FixedPoint96.sol::2 => pragma solidity >=0.4.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/FullMath.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Oracle.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Position.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SafeCast.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SqrtPriceMath.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/SwapMath.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/Tick.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/TickBitmap.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/TickMath.sol::2 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/uniswap/libraries/UnsafeMath.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IERC20Minimal.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/ISwapRouter.sol::2 => pragma solidity >=0.7.5;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IUniswapV3Factory.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IUniswapV3Pool.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/IUniswapV3PoolDeployer.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/callback/IUniswapV3FlashCallback.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/callback/IUniswapV3MintCallback.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/callback/IUniswapV3SwapCallback.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolActions.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolDerivedState.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolEvents.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolImmutables.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolOwnerActions.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/interfaces/pool/IUniswapV3PoolState.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/BitMath.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FixedPoint128.sol::2 => pragma solidity >=0.4.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FixedPoint96.sol::2 => pragma solidity >=0.4.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/FullMath.sol::2 => pragma solidity >=0.4.0 <0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/LiquidityMath.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/LowGasSafeMath.sol::2 => pragma solidity >=0.7.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Oracle.sol::2 => pragma solidity >=0.5.0 <0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Position.sol::2 => pragma solidity >=0.5.0 <0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SafeCast.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SqrtPriceMath.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/SwapMath.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/Tick.sol::2 => pragma solidity >=0.5.0 <0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TickBitmap.sol::2 => pragma solidity >=0.5.0 <0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TickMath.sol::2 => pragma solidity >=0.5.0 <0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/TransferHelper.sol::2 => pragma solidity >=0.6.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/univ3/libraries/UnsafeMath.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/IDelegate.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/IWETHUpgradable.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/MarketConsts.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/X2Y2_r1.sol::3 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/interfaces/IParaProxy.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/misc/interfaces/IUniswapV2Pair.sol::2 => pragma solidity >=0.5.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::9 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Azuki.sol::87 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::9 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::501 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::691 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::773 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::851 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/CloneX.sol::914 => pragma solidity ^0.8.2;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::6 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::770 => pragma solidity ^0.8.1;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/DOODLES.sol::1803 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::7 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::503 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::693 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::786 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::916 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::993 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::1048 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/MAYC.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Meebits.sol::5 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::12 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::356 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::436 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::474 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::496 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::572 => pragma solidity ^0.8.4;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::875 => pragma solidity ^0.8.4;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::1928 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2020 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2111 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2130 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2187 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2249 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2454 => pragma solidity >=0.8.0 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2505 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2570 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2615 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2909 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3027 => pragma solidity >=0.8.10 <0.9.0;
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/dependencies/ERC721A.sol::30 => pragma solidity ^0.8.0;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaProxy.sol::2 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/ParaReentrancyGuard.sol::3 => pragma solidity ^0.8.10;
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol::2 => pragma solidity ^0.8.10;
```
#### Tools used
Manual




### 3rd BUG
Do not use Deprecated Library Functions

#### Impact
Issue Information: 
The usage of deprecated library functions should be discouraged.

This issue is mostly related to OpenZeppelin libraries.

Example
🤦 Bad:

use SafeERC20 for IERC20;

// ...

IERC20(token).safeApprove(spender, value);
🚀 Good:

use SafeERC20 for IERC20;

// ...

IERC20(token).safeIncreaseAllowance(spender, value);
#### Findings:
```
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/AccessControl.sol::212 => function _setupRole(bytes32 role, address account) internal virtual {
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/contracts/SafeERC20.sol::46 => function safeApprove(
2022-11-paraspace/paraspace-core/contracts/dependencies/openzeppelin/upgradeability/SafeERC20Upgradeable.sol::46 => function safeApprove(
2022-11-paraspace/paraspace-core/contracts/dependencies/x2y2/contracts/ERC721Delegate.sol::21 => _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::104 => _setupRole(DEFAULT_ADMIN_ROLE, _admin);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::106 => _setupRole(UPDATER_ROLE, _admin);
2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol::314 => _setupRole(UPDATER_ROLE, _feeder);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Land.sol::830 => function safeApprove(
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::2876 => function _setupRole(bytes32 role, address account) internal virtual {
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3014 => function _setupRole(bytes32 role, address account)
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3019 => super._setupRole(role, account);
2022-11-paraspace/paraspace-core/contracts/mocks/tokens/Moonbirds.sol::3213 => _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
2022-11-paraspace/paraspace-core/contracts/protocol/configuration/ACLManager.sol::36 => _setupRole(DEFAULT_ADMIN_ROLE, aclAdmin);
2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol::555 => IERC20(token).safeApprove(operator, type(uint256).max);
```
#### Tools used
Manual