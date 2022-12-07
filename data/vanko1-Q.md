# Low/Non-critial issues

# Borrower can liqudate himself when the collateral is ERC20 token

## Proof of Concept

Liqudations for ERC20 collateral can be triggered in the `PoolCore.sol` contract. Validation is done in `ValidationLogic.sol` library:  
```solidity
    function validateLiquidateERC20(
        DataTypes.UserConfigurationMap storage userConfig,
        DataTypes.ReserveData storage collateralReserve,
        DataTypes.ValidateLiquidateERC20Params memory params
    ) internal view {
```   
There is no validation if a the liqudatior address is the same as borrower address.
## Impact

This might lead to a user with bad debt collecting the liqudation penalty. Furthermore, the protocol might miscalculate the user's XTokens and this can lead to potential attack vectors.


## Recommendation

Use the `LIQUIDATIOR_CANNOT_BE_SELF` error in `ERRORS.sol` like it is used in the `validateLiquidateERC721` function :
```solidity
   require(
       params.liquidator != params.borrower,
       Errors.LIQUIDATOR_CAN_NOT_BE_SELF
   );
```   

# Missing revert can cause redundant gas cost for user

## Proof of Concept

Users can set/unset their supplied assets as collateral in `PoolCore.sol` contract. The logic is in `SupplyLogic.sol` library, `executeUseERC20AsCollateral` function. If the user's settings on-chain already match the function parameters, it returns:

```solidity
   if (useAsCollateral == userConfig.isUsingAsCollateral(reserve.id))
       return; 
```
This can lead to users potentialy waisting gas for wrong inputs.

## Recommendation
Use `revert` instead. This way web3 wallet providers will not estimate gas or warn the user that the transaction will fail. 

# Unused constants in `Errors.sol`

## Proof of Concept

`Errors.sol` defines the protocol's error messages, but some of them are not used. Consider if they are to be used in the future or there is missing logic in the protol where they should be used.  
There are 31 instances of this issue:  

```solidity
15: string public constant CALLER_NOT_BRIDGE = "6"; // 'The caller of the function is not a bridge'
36: string public constant INVALID_INTEREST_RATE_MODE_SELECTED = "33"; // 'Invalid interest rate mode selected'
41: string public constant COLLATERAL_SAME_AS_BORROWING_CURRENCY = "37"; // 'Collateral is (mostly) the same currency that is being borrowed'
42: string public constant AMOUNT_BIGGER_THAN_MAX_LOAN_SIZE_STABLE = "38"; // 'The requested amount is greater than the max loan size in stable rate mode'
45: string public constant NO_OUTSTANDING_STABLE_DEBT = "41"; // 'User does not have outstanding stable rate debt on this reserve
46: string public constant NO_OUTSTANDING_VARIABLE_DEBT = "42"; // 'User does not have outstanding variable rate debt on this reserve'
48: string public constant INTEREST_RATE_REBALANCE_CONDITIONS_NOT_MET = "44"; // 'Interest rate rebalance conditions were not met'
56: string public constant STABLE_DEBT_NOT_ZERO = "55"; // 'Stable debt supply is not zero'
69: string public constant INVALID_DEBT_CEILING = "73"; // 'Invalid debt ceiling for the reserve
79: string public constant INVALID_OPTIMAL_STABLE_TO_TOTAL_DEBT_RATIO = "84"; // 'Invalid optimal stable to total debt ratio'
84: string public constant SILOED_BORROWING_VIOLATION = "89"; // 'User is trying to borrow multiple assets including a siloed one'
92: string public constant TOKEN_TRANSFERRED_CAN_NOT_BE_SELF_ADDRESS = "97"; //token transferred can not be self address.
96: string public constant SUPPLIER_NOT_NTOKEN = "101"; //supplier is not the NToken contract
98-103:  
string public constant INVALID_MARKETPLACE_ID = "103"; //invalid marketplace id.
string public constant INVALID_MARKETPLACE_ORDER = "104"; //invalid marketplace id.
string public constant CREDIT_DOES_NOT_MATCH_ORDER = "105"; //credit doesn't match order.
string public constant PAYNOW_NOT_ENOUGH = "106"; //paynow not enough.
string public constant INVALID_CREDIT_SIGNATURE = "107"; //invalid credit signature.
string public constant INVALID_ORDER_TAKER = "108"; //invalid order taker.  
105: string public constant INVALID_AUCTION_RECOVERY_HEALTH_FACTOR = "110"; //invalid auction recovery health factor.
110: string public constant TOKEN_IN_AUCTION = "115"; //tokenId is in auction.
111-113:
string public constant AUCTIONED_BALANCE_NOT_ZERO = "116"; //auctioned balance not zero.
string public constant LIQUIDATOR_CAN_NOT_BE_SELF = "117"; //user can not liquidate himself.
string public constant INVALID_RECIPIENT = "118"; //invalid recipient specified in order.
115-117:
string public constant NTOKEN_BALANCE_EXCEEDED = "120"; //ntoken balance exceed limit.
string public constant ORACLE_PRICE_NOT_READY = "121"; //oracle price not ready.
string public constant SET_ORACLE_SOURCE_NOT_ALLOWED = "122"; //source of oracle not allowed to set.
121-122:
string public constant ORACLE_PRICE_EXPIRED = "126"; //oracle price expired.
string public constant APE_STAKING_POSITION_EXISTED = "127"; //ape staking position is existed.
124-125: 
string public constant TOTAL_STAKING_AMOUNT_WRONG = "129"; //cash plus borrow amount not equal to total staking amount.
string public constant APE_STAKING_AMOUNT_NON_ZERO = "130"; //ape staking amount should be zero when supply bayc/mayc.
```

