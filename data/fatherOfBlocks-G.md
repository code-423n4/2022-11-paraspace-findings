**paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol**
- L51/55/84/88 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L41/51/54/71/84/87 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol**
- L38/44/51 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**paraspace-core/contracts/misc/NFTFloorOracle.sol**
- L118/123/128/133/207/225/243/267/356 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L356/417 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L227/356 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

- L201/291/321/411 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L226/229/291/321 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L331 - It is validated that feederIndex is >= 0, being a uint8, this is unnecessary since it will always return true. Therefore it should be removed.


**paraspace-core/contracts/misc/ParaSpaceOracle.sol**
- L92/95/196/197 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L95/125/197 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L91/96/134/222 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L127/130/148/165/182 - Use assembly to check for address(0), Saves 6 gas per instance.


**paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol**
- L191/193/210 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L193/208/210 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.


**paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol**
- L214/268/276/312/344 - Use assembly to check for address(0), Saves 6 gas per instance.


**paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol**
- L78 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.


**paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol**
- L38/56 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L38/56 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.


**paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol**
- L371/466 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.


**paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol**
- L172/173/177/280/281/284/382/470 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L177/284/380/382/470 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L570/580 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L171/245/279/294/384/388/393/412/440/472/489/494 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol**
- L45/56/66 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L58/104 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L58/104 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.


**paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol**
- L148 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L184/195 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L184/195 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.


**paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol**
- L68/71/84/85/86/92/102/123/133/140/160/162/168/181/185/186/203/207/208/258/269/273/274/275/276/278/306/390-392/515-578/636-690/757-795/815-833/975-984/1000-1011/1057-1059/1196-1208 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L131/212/465/910/1034 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L212/465/910/1034 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.


**paraspace-core/contracts/protocol/pool/PoolApeStaking.sol**
- L71/72/103/135/137/138/172/199/215/279/289/305 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L72/103/138/199/215/279/289/305 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L260/270/317/385/398 - It is less expensive to validate that uint != 0 than to validate uint > 0


**paraspace-core/contracts/protocol/pool/PoolCore.sol**
- L80/692/726/761/803 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L631/634 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.


**paraspace-core/contracts/protocol/pool/PoolParameters.sol**
- L70/77/157/158/172/173/187/188/214/216/271/281 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.



**paraspace-core/contracts/protocol/tokenization/base/MintableIncentivizedERC721.sol**
- L49/60/193/195/213/259/296/354/594/618 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L197/215/261/298/356/596/620 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**paraspace-core/contracts/protocol/tokenization/NToken.sol**
- L60/64/141/172/176 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L106/145 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L106/145 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.


**paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol**
- L107 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L107 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.


**paraspace-core/contracts/protocol/tokenization/NTokenMoonBirds.sol**
- L51/97 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L51/97 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- 100 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**paraspace-core/contracts/protocol/tokenization/NTokenUniswapV3.sol**
- L59/109/116 - It is less expensive to validate that uint != 0 than to validate uint > 0


**paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol**
- 91/93/380/398 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

- L89/93/94/165/168/200/211/284/285/364/374/378/392/396/410 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L112/308 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L206/208/273/281 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L208/281 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.


**paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol**
- L108/128/164/182 - It is less expensive to validate that uint != 0 than to validate uint > 0


**paraspace-core/contracts/ui/WPunkGateway.sol**
- L82/109/113/136/174 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value.

- L82/109/113/136/174 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L82/109/113/136/174 - It is less expensive to do ++i or --i, rather than i++, i-- or i - 1.
