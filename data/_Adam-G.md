### [G01] x = x + y is Cheaper than x += y

**Description:**
Based on test in remix you can save ~1,007 gas on deployment and ~15 gas on execution cost if you use x = x + y over x += y (Is only true for Storage Variables).

```
contract Test {
	uint256 x = 1;
	function test() external {
		x += 3; 
		(Deployment Cost: 153,124, Execution Cost: 30,369)
		
		vs
		
		x = x + 1;
		(Deployment Cost: 152,117, Execution Cost: 30,354)
	}

}
```

**LOC:**
[MintableERC721Logic.sol#L146](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L146)  
[MintableERC721Logic.sol#L318](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L318) 

**Recommendation:**
Change line 146 to: erc721Data.userState[from].collateralizedBalance = erc721Data.userState[from].collateralizedBalance - 1;
Change line 318 to: erc721Data.userState[user].balance = erc721Data.userState[user].balance - balanceToBurn;


### [G02] Emitting State Variables

**Description:** You can save reading from storage (~115 gas) by emiting local variables over storage variables when they have the same value. (will also save a small amount of deployment gas ~483) 

```
contract Test {
	uint256 testNum;
	event testEvent(uint256 testNum);

	function test(uint256 num) public {
		testNum = num;
		emit testEvent(num);
		(Deployment Cost: 145,812, Execution Cost: 51,627)
		
		vs
		
		testNum = num;
		emit testEvent(testNum);
		(Deployment Cost: 146,295, Execution Cost: 51,742)
	}

}
```

**LOC:** 
[NFTFloorOracle.sol#L381-L385](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L381-L385) 

**Recommendation:** 
Change line 383 to: `_twap`
Change line 384 to: block.number


### [G03] Long Revert Strings 

**Description:** 
If opting not to update revert strings to custom errors, keeping revert strings <= 32 bytes in length will save gas. I ran a test in remix and found the savings for a single short revert string vs long string to be ~9,377 gas in deployment cost and ~18 gas on function call. 

``` 
contract Test { 
	uint256 a; 
	
	function check() external { 
		require(a != 0, "short error message"); 
		(Deployment cost: 114,799, Cost on function call: 23,392) 
		
		vs 
		
		require(a != 0, "A longer Error Message over 32 bytes in length"); 
		(Deployment cost: 124,176, Cost on function call: 23,410) 
	} 
} 
```

**LOC:** 
[NFTFloorOracle.sol#L225-L228](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L225-L228) 
[NFTFloorOracle.sol#L356](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L356) 
[MintableIncentivizedERC721.sol#L195-L198](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L195-L198) 
[MintableIncentivizedERC721.sol#L213-L216](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L213-L216) 
[MintableIncentivizedERC721.sol#L259-L262](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L259-L262) 
[MintableIncentivizedERC721.sol#L296-L299](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L296-L299) 
[MintableIncentivizedERC721.sol#L354-L357](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L354-L357) 
[MintableIncentivizedERC721.sol#L594-L597](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L594-L597) 
[MintableIncentivizedERC721.sol#L618-L621](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L618-L621) 
[NTokenMoonBirds.sol#L98-L101](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L98-L101) 
[MintableERC721Logic.sol#L88-L91](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L88-L91) 
[MintableERC721Logic.sol#L92](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92) 
[MintableERC721Logic.sol#L377-L380](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L377-L380) 
[MintableERC721Logic.sol#L395-L398](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L395-L398) 

**Recommendation:** 
Either replace the revert strings with custom errors or shorten to be less than 32 bytes in length.

### [G04] && in Require Statements 

**Description:**
If optimising for runtime costs over deployment costs you can seperate && in require functions into 2 parts. I ran a basic test in remix and it cost an extra 234 gas to deploy but will save ~9 gas everytime the require function is called. (note: If you upgrade to solidity version > 0.8.13 this is no longer true) 

``` solidity 
contract Test { 
	uint256 a = 0; 
	uint256 b = 1; 
	
	function test() external { 
		require(a == 0 && b > a) 
		(Deployment cost: 123,291, Cost on function call: 29,371) 
		
		vs 
		
		require(a == 0); 
		require(b > a); 
		(Deployment cost: 123,525, Cost on function call: 29,362) 
	} 
} 
```

**LOC:** 
[SeaportAdapter.sol#L41-L45](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41-L45) 
[SeaportAdapter.sol#L71-L77](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L71-L77) 
[MarketplaceLogic.sol#L171-L175](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-L175) 
[MarketplaceLogic.sol#L279-L283](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279-L283) 
[MarketplaceLogic.sol#L388-L392](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L388-L392) 
[ValidationLogic.sol#L546-L549](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L549) 
[ValidationLogic.sol#L550-L553](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L550-L553) 
[ValidationLogic.sol#L636-L639](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L636-L639) 
[ValidationLogic.sol#L640-L643](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L640-L643) 
[ValidationLogic.sol#L1196-L1199](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1196-L1199) 
[ValidationLogic.sol#L1202-L1205](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1202-L1205) 
[ValidationLogic.sol#L1208-L1211](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1208-L1211) 
[PoolParameters.sol#L216-L220](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-L220) 

**Recommendation:** 
Depending on whether you decided to prioritise running costs vs deployment costs consider splitting the require statements into 2 seperate ones.


### [G05] Unchecked Increments in Loops 

**Description:**
When incrementing i in for loops there is no chance of overflow so unchecked can be used to save gas. I ran a simple test in remix and found deployment savings of ~31,653 gas and on each function call saved ~141 gas per iteration. 

``` 
contract Test { 
	function loopTest() external { 
		for (uint256 i; i < 1; ++i) { 
			Deployment Cost: 125,637, Cost on function call: 24,601 
			
		vs 
			
		for (uint256 i; i < 1; ) { 
			// for loop body 
			unchecked { ++i } 
			Deployment Cost: 93,984, Cost on function call: 24,460 
		} 
	} 
} 
```

**LOC:**
[NFTFloorOracle.sol#L229-L231](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229-L231) 
[NFTFloorOracle.sol#L291-L293](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291-L293) 
[NFTFloorOracle.sol#L321-L323](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321-L323) 
[NFTFloorOracle.sol#L413-L424](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413-L424) 
[ParaSpaceOracle.sol#L95-L102](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95-L102) 
[ParaSpaceOracle.sol#L197-L199](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197-L199) 
[UniswapV3OracleWrapper.sol#L193-L195](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193-L195) 
[UniswapV3OracleWrapper.sol#L210-L212](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210-L212) 
[FlashClaimLogic.sol#L38-L43](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38-L43) 
[FlashClaimLogic.sol#L56-L69](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56-L69) 
[GenericLogic.sol#L371-L386](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371-L386) 
[GenericLogic.sol#L466-L501](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466-L501) 
[MarketplaceLogic.sol#L177-L224](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177-L224) 
[MarketplaceLogic.sol#L284-L318](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284-L318) 
[MarketplaceLogic.sol#L382-L398](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382-L398) 
[MarketplaceLogic.sol#L470-L528](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470-L528) 
[SupplyLogic.sol#L184-L193](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184-L193) 
[SupplyLogic.sol#L195-L201](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195-L201) 
[ValidationLogic.sol#L131-L146](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131-L146) 
[ValidationLogic.sol#L212-L220](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212-L220) 
[ValidationLogic.sol#L465-L474](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465-L474) 
[ValidationLogic.sol#L910-L919](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910-L919) 
[ValidationLogic.sol#L1034-L1039](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034-L1039) 
[PoolApeStaking.sol#L72-L78](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72-L78) 
[PoolApeStaking.sol#L103-L108](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103-L108) 
[PoolApeStaking.sol#L138-L167](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138-L167) 
[PoolApeStaking.sol#L172-L178](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172-L178) 
[PoolApeStaking.sol#L199-L210](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199-L210) 
[PoolApeStaking.sol#L215-L221](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215-L221) 
[PoolApeStaking.sol#L279-L285](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279-L285) 
[PoolApeStaking.sol#L289-L302](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289-L302) 
[PoolApeStaking.sol#L305-L311](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305-L311) 
[PoolCore.sol#L634-L640](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634-L640) 
[MintableIncentivizedERC721.sol#L493-L501](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493-L501) 
[NToken.sol#L106-L112](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106-L112) 
[NToken.sol#L145-L147](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145-L147) 
[NTokenApeStaking.sol#L107-L120](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107-L120) 
[NTokenMoonBirds.sol#L51-L57](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51-L57) 
[NTokenMoonBirds.sol#L97-L102](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97-L102) 
[ApeStakingLogic.sol#L213-L220](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213-L220) 
[MintableERC721Logic.sol#L207-L238](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207-L238) 
[MintableERC721Logic.sol#L280-L316](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280-L316) 
[WPunkGateway.sol#L82-L87](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L82-L87) 
[WPunkGateway.sol#L109-L111](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L109-L111) 
[WPunkGateway.sol#L113-L116](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L113-L116) 
[WPunkGateway.sol#L136-L147](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L136-L147) 
[WPunkGateway.sol#L174-L185](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/ui/WPunkGateway.sol#L174-L185) 

**Recommendation:** 
Move the loop increments to the end of the loops body and wrap in an unchecked block.