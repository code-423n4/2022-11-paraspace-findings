[1] DUPLICATED ``REQUIRE()``/``REVERT()`` CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

Saves deployment cost

*There are 71 instances of this issue:*

```
File: 2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

70:  revert(Errors.CALL_MARKETPLACE_FAILED);

102:   revert(Errors.CALL_MARKETPLACE_FAILED);
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L70](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L70)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L102](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L102)

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

51:  require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

84:  require(orderInfo.offer.length > 0, Errors.INVALID_MARKETPLACE_ORDER);

54-55:     require(
            orderInfo.consideration.length > 0,

87-88:  require(
            orderInfo.consideration.length > 0,

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L51](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L51)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L84)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L54-L55](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L54-L55)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L87-L88](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L87-L88)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

85:  require(isActive, Errors.RESERVE_INACTIVE);

185:  require(isActive, Errors.RESERVE_INACTIVE);

207:  require(isActive, Errors.RESERVE_INACTIVE);

390: require(isActive, Errors.RESERVE_INACTIVE);

438:  require(isActive, Errors.RESERVE_INACTIVE);

460:  require(isActive, Errors.RESERVE_INACTIVE);

1029:  require(isActive, Errors.RESERVE_INACTIVE);

1057: require(isActive, Errors.RESERVE_INACTIVE);

86:  require(!isPaused, Errors.RESERVE_PAUSED);

186: require(!isPaused, Errors.RESERVE_PAUSED);

208:  require(!isPaused, Errors.RESERVE_PAUSED);

391:  require(!isPaused, Errors.RESERVE_PAUSED);

439:  require(!isPaused, Errors.RESERVE_PAUSED);

461:  require(!isPaused, Errors.RESERVE_PAUSED);

1030:  require(!isPaused, Errors.RESERVE_PAUSED);

1058:  require(!isPaused, Errors.RESERVE_PAUSED);

68:  require(amount != 0, Errors.INVALID_AMOUNT);

160:  require(amount != 0, Errors.INVALID_AMOUNT);

71-72:  require(
            xToken.getXTokenType() != XTokenType.PTokenSApe,

168-169:  require(
            xToken.getXTokenType() != XTokenType.PTokenSApe,

421-422: require(
            xToken.getXTokenType() != XTokenType.PTokenSApe,

533-534:  require(
            xToken.getXTokenType() != XTokenType.PTokenSApe,

181-182:  require(
            reserveAssetType == DataTypes.AssetType.ERC20,

434-435: require(
            reserveAssetType == DataTypes.AssetType.ERC20,

203-204:  require(
            reserveAssetType == DataTypes.AssetType.ERC721,

456-457:  require(
            reserveAssetType == DataTypes.AssetType.ERC721,

611-612:  require(
            vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,

757-758:  require(
            vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,

815-816: require(
            vars.collateralReserveAssetType == DataTypes.AssetType.ERC721,

564-565:   require(
            params.healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

666-667: require(
                params.healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

574-575:  require(
            vars.isCollateralEnabled,

686-687: require(
            vars.isCollateralEnabled,

795-796:  require(
            vars.isCollateralEnabled,

762-764:   require(
            IERC721(params.xTokenAddress).ownerOf(params.tokenId) ==
                params.user,

819-821:  require(
            IERC721(params.xTokenAddress).ownerOf(params.tokenId) ==
                params.user,

768:   require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

824:  require(vars.collateralReserveActive, Errors.RESERVE_INACTIVE);

769:  require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

825:  require(!vars.collateralReservePaused, Errors.RESERVE_PAUSED);

937:  require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

950:  require(!reserve.configuration.getPaused(), Errors.RESERVE_PAUSED);

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L85](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L85)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L185](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L185)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L207](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L207)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L390](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L390)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L438](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L438)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L460](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L460)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1029](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1029)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1057](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1057)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L86](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L86)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L186](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L186)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L208](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L208)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L391](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L391)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L439](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L439)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L461](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L461)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1030](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1030)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1058](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1058)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L68](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L68)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L160](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L160)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L71-L72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L71-L72)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L168-L169](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L168-L169)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L421-L422](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L421-L422)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L533-L533](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L534-L534)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L181-L182](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L181-L182)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L434-L435](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L434-L435)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L203-L204](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L203-L204)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L456-L457](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L456-L457)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L611-L612](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L611-L612)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L757-L758](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L757-L758)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L815-L816](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L815-L816)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L564-L565](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L564-L565)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L666-L667](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L666-L667)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L574-L575](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L574-L575)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L686-L687](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L686-L687)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L795-L796](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L795-L796)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L762-L764](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L762-L764)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L819-L821](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L819-L821)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L768](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L768)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L824](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L824)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L769](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L769)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L825](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L825)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L937](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L937)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L950](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L950)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

85-87: require(
            getUserHf(msg.sender) >
                DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

112-114:  require(
            getUserHf(msg.sender) >
                DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

180-182:  require(
            getUserHf(msg.sender) >
                DataTypes.HEALTH_FACTOR_LIQUIDATION_THRESHOLD,

139-141:  require(
                INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
                    msg.sender,

200-202: require(
                INToken(xTokenAddress).ownerOf(_nftPairs[index].mainTokenId) ==
                    msg.sender,
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L85-L87](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L85-L87)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L112-L114](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L112-L114)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L180-L182](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L180-L182)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L139-L141](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L139-L141)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L200-L202](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L200-L202)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol

692-693:  require(
            msg.sender == ps._reserves[asset].xTokenAddress,

726-727: require(
            msg.sender == ps._reserves[asset].xTokenAddress,
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L692-L693](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L692-L693)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L726-L727](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L726-L727)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol

157: require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

172: require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

187:  require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

158-159:  require(
            ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,

173-174:  require(
            ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,

188-189:  require(
            ps._reserves[asset].id != 0 || ps._reservesList[0] == asset,
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L158-L159](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L158-L159)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L173-L174](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L173-L174)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L188-L189](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L188-L189)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

259-260 :  require(
            _isApprovedOrOwner(_msgSender(), tokenId),

296-297:  require(
            _isApprovedOrOwner(_msgSender(), tokenId),
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L259-L260](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L259-L260)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L296-L297](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L296-L297)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

93-94: require(
            !isAuctioned(erc721Data, POOL, tokenId),

167-168:  require(
            !isAuctioned(erc721Data, POOL, tokenId),

284-285:  require(
            !isAuctioned(erc721Data, POOL, tokenId),

373-374:  require(
            !isAuctioned(erc721Data, POOL, tokenId),

92:  require(to != address(0), "ERC721: transfer to the zero address");

199:  require(to != address(0), "ERC721: transfer to the zero address");

377-378:  require(
            _exists(erc721Data, tokenId),

395-396:  require(
            _exists(erc721Data, tokenId),
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L93-L94](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L93-L94)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L167-L168](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L167-L168)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L284-L285](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L284-L285)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L373-L374](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L373-L374)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L199)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L377-L378](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L377-L378)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L395-L396](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L395-L396)

[2] SPLITTING ``REQUIRE()`` STATEMENTS THAT USE ``&&`` SAVES GAS

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper.

*There are 14 instances of this issue:*

```
File: 2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

41-43:  require(
            // NOT criteria based and must be basic order
            resolvers.length == 0 && isBasicOrder(advancedOrder),

71-75:  require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&
                isBasicOrder(advancedOrders[0]) &&
                isBasicOrder(advancedOrders[1]),
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41-L43](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L41-L43)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L71-L75](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol#L71-L75)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

171-173 : require(
            marketplaceIds.length == payloads.length &&
                payloads.length == credits.length,

279-281:  require(
            marketplaceIds.length == payloads.length &&
                payloads.length == credits.length,

388-390:  require(
                item.itemType == ItemType.ERC20 ||
                    (vars.isETH && item.itemType == ItemType.NATIVE),
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-L173](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L171-L173)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279-L281](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L279-L281)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L388-L390](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L388-L390)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

546-547:  require(
            vars.collateralReserveActive && vars.principalReserveActive,

550-551:  require(
            !vars.collateralReservePaused && !vars.principalReservePaused,

636-637: require(
            vars.collateralReserveActive && vars.principalReserveActive,

640-641: require(
            !vars.collateralReservePaused && !vars.principalReservePaused,

672-674:  require(
            params.maxLiquidationAmount >= params.actualLiquidationAmount &&
                (msg.value == 0 || msg.value >= params.maxLiquidationAmount),

1196-1197:  require(
                vars.token0IsActive && vars.token1IsActive,

1202-1203:  require(
                !vars.token0IsPaused && !vars.token1IsPaused,

1208-1209:  require(
                !vars.token0IsFrozen && !vars.token1IsFrozen,
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L547](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L546-L547)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L550-L551](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L550-L551)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L636-L637](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L636-L637)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L640-L641](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L640-L641)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L672-L674](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L672-L674)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1196-L1197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1196-L1197)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1202-L1203](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1202-L1203)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1208-L1209](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1208-L1209)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol

216-218:  require(
            value > MIN_AUCTION_HEALTH_FACTOR &&
                value <= MAX_AUCTION_HEALTH_FACTOR,
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-L218](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L216-L218)


[3]  ++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--I/I-- TOO)

Saves 5 gas per loop

*There are 49 instances of this issue:*
```
File:  2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

229:  for (uint256 i = 0; i < _assets.length; i++) {

291:  for (uint256 i = 0; i < _assets.length; i++) {

321:  for (uint256 i = 0; i < _feeders.length; i++) {

413: for (uint256 i = 0; i < feederSize; i++) {


```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413)


```
File:  2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol

95: for (uint256 i = 0; i < assets.length; i++) {

197: for (uint256 i = 0; i < assets.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197)

```
File: 2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:  for (uint256 index = 0; index < tokenIds.length; index++) {

210:  for (uint256 index = 0; index < tokenIds.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38: for (i = 0; i < params.nftTokenIds.length; i++) {

56:  for (i = 0; i < params.nftTokenIds.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

371:  for (uint256 index = 0; index < totalBalance; index++) {

466:  for (uint256 index = 0; index < totalBalance; index++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:  for (uint256 i = 0; i < marketplaceIds.length; i++) {

284:  for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:  for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:  for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58:  for (uint16 i = 0; i < params.reservesCount; i++) {

104:  for (uint256 i = 0; i < assets.length; i++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:  for (uint256 index = 0; index < params.tokenData.length; index++) {

195:  for (uint256 index = 0; index < params.tokenData.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

131: for (uint256 index = 0; index < amount; index++) {

212:  for (uint256 index = 0; index < tokenIds.length; index++) {

465:  for (uint256 index = 0; index < tokenIds.length; index++) {

910:  for (uint256 index = 0; index < tokenIds.length; index++) {

1034:   for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72: for (uint256 index = 0; index < _nfts.length; index++) {

103:  for (uint256 index = 0; index < _nfts.length; index++) {

138:   for (uint256 index = 0; index < _nftPairs.length; index++) {

172: for (uint256 index = 0; index < actualTransferAmount; index++) {

199:  for (uint256 index = 0; index < _nftPairs.length; index++) {

215:  for (uint256 index = 0; index < _nftPairs.length; index++) {

279:  for (uint256 index = 0; index < _nfts.length; index++) {

289:  for (uint256 index = 0; index < _nftPairs.length; index++) {

305:  for (uint256 index = 0; index < _nftPairs.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol

634:  for (uint256 i = 0; i < reservesListCount; i++) {


```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493:  for (uint256 index = 0; index < tokenIds.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol##L493](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

106: for (uint256 index = 0; index < tokenIds.length; index++) {

145:  for (uint256 i = 0; i < ids.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

107:   for (uint256 index = 0; index < tokenIds.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:  for (uint256 index = 0; index < tokenIds.length; index++) {

97:  for (uint256 index = 0; index < tokenIds.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:  for (uint256 index = 0; index < tokenData.length; index++) {

280:  for (uint256 index = 0; index < tokenIds.length; index++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213:  for (uint256 index = 0; index < totalBalance; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213)

```
File:  2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

82:  for (uint256 i = 0; i < punkIndexes.length; i++) {

109:  for (uint256 i = 0; i < punkIndexes.length; i++) {

113:  for (uint256 i = 0; i < punkIndexes.length; i++) {

136:  for (uint256 i = 0; i < punkIndexes.length; i++) {

174:  for (uint256 i = 0; i < punkIndexes.length; i++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174)

[4] ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
 
The ``unchecked`` keyword is new in solidity 0.8.0,so this only applies to that version or higher, which these instances are. This saves 30-40 gas *[PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)*

*There are 49 instances of this issue:*

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

229:  for (uint256 i = 0; i < _assets.length; i++) {

291:  for (uint256 i = 0; i < _assets.length; i++) {

321:  for (uint256 i = 0; i < _feeders.length; i++) {

413: for (uint256 i = 0; i < feederSize; i++) {


```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L229)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L413)


```
File:  2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol

95: for (uint256 i = 0; i < assets.length; i++) {

197: for (uint256 i = 0; i < assets.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197)

```
File: 2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193:  for (uint256 index = 0; index < tokenIds.length; index++) {

210:  for (uint256 index = 0; index < tokenIds.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38: for (i = 0; i < params.nftTokenIds.length; i++) {

56:  for (i = 0; i < params.nftTokenIds.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

371:  for (uint256 index = 0; index < totalBalance; index++) {

466:  for (uint256 index = 0; index < totalBalance; index++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L371)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L466)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:  for (uint256 i = 0; i < marketplaceIds.length; i++) {

284:  for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:  for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:  for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58:  for (uint16 i = 0; i < params.reservesCount; i++) {

104:  for (uint256 i = 0; i < assets.length; i++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:  for (uint256 index = 0; index < params.tokenData.length; index++) {

195:  for (uint256 index = 0; index < params.tokenData.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

131: for (uint256 index = 0; index < amount; index++) {

212:  for (uint256 index = 0; index < tokenIds.length; index++) {

465:  for (uint256 index = 0; index < tokenIds.length; index++) {

910:  for (uint256 index = 0; index < tokenIds.length; index++) {

1034:   for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L131)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72: for (uint256 index = 0; index < _nfts.length; index++) {

103:  for (uint256 index = 0; index < _nfts.length; index++) {

138:   for (uint256 index = 0; index < _nftPairs.length; index++) {

172: for (uint256 index = 0; index < actualTransferAmount; index++) {

199:  for (uint256 index = 0; index < _nftPairs.length; index++) {

215:  for (uint256 index = 0; index < _nftPairs.length; index++) {

279:  for (uint256 index = 0; index < _nfts.length; index++) {

289:  for (uint256 index = 0; index < _nftPairs.length; index++) {

305:  for (uint256 index = 0; index < _nftPairs.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L172)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol

634:  for (uint256 i = 0; i < reservesListCount; i++) {


```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L634)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493:  for (uint256 index = 0; index < tokenIds.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol##L493](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

106: for (uint256 index = 0; index < tokenIds.length; index++) {

145:  for (uint256 i = 0; i < ids.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

107:   for (uint256 index = 0; index < tokenIds.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:  for (uint256 index = 0; index < tokenIds.length; index++) {

97:  for (uint256 index = 0; index < tokenIds.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:  for (uint256 index = 0; index < tokenData.length; index++) {

280:  for (uint256 index = 0; index < tokenIds.length; index++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

213:  for (uint256 index = 0; index < totalBalance; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L213)

```
File:  2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

82:  for (uint256 i = 0; i < punkIndexes.length; i++) {

109:  for (uint256 i = 0; i < punkIndexes.length; i++) {

113:  for (uint256 i = 0; i < punkIndexes.length; i++) {

136:  for (uint256 i = 0; i < punkIndexes.length; i++) {

174:  for (uint256 i = 0; i < punkIndexes.length; i++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174)


[5]  ``<ARRAY>.LENGTH`` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A`` FOR``-LOOP

The overheads outlined below are *PER LOOP*, excluding the first loop

-storage arrays incur a Gwarmaccess (100 gas)
-memory arrays use MLOAD (3 gas)
-calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset.

*There are 41 instances of this issue:*

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

229:  for (uint256 i = 0; i < _assets.length; i++) {

291:  for (uint256 i = 0; i < _assets.length; i++) {

321:  for (uint256 i = 0; i < _feeders.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L221](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L221)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L291)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L321)

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol

95:  for (uint256 i = 0; i < assets.length; i++) {

197:  for (uint256 i = 0; i < assets.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L95)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L197)

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

193: for (uint256 index = 0; index < tokenIds.length; index++) {

210:  for (uint256 index = 0; index < tokenIds.length; index++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L193)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L210)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol

38:  for (i = 0; i < params.nftTokenIds.length; i++) {

56:   for (i = 0; i < params.nftTokenIds.length; i++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L38)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L56)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

177:  for (uint256 i = 0; i < marketplaceIds.length; i++) {

284:  for (uint256 i = 0; i < marketplaceIds.length; i++) {

382:  for (uint256 i = 0; i < params.orderInfo.consideration.length; i++) {

470:   for (uint256 i = 0; i < params.orderInfo.offer.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L177)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L284)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L382)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L470)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

104:  for (uint256 i = 0; i < assets.length; i++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L104)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

184:  for (uint256 index = 0; index < params.tokenData.length; index++) {

195:   for (uint256 index = 0; index < params.tokenData.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L184)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L195)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

212:  for (uint256 index = 0; index < tokenIds.length; index++) {

465:  for (uint256 index = 0; index < tokenIds.length; index++) {

910:  for (uint256 index = 0; index < tokenIds.length; index++) {

1034:  for (uint256 i = 0; i < params.nftTokenIds.length; i++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L212)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L465)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L910)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1034)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

72:  for (uint256 index = 0; index < _nfts.length; index++) {

103: for (uint256 index = 0; index < _nfts.length; index++) {

138:  for (uint256 index = 0; index < _nftPairs.length; index++) {

199:  for (uint256 index = 0; index < _nftPairs.length; index++) {

215:  for (uint256 index = 0; index < _nftPairs.length; index++) {

279:  for (uint256 index = 0; index < _nfts.length; index++) {

289:   for (uint256 index = 0; index < _nftPairs.length; index++) {

305:   for (uint256 index = 0; index < _nftPairs.length; index++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L103)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L138)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L199)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L215)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L279)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L289)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L305)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

493:  for (uint256 index = 0; index < tokenIds.length; index++) {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L493)


```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

106:  for (uint256 index = 0; index < tokenIds.length; index++) {

145:   for (uint256 i = 0; i < ids.length; i++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L106)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L145)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

107:  for (uint256 index = 0; index < tokenIds.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L107)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

51:   for (uint256 index = 0; index < tokenIds.length; index++) {

97:   for (uint256 index = 0; index < tokenIds.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L51)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L97)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

207:  for (uint256 index = 0; index < tokenData.length; index++) {

280:   for (uint256 index = 0; index < tokenIds.length; index++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L207)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L280)

```
File:  2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

82:  for (uint256 i = 0; i < punkIndexes.length; i++) {

109:  for (uint256 i = 0; i < punkIndexes.length; i++) {

113:   for (uint256 i = 0; i < punkIndexes.length; i++) {

136:  for (uint256 i = 0; i < punkIndexes.length; i++) {

174:  for (uint256 i = 0; i < punkIndexes.length; i++) {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L82)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L109)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L113)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L136)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L174)

[6]  USAGE OF ``UINTS/INTS`` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

*There are 92 instances of this issue*

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

9:  uint8 constant MIN_ORACLES_NUM = 3;

12: uint128 constant EXPIRATION_PERIOD = 1800;

14:  uint128 constant MAX_DEVIATION_RATE = 150;

18:  uint128 expirationPeriod;

20:  uint128 maxPriceDeviation;

36:  uint8 index;

47:  uint8 index;

284:   assetFeederMap[_asset].index = uint8(assets.length - 1);

300:  uint8 assetIndex = assetFeederMap[_asset].index;

343:  function _setConfig(uint128 _expirationPeriod, uint128 _maxPriceDeviation)
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L18](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L18)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L20](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L20)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L36](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L36)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L47](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L47)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L284](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L284)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L300](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L300)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L343](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L343)

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

43:  uint8 token0Decimal;

44:  uint8 token1Decimal;

45:  uint160 sqrtPriceX96;

61:  uint24 fee,

62: int24 tickLower,

63:  int24 tickUpper,

64:  uint128 liquidity,

74:  (uint160 currentPrice, int24 currentTick, , , , , ) = pool.slot0();

243:  oracleData.sqrtPriceX96 = uint160(

251:  oracleData.sqrtPriceX96 = uint160(

263:  oracleData.sqrtPriceX96 = uint160(

362:  token0Amount = uint128(

371:  token1Amount = uint128(
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L43-L45](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L43-L45)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L61-L64](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L61-L64)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L74](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L74)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L243](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L243)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L251](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L251)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L263](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L263)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L362](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L362)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L371](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol#L371)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol

33:  uint16 indexed referralCode
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L33](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol#L33)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

125:  uint16 liquidationAssetReserveId;
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L125](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L125)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

69:  uint16 referralCode

166:   uint16 referralCode

236:   uint16 referralCode

274:  uint16 referralCode
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L69](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L69)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L166](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L166)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L236](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L236)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L274](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L274)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

58:  for (uint16 i = 0; i < params.reservesCount; i++) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L58)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

48:  uint16 indexed referralCode

61:  uint16 indexed referralCode,

135:  uint16 reserveId,

144:  uint64 oldCollateralizedBalance,

145:  uint64 newCollateralizedBalance

356:  uint64 oldCollateralizedBalance,

357:  uint64 newCollateralizedBalance
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L48](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L48)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L61](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L61)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L135](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L135)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L144](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L144)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L145)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L356](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L356)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L357](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L357)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol

95:  uint16 referralCode

117:  uint16 referralCode

160:   uint16 referralCode,

240:   uint128 liquidityDecrease,

270:  uint16 referralCode,

650:  function getReserveAddressById(uint16 id) external view returns (address) {

662:  returns (uint16)

673:   returns (uint64)

162:  uint8 permitV,

343:  uint8 permitV,
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L95](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L95)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L117](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L117)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L160](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L160)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L240](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L240)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L270](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L270)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L650](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L650)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L662](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L662)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L673](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L673)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L162](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L162)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L343](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L343)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol

75:  uint16 referralCode

94:  uint16 referralCode

114:  uint16 referralCode

135:  uint16 referralCode
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L75](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L75)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L94](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L94)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L114](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L114)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L135](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolMarketplace.sol#L135)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol

206:  function setAuctionRecoveryHealthFactor(uint64 value)
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

142:  function setBalanceLimit(uint64 limit) external onlyPoolAdmin {

371:   uint64 oldCollateralizedBalance,

372:  uint64 newCollateralizedBalance

388:  uint64 oldCollateralizedBalance,

389:   uint64 newCollateralizedBalance
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L371-L372](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L371-L372)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L388-L389](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L388-L389)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

82: ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

91:  ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

99:    ) internal returns (uint64, uint64) {

101:    uint64 oldCollateralizedBalance,

102:  uint64 newCollateralizedBalance
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L82](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L82)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L91](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L91)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L99](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L99)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L101-L102](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L101-L102)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

106:    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L106)

```
File:   2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

44:  ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

46:   uint64 oldCollateralizedBalance,

47:  uint64 newCollateralizedBalance
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L44](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L44)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L46-L47](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L46-L47)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

54:  uint128 liquidityDecrease,

100:    amount0Max: type(uint128).max,

101:    amount1Max: type(uint128).max

126:   uint128 liquidityDecrease,
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L54](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L54)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L100-L101](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L100-L101)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L126](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L126)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

13:  uint64 balance;

14:  uint64 collateralizedBalance;

15:   uint128 additionalData;

42:  uint64 balanceLimit;

103:    uint64 oldSenderBalance = erc721Data.userState[from].balance;

105:   uint64 oldRecipientBalance = erc721Data.userState[to].balance;

106:  uint64 newRecipientBalance = oldRecipientBalance + 1;

173:   uint64 collateralizedBalance = erc721Data

195:  uint64 oldCollateralizedBalance,

196:   uint64 newCollateralizedBalance

200:  uint64 oldBalance = erc721Data.userState[to].balance;

205:    uint64 collateralizedTokens = 0;

247:    uint64 newBalance = oldBalance + uint64(tokenData.length);

268:    uint64 oldCollateralizedBalance,

269:     uint64 newCollateralizedBalance

272:  uint64 burntCollateralizedTokens = 0;

273:   uint64 balanceToBurn;

405:  uint64 balance

408:   uint64 balanceLimit = erc721Data.balanceLimit;
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L13-L15](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L13-L15)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L42](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L42)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L103](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L103)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L105-L106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L105-L106)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L173](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L173)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L195-L196](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L195-L196)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L200](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L200)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L205](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L205)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L247](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L247)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L268-L269](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L268-L269)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272-L273](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L272-L273)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L405](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L405)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L408](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L408)

```
File:  2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

80:  uint16 referralCode

134:   uint16 referralCode

172:   uint16 referralCode

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L80](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L80)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L134](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L134)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L172)


[7]  FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED ``PAYABLE``

If a function modifier such as ``onlyOwner`` is used, the function will revert if a normal user tries to pay the function. Marking the function as ``payable`` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are ``CALLVALUE``(2),``DUP1``(3),``ISZERO``(3),``PUSH``2(3),``JUMPI``(10),``PUSH1``(3),``DUP1``(3),``REVERT``(0),``JUMPDEST``(1),``POP``(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

*There are 55 instances of this issue:*

```
File:  2022-11-paraspace/paraspace-core/contracts/misc/ParaSpaceOracle.sol

66-69:  function setAssetSources(
        address[] calldata assets,
        address[] calldata sources
    ) external override onlyAssetListingOrPoolAdmins {

74-77:   function setFallbackOracle(address fallbackOracle)
        external
        override
        onlyAssetListingOrPoolAdmins
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L66-L69](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L66-L69)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L74-L77](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ParaSpaceOracle.sol#L74-L77)

```
File:   2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

56-59:  function setMarketId(string memory newMarketId)
        external
        override
        onlyOwner

70-73:  function setAddress(bytes32 id, address newAddress)
        external
        override
        onlyOwner

81-84:  function setAddressAsProxy(bytes32 id, address newImplementationAddress)
        external
        override
        onlyOwner

105-109:  function updatePoolImpl(
        IParaProxy.ProxyImplementation[] calldata implementationParams,
        address _init,
        bytes calldata _calldata
    ) external override onlyOwner {

121-124:  function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)
        external
        override
        onlyOwner

142-145:  function setPriceOracle(address newPriceOracle)
        external
        override
        onlyOwner

158:  function setACLManager(address newAclManager) external override onlyOwner {

170: function setACLAdmin(address newAclAdmin) external override onlyOwner {

182-185:  function setPriceOracleSentinel(address newPriceOracleSentinel)
        external
        override
        onlyOwner

224-227:  function setProtocolDataProvider(address newDataProvider)
        external
        override
        onlyOwner

235:  function setWETH(address newWETH) external override onlyOwner {

242-248:  function setMarketplace(
        bytes32 id,
        address marketplace,
        address adapter,
        address operator,
        bool paused
    ) external override onlyOwner {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56-L59](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L56-L59)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70-L73](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L70-L73)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L81-L84](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L81-L84)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L105-L109](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L105-L109)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L121-L124](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L121-L124)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142-L145](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L142-L145)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182-L185](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L182-L185)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224-L227](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L224-L227)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242-L248](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L242-L248)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol

110-116:  function initReserve(
        address asset,
        address xTokenAddress,
        address variableDebtAddress,
        address interestRateStrategyAddress,
        address auctionStrategyAddress
    ) external virtual override onlyPoolConfigurator {

139-143:  function dropReserve(address asset)
        external
        virtual
        override
        onlyPoolConfigurator

151-154:  function setReserveInterestRateStrategyAddress(
        address asset,
        address rateStrategyAddress
    ) external virtual override onlyPoolConfigurator {

166-169:  function setReserveAuctionStrategyAddress(
        address asset,
        address auctionStrategyAddress
    ) external virtual override onlyPoolConfigurator {

181-184:  function setConfiguration(
        address asset,
        DataTypes.ReserveConfigurationMap calldata configuration
    ) external virtual override onlyPoolConfigurator {

206-210:  function setAuctionRecoveryHealthFactor(uint64 value)
        external
        virtual
        override
        onlyPoolConfigurator

196-201:   function rescueTokens(
        DataTypes.AssetType assetType,
        address token,
        address to,
        uint256 amountOrTokenId
    ) external virtual override onlyPoolAdmin {

```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L110-L116](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L110-L116)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139-L143](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L139-L143)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151-L154](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L151-L154)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166-L169](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L166-L169)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181-L184](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L181-L184)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206-L210](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L206-L210)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L196-L201](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L196-L201)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol

131-133:  function setIncentivesController(IRewardController controller)
        external
        onlyPoolAdmin

142:  function setBalanceLimit(uint64 limit) external onlyPoolAdmin {

458-462:  function setIsUsedAsCollateral(
        uint256 tokenId,
        bool useAsCollateral,
        address sender
    ) external virtual override onlyPool nonReentrant returns (bool) {

474-483:  function batchSetIsUsedAsCollateral(
        uint256[] calldata tokenIds,
        bool useAsCollateral,
        address sender
    )
        external
        virtual
        override
        onlyPool
        nonReentrant


529-534:  function startAuction(uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant

540-545:  function endAuction(uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131-L133](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L131-L133)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L142)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458-L462](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L458-L462)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L474-L483](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L474-L483)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L529-L534](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L529-L534)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L540-L545](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol#L540-L545)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

79-82:  function mint(
        address onBehalfOf,
        DataTypes.ERC721SupplyParams[] calldata tokenData
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

87-91:  function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

119-123: function transferOnLiquidation(
        address from,
        address to,
        uint256 value
    ) external onlyPool nonReentrant {


127-131:  function rescueERC20(
        address token,
        address to,
        uint256 amount
    ) external override onlyPoolAdmin {


136-140:  function rescueERC721(
        address token,
        address to,
        uint256[] calldata ids
    ) external override onlyPoolAdmin {


151-157:  function rescueERC1155(
        address token,
        address to,
        uint256[] calldata ids,
        uint256[] calldata amounts,
        bytes calldata data
    ) external override onlyPoolAdmin {


168-171:  function executeAirdrop(
        address airdropContract,
        bytes calldata airdropParams
    ) external override onlyPoolAdmin {

199-204:  function transferUnderlyingTo(address target, uint256 tokenId)
        external
        virtual
        override
        onlyPool
        nonReentrant

214-219:  function handleRepayment(address user, uint256 amount)
        external
        virtual
        override
        onlyPool
        nonReentrant
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L79-L82](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L79-L82)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L87-L91](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L87-L91)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L119-L123](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L119-L123)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L127-L131](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L127-L131)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L136-L140](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L136-L140)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L151-L157](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L151-L157)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L168-L171](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L168-L171)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L199-L204](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L199-L204)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L214-L219](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L214-L219)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

102-106: function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {

136:  function setUnstakeApeIncentive(uint256 incentive) external onlyPoolAdmin {

159-162:  function unstakePositionAndRepay(uint256 tokenId, address incentiveReceiver)
        external
        onlyPool
        nonReentrant
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102-L106](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L102-L106)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L136](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L136)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L159-L162](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L159-L162)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

26-29:  function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant

39-42:  function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant

52-55:  function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {

66-69:  function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant

82-85:  function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {

97-100:  function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L26-L29](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L26-L29)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L39-L42](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L39-L42)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L52-L55](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L52-L55)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L66-L69](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L66-L69)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L82-L85](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L82-L85)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97-L100](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol#L97-L100)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

26-29:  function depositApeCoin(ApeCoinStaking.SingleNft[] calldata _nfts)
        external
        onlyPool
        nonReentrant

39-42:  function claimApeCoin(uint256[] calldata _nfts, address _recipient)
        external
        onlyPool
        nonReentrant

52-55:  function withdrawApeCoin(
        ApeCoinStaking.SingleNft[] calldata _nfts,
        address _recipient
    ) external onlyPool nonReentrant {

66-69:  function depositBAKC(ApeCoinStaking.PairNftWithAmount[] calldata _nftPairs)
        external
        onlyPool
        nonReentrant

82-85:   function claimBAKC(
        ApeCoinStaking.PairNft[] calldata _nftPairs,
        address _recipient
    ) external onlyPool nonReentrant {

97-100:  function withdrawBAKC(
        ApeCoinStaking.PairNftWithAmount[] memory _nftPairs,
        address _apeRecipient
    ) external onlyPool nonReentrant {
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26-L29](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L26-L29)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L39-L42](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L39-L42)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L52-L55](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L52-L55)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L66-L69](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L66-L69)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L82-L85](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L82-L85)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-L100](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol#L97-L100)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

40-44:  function burn(
        address from,
        address receiverOfUnderlying,
        uint256[] calldata tokenIds
    ) external virtual override onlyPool nonReentrant returns (uint64, uint64) {
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40-L44](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L40-L44]

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol

123-130:  function decreaseUniswapV3Liquidity(
        address user,
        uint256 tokenId,
        uint128 liquidityDecrease,
        uint256 amount0Min,
        uint256 amount1Min,
        bool receiveEthAsWeth
    ) external onlyPool nonReentrant {

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123-L130](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol#L123-L130)

```
File: 2022-11-paraspace/paraspace-core/contracts/ui/WPunkGateway.sol

202-206:  function emergencyERC721TokenTransfer(
        address token,
        uint256 tokenId,
        address to
    ) external onlyOwner {

217-219:  function emergencyPunkTransfer(address to, uint256 punkIndex)
        external
        onlyOwner
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L202-L206](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L202-L206)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L217-L219](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol#L217-L219)

[8] USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS

When a function with a ``memory`` array is called externally, the ``abi.decode()`` step has to use a for-loop to copy each index of the ``calldata`` to the ``memory`` index. Each iteration of this for-loop costs at least 60 gas (i.e. ``60 * <mem_array>.length``). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having ``memory`` arguments, it’s still valid for implementation contracs to use ``calldata`` arguments instead.

If the array is passed to an ``internal`` function which passes the array to another internal function where the array is modified and therefore ``memory`` is used in the ``external`` call, it’s still more gass-efficient to use ``calldata`` when the ``external`` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I’ve also flagged instances where the function is ``public`` but can be marked as ``external`` since it’s not called by the contract, and cases where a constructor is involved

*There are 13 instances of this issue.*

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

165:  DataTypes.ExecuteLiquidateParams memory params

290:  DataTypes.ExecuteLiquidateParams memory params
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L165](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L165)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L290](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L290)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

42:  DataTypes.InitReserveParams memory params

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L42](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol#L42)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

84:  DataTypes.ExecuteSupplyParams memory params

170:  DataTypes.ExecuteSupplyERC721Params memory params

231:  DataTypes.ExecuteSupplyERC721Params memory params

274: DataTypes.ExecuteWithdrawParams memory params

339:  DataTypes.ExecuteWithdrawERC721Params memory params

400:  DataTypes.ExecuteDecreaseUniswapV3LiquidityParams memory params

466:  DataTypes.FinalizeTransferParams memory params

527:  DataTypes.FinalizeTransferERC721Params memory params
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L84](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L84)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L170](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L170)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L231](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L231)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L274](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L274)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L339](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L339)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L400](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L400)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L466](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L466)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L527](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol#L527)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

122: ApeCoinStaking.PairNftWithAmount[] memory _nftPairs
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L122](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L122)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol

67:  bytes memory
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L67](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol#L67)

[9] USING  ``PRIVATE`` RATHER THAN ``PUBLIC`` FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

*There are 13 instances of this issue:*

```
File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

9: uint8 constant MIN_ORACLES_NUM = 3;

12:  uint128 constant EXPIRATION_PERIOD = 1800;

14:   uint128 constant MAX_DEVIATION_RATE = 150;

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L9)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L12)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L14)

```
File:   2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol

57:  uint256 public constant MAX_LIQUIDATION_CLOSE_FACTOR = 1e4;

64:    uint256 public constant CLOSE_FACTOR_HF_THRESHOLD = 0.95e18;
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L57](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L57)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L64](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L64)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

45: uint256 public constant REBALANCE_UP_LIQUIDITY_RATE_THRESHOLD = 0.9e4;

49-50:  uint256 public constant MINIMUM_HEALTH_FACTOR_LIQUIDATION_THRESHOLD =
        0.95e18;

56: uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L45)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L49-L50](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L49-L50)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L56](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L56)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolCore.sol

53:  uint256 public constant POOL_REVISION = 1;

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L53](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolCore.sol#L53)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolStorage.sol

16:  bytes32 constant POOL_STORAGE_POSITION =
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolStorage.sol#L16)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

32:  uint256 public constant NTOKEN_REVISION = 0x1;
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L32](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L32)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol

23:  bytes32 constant APE_STAKING_DATA_STORAGE_POSITION =

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L23)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

22-24: uint256 constant BAYC_POOL_ID = 1;
    uint256 constant MAYC_POOL_ID = 2;
    uint256 constant BAKC_POOL_ID = 3;
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L22-L24](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol#L22-L24)

[10] USE ASSEMBLY TO CHECK FOR ADDRESS(0) to save gas.

*There are 13 instances of this issue:*

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol

268: require(newAddress != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L268](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L268)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

440:  require(vars.xTokenAddress != address(0), Errors.ASSET_NOT_LISTED);
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L440](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L440)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

771-772: require(
            collateralReserve.auctionStrategyAddress != address(0),

975: require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

1004-1005:  require(
            params.receiverAddress != address(0),
```

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L771-L772](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L771-L772)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L975](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L975)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1004-L1005](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1004-L1005)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/pool/PoolParameters.sol

157:  require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

172:  require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

187:  require(asset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

271:  require(user != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L157)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L172)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L187)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L271](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/pool/PoolParameters.sol#L271)

```
File:  2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/NToken.sol

64:  require(underlyingAsset != address(0), Errors.ZERO_ADDRESS_NOT_VALID);

172-173:   require(
            airdropContract != address(0),
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L64](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L64)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L172-L173](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NToken.sol#L172-L173)

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

92:   require(to != address(0), "ERC721: transfer to the zero address");

199:  require(to != address(0), "ERC721: mint to the zero address");
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L92)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L199](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol#L199)

[11] ``ABI.ENCODE()`` IS LESS EFFICIENT THAN ``ABI.ENCODEPACKED()``

*There are 2 instances of this issue:*

```
File: 2022-11-paraspace/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

1124:  abi.encode(

1136:  abi.encode(
```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1124](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1124)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1136](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol#L1136)

12]   DIVISION   BY TWO SHOULD USE BIT SHIFTING

``<x> / 2`` is the same as ``<x> >>1``. The ``DIV`` opcode costs 5 gas, whereas ``SHR`` only costs 3 gas

*There are 2 instances of this issue:*

```
File: 2022-11-paraspace/paraspace-core/contracts/misc/NFTFloorOracle.sol

429:  return (true, validPriceList[validNum / 2]);

440:  uint256 pivot = arr[uint256(left + (right - left) / 2)];

```
[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L429](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L429)

[https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L440](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol#L440)
































 































































































































 














