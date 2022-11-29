##

##   [GAS -1]    instead of using operator && on single require or if checks . Using Multiple REQUIRE or IF checks can save more gas. 


          2022-11-paraspace/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

           require(
            // NOT criteria based and must be basic order
            resolvers.length == 0 && isBasicOrder(advancedOrder),
            Errors.INVALID_MARKETPLACE_ORDER
            );


          require(
            // NOT criteria based and must be basic order
            advancedOrders.length == 2 &&          
                isBasicOrder(advancedOrders[0]) &&  
                isBasicOrder(advancedOrders[1]),
            Errors.INVALID_MARKETPLACE_ORDER
        );

[Link To Code](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol)

## 












-------------------------------------------------------------------------------------------------------------------------------------------------------

GAS-1	Use assembly to check for address(0)	40
GAS-2	Using bools for storage incurs overhead	3
GAS-3	Cache array length outside of loop	45
GAS-4	State variables should be cached in stack variables rather than re-reading them from storage	4
GAS-5	Use calldata instead of memory for function arguments that do not get mutated	39
GAS-6	Use Custom Errors	16
GAS-7	Don't initialize variables with default value	59
GAS-8	++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)	58
GAS-9	Using private rather than public for constants, saves gas	8
GAS-10	Use shift Right/Left instead of division/multiplication if possible	2
GAS-11	Use != 0 instead of > 0 for unsigned integer comparison	23
GAS-12	internal functions not called by the contract should be removed	24