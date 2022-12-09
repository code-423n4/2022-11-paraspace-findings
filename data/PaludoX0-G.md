#### Require to be called before storage pointer initialization
*https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol#L123*
Require check to be call before ds pointer definition to save gas if require reverts 
        
        ProxyStorage storage ds = diamondStorage();
        require(_implementationAddress != address(0), "ParaProxy: Add implementation can't be address(0)");


#### Lack of parameters validation at the beginning of the function
In PoolCore there's a general lack of parameters validation at the beginning of functions. I'll not  list all occurences, but please check the following one as an example.
Function supplyERC721 *(contracts/protocol/pool/PoolCore.sol#L113)* checks only at the end of the calls chain if `address onBehalfOf !=0`
These are the nested calls:
*PoolCore.supplyERC721 ->SupplyLogic.executeSupplyERC721-> SupplyLogic.executeSupplyERC721 -> Ntoken.mint ->
MintableIncentivizedERC721._mintMultiple -> MintableERC721Logic.executeMintMultiple*

Only at last step there's the check `require(to != address(0), "ERC721: mint to the zero address");` 


#### Use of unchecked keyword
*https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolApeStaking.sol#L72*

        for (uint256 index = 0; index < _nfts.length; index++)

There are many instances like the one above where you could use unchecked keyword. 
In this case the loop will run out of gas before going overflow.


#### Struct type variables initialization
*https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/paraspace-upgradeability/lib/ParaProxyLib.sol#L237*
The way a variable of type Struct is initialize change the gas used.
I tried 3 different approaches and tested in Remix (with optimization 200) to initialize the variable at ParaProxyLib.sol#L237

The results that the most efficient is the following:

         ds.selectorToImplAndPosition[_selector] = ImplementationAddressAndPosition({
                functionSelectorPosition: _selectorPosition,
                implAddress : _implementationAddress });`

Instead of:

         ds.selectorToImplAndPosition[_selector] 
             .functionSelectorPosition = _selectorPosition;
         ds.selectorToImplAndPosition[_selector]
             .implAddress = _implementationAddress;
        
You can try by coping following snippet in Remix.
For your perusual I've used the same address for each function and the following input data:
- addfunctions  _functionSelectors: ["0x12345678","0x23456789"]
- addfunctions2 _functionSelectors: ["0x32345678","0x43456789"]
- addfunctions3 _functionSelectors: ["0x52345678","0x63456789"]

The first function consumed **113999gas**, the second (the best one) **79591gas**, the third **79613gas**


    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.10;
    import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    contract MyToken {
    struct ImplementationAddressAndPosition {
        address implAddress;
        uint96 functionSelectorPosition; // position in implementationFunctionSelectors.functionSelectors array
    }
    struct ImplementationFunctionSelectors {
        bytes4[] functionSelectors;
        uint256 implementationAddressPosition; // position of implAddress in implementationAddresses array
    }
    struct ProxyStorage {
        mapping(bytes4 => ImplementationAddressAndPosition) selectorToImplAndPosition;
        mapping(address => ImplementationFunctionSelectors) implementationFunctionSelectors;
        address[] implementationAddresses;
        mapping(bytes4 => bool) supportedInterfaces;
        address contractOwner;
    }
    ProxyStorage ds;

    function addFunctions(
        address _implementationAddress,
        bytes4[] memory _functionSelectors
    ) external {
        uint96 selectorPosition = uint96(
            ds
                .implementationFunctionSelectors[_implementationAddress]
                .functionSelectors
                .length
        );
        for (
            uint256 selectorIndex;
            selectorIndex < _functionSelectors.length;
            selectorIndex++
        ) {
            bytes4 selector = _functionSelectors[selectorIndex];
            addFunction(ds, selector, selectorPosition, _implementationAddress);
            selectorPosition++;
        }
    }
    function addFunction(
        ProxyStorage storage ds,
        bytes4 _selector,
        uint96 _selectorPosition,
        address _implementationAddress
    ) internal {
        ds
            .selectorToImplAndPosition[_selector] 
            .functionSelectorPosition = _selectorPosition;
        ds
            .implementationFunctionSelectors[_implementationAddress]
            .functionSelectors
            .push(_selector);
        ds
            .selectorToImplAndPosition[_selector]
            .implAddress = _implementationAddress;
    }
    function addFunctions2(
        address _implementationAddress,
        bytes4[] memory _functionSelectors
    ) external {
        uint96 selectorPosition = uint96(
            ds
                .implementationFunctionSelectors[_implementationAddress]
                .functionSelectors
                .length
        );
       for (
            uint256 selectorIndex;
            selectorIndex < _functionSelectors.length;
            selectorIndex++
        ) {
            bytes4 selector = _functionSelectors[selectorIndex];
            addFunction2(ds, selector, selectorPosition, _implementationAddress);
            selectorPosition++;
        }
    }
    function addFunction2(
        ProxyStorage storage ds,
        bytes4 _selector,
        uint96 _selectorPosition,
        address _implementationAddress
    ) internal {
        ds
            .selectorToImplAndPosition[_selector] = ImplementationAddressAndPosition({
                functionSelectorPosition: _selectorPosition,
                implAddress : _implementationAddress
            });
        ds
            .implementationFunctionSelectors[_implementationAddress]
            .functionSelectors
            .push(_selector);          
    }

    function addFunctions3(
        address _implementationAddress,
        bytes4[] memory _functionSelectors
    ) external {
        uint96 selectorPosition = uint96(
            ds
                .implementationFunctionSelectors[_implementationAddress]
                .functionSelectors
                .length
        );
        for (
            uint256 selectorIndex;
            selectorIndex < _functionSelectors.length;
            selectorIndex++
        ) {
            bytes4 selector = _functionSelectors[selectorIndex];
            addFunction3(ds, selector, selectorPosition, _implementationAddress);
            selectorPosition++;
        }
    }
    function addFunction3(
        ProxyStorage storage ds,
        bytes4 _selector,
        uint96 _selectorPosition,
        address _implementationAddress
    ) internal {
        ds
            .selectorToImplAndPosition[_selector] = ImplementationAddressAndPosition(_implementationAddress,_selectorPosition);
        ds
            .implementationFunctionSelectors[_implementationAddress]
            .functionSelectors
            .push(_selector);    
    } }


#### Use of an internal pure function for returning a PUBLIC constant
*https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/pool/PoolCore.sol#L56*
Check if you really need this INTERNAL pure function which returns a PUBLIC constant
This issue is recoursive in many contracts of this project.
Try the following snippet in Remix, calling `getterRevision2()` could save around20gas per call


    contract MyTest2 {
    uint256 public constant POOL_REVISION = 1;

    function getRevision() internal pure virtual  returns (uint256) {
        return POOL_REVISION;
    }

    function getterRevision1() external returns (uint256)
    {
        return getRevision();
    }

    function getterRevision2() external returns (uint256)
    {
        return POOL_REVISION;
    } }

