## Unhandled return value of transfer in WETHGateway.sol
[WETHGateway.sol#L81](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WETHGateway.sol#L81)

ERC20 implementations are not always consistent. Some implementations of transfer and transferFrom could return ‘false’ on failure instead of reverting. It is safer to wrap such calls into require() statements to handle these failures.
The transfer call on L81 of `withdrawEthWithPermit()` could be made on a user-supplied untrusted token address (from the different call sites) whose implementation can be malicious.
## Mitigation Steps
Check the return value and revert on 0/false, or use OpenZeppelin’s SafeERC20 wrapper functions.
