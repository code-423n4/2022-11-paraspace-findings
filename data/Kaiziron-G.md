## [G-01] Functions guaranteed to revert when called by normal users can be marked payable

For functions that are guaranteed to revert when called by normal users, such as having `onlyOwner` modifier which should only be called by the owner. Using the payable modifier can lower the gas usage for the intended caller, as it won't include additional checks for the value being sent, which results in fewer opcodes being executed and lower gas usage.

*There are 3 instances of this issue*
```
paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:158:    function setACLManager(address newAclManager) external override onlyOwner {
paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:170:    function setACLAdmin(address newAclAdmin) external override onlyOwner {
paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol:235:    function setWETH(address newWETH) external override onlyOwner {
```
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L158
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L170
https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol#L235