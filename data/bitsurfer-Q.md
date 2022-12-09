# NO TRANSFER OWNERSHIP PATTERN

Openzeppelin Ownership contract (including the upgradable) is not using two step transfer pattern. This function checks that the new owner is not the zero address and proceeds to write the new owner’s address into the owner’s state variable. If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier.

Recommend considering implementing a two step process where the owner nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.

# USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’

There are still contract using plain import file, which is not recommended.

```solidity
PoolApeStaking.sol
04: import "../libraries/paraspace-upgradeability/ParaReentrancyGuard.sol";
05: import "../libraries/paraspace-upgradeability/ParaVersionedInitializable.sol";
07: import "../../interfaces/IPoolApeStaking.sol";
08: import "../../interfaces/IPToken.sol";
09: import "../../dependencies/yoga-labs/ApeCoinStaking.sol";
10: import "../../interfaces/IXTokenType.sol";
11: import "../../interfaces/INTokenApeStaking.sol";
20: import "../libraries/logic/BorrowLogic.sol";
21: import "../libraries/logic/SupplyLogic.sol";

NFTFloorOracle.sol
4: import "../dependencies/openzeppelin/contracts/AccessControl.sol";
5: import "../dependencies/openzeppelin/upgradeability/Initializable.sol";
6: import "./interfaces/INFTFloorOracle.sol";

MintableERC721Logic.sol
7: import "../../../interfaces/IRewardController.sol";
8: import "../../libraries/types/DataTypes.sol";
9: import "../../../interfaces/IPool.sol";

ApeStakingLogic.sol
07: import "../../../interfaces/IPool.sol";
11: import "./MintableERC721Logic.sol";

NTokenApeStaking.sol
12: import "../../interfaces/INTokenApeStaking.sol";

FlashClaimLogic.sol
10: import "../../../interfaces/INTokenApeStaking.sol";

ValidationLogic.sol
30: import "../../../interfaces/INTokenApeStaking.sol";
```

# NO EMIT WHEN CHANGING STATE

When changing state, it's best practice to emit an event.
Most of contract in this project did emit an event, but this following functions are missing in `PoolParameters.sol`

- `setReserveInterestRateStrategyAddress()`
- `setReserveAuctionStrategyAddress()`
- `setConfiguration()`
- `setAuctionRecoveryHealthFactor()`
- `setAuctionValidityTime()`
