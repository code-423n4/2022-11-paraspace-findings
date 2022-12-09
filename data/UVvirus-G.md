# 1)FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE 

If a function modifier such as onlyRole, onlyPoolAdmin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost. 

There are 8 instances of this issue: 

In NFTFloorOracle.sol 

#L141
 onlyRole(DEFAULT_ADMIN_ROLE)
 
#L150
 onlyRole(DEFAULT_ADMIN_ROLE)
 
#L160
 onlyRole(DEFAULT_ADMIN_ROLE)
 
#L177
 onlyRole(DEFAULT_ADMIN_ROLE)

#L185
 onlyRole(DEFAULT_ADMIN_ROLE)

#L197
 onlyRole(UPDATER_ROLE)

#L224
 external onlyRole(UPDATER_ROLE) {
 
In MintableIncentivizedERC721.sol 
#L133
  onlyPoolAdmin
  
=====================================================================================================================
 # 2)USE CUSTOM ERRORS RATHER THAN REQUIRE() STRINGS TO SAVE GAS 

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas  

There are 16 instances of this issue: 

In NFTFloorOracle.sol 

#L118
 require(_isAssetExisted(_asset), "NFTOracle: asset not existed");

#L123
 require(!_isAssetExisted(_asset), "NFTOracle: asset existed");

#L128
 require(_isFeederExisted(_feeder), "NFTOracle: feeder not existed");
 
 
#L133
 require(!_isFeederExisted(_feeder), "NFTOracle: feeder existed");
 
#L207
 require(dataValidity, "NFTOracle: invalid price data");
 
#L225
 require(
            _assets.length == _twaps.length,
            "NFTOracle: Tokens and price length differ"
        );
        
#L243
require(
            (block.number - updatedAt) <= config.expirationPeriod,
            "NFTOracle: asset price expired"
        );
        
#L267
 require(!_paused, "NFTOracle: nft price feed paused");

#L356
 require(_twap > 0, "NFTOracle: price should be more than 0");
 
In MintableIncentivizedERC721.sol 
#L193
 require(to != owner, "ERC721: approval to old owner");
 
#L197
 "ERC721: approve caller is not owner nor approved for all"
 
#L215
 "ERC721: approved query for nonexistent token"
 
#L261
 require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );
        
#L298
  require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );


=================================================================================================================
 # 3)++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--I/I-- TOO) 

Saves 5 gas per loop. 

There are 13 instances of this issue: 

In ParaSpaceOracle.sol #L197
 for (uint256 i = 0; i < assets.length; i++) 
 
In FlashClaimLogic.sol #L56
 for (i = 0; i < params.nftTokenIds.length; i++)
 

In MarketPlaceLogic.sol #L177
 for (uint256 i = 0; i < marketplaceIds.length; i++) {
 
In PoolLogic.sol 

#L58
  for (uint16 i = 0; i < params.reservesCount; i++)

#L104
 for (uint256 i = 0; i < assets.length; i++)
 
In PoolApeStaking.sol 

#L103
 for (uint256 index = 0; index < _nfts.length; index++)
 
#L138
 for (uint256 index = 0; index < _nftPairs.length; index++)
  
#L172
 for (uint256 index = 0; index < actualTransferAmount; index++)
 
#L199
 for (uint256 index = 0; index < _nftPairs.length; index++) 
 
#L215
 for (uint256 index = 0; index < _nftPairs.length; index++)
 
#L279
 for (uint256 index = 0; index < _nfts.length; index++)
 
#L289
 for (uint256 index = 0; index < _nftPairs.length; index++)
 
#L305
  for (uint256 index = 0; index < _nftPairs.length; index++)

================================================================================

  # 4)X += Y COSTS MORE GAS THAN X = X + Y  

Using the addition operator instead of plus-equals saves 113 gas. 

There are 7 instances of this issue: 

In GenericLogic.sol  
  
#L496
 totalLTV += tmpLTV * tokenPrice; 
 
#L497
  totalLiquidationThreshold +=
  
In PoolApeStaking.sol #L166
  amountToWithdraw += _nftPairs[index].amount;

============================================================================================================
 # 5)instead of using operator && on single require check  using double require check can save more gas:

There are 1 instances of this issue: 

In PoolParameters.sol #L216
 require(
            value > MIN_AUCTION_HEALTH_FACTOR &&
                value <= MAX_AUCTION_HEALTH_FACTOR,
            Errors.INVALID_AMOUNT
        );

=======================================================================================================================










