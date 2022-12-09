#QA Report

## Low Risk

[LR-01] MintableIncentivizedERC721 onlyPoolAdmin modifier should use \_msgSender() instead of msg.sender

```solidity
    modifier onlyPoolAdmin() {
        IACLManager aclManager = IACLManager(
            _addressesProvider.getACLManager()
        );
        require(
            aclManager.isPoolAdmin(msg.sender),
            Errors.CALLER_NOT_POOL_ADMIN
        );
        _;
    }
```