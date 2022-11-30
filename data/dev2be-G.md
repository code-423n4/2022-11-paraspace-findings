#  COMBINE SMALL BYTE VARIABLES TO REDUCE EVM SLOTS BY PLACING THEM IN THE RIGHT READ ORDER (2 INSTANCES)

Write variables in byte order to use less EVM slots. address = 160bytes, you can combine it with any variable that is less than 96bytes. Like uint16 or bool. In or order to reduce gas price.

FOR EXAMPLE:

* address
* address
* address
* uint16
* uint256
* uint256

[.logic/BorrowLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/BorrowLogic.sol)


        event Borrow(
        address indexed reserve,
        address user,
        address indexed onBehalfOf,
        uint256 amount,
        uint256 borrowRate,
        uint16 indexed referralCode
        );

        event Repay(
        address indexed reserve,
        address indexed user,
        address indexed repayer,
        uint256 amount,
        bool usePTokens
        );


       