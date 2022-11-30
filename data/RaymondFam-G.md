## Unused Constructor
Empty constructor() that is not used can be removed to save deployment gas. 

Here are the instances entailed:

[File: LooksRareAdapter.sol#L23](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L23)

```
    constructor() {}
```
## Use Storage Instead of Memory for Structs/Arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct. 

Here are the instances entailed:

[File: LooksRareAdapter.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol)

```
32:            OrderTypes.TakerOrder memory takerBid,
33:            OrderTypes.MakerOrder memory makerAsk

37:        OfferItem[] memory offer = new OfferItem[](1);

47:        ConsiderationItem[] memory consideration = new ConsiderationItem[](1);
```
## Ternary Over if ... else
Using ternary operator instead of the if else statement saves gas. 

For instance, the code block below may be refactored as follows:

[LooksRareAdapter.sol#L79-L85](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L79-L85)

```
    value == 0
        ? {
            selector = ILooksRareExchange.matchAskWithTakerBid.selector;
        } 
        : {
            selector = ILooksRareExchange
                .matchAskWithTakerBidUsingETHAndWETH
                .selector;
        }
```
## Use of Named Returns for Local Variables Saves Gas
You can have further advantages in term of gas cost by simply using named return values as temporary local variable.

As an example, the following instance of code block can refactored as follows:

[File: LooksRareAdapter.sol#L73-L94](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L73-L94)

```
    function matchAskWithTakerBid(
        address marketplace,
        bytes calldata params,
        uint256 value
    ) external payable override returns (bytes memory _data) {
       
        ...

        _data =
            Address.functionCallWithValue(
                marketplace,
                data,
                value,
                Errors.CALL_MARKETPLACE_FAILED
            );
    }
```
## Merging of Identical Imports
In `LooksRareAdapter.sol`, the following two lines of imports originate from the same `ConsiderationStructs.sol`:

[File: LooksRareAdapter.sol#L10-L11](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol#L10-L11)

```
import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```
They may be merged into one import as follows:

```
import {ConsiderationItem, AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
``` 