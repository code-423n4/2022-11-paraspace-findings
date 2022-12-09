## Unhandled return value of approve in NTokenApeStaking.sol

[NTokenApeStaking.sol#L45-L46](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/NTokenApeStaking.sol#L45-L46).

ERC20 implementations are not always consistent. Some implementations of `approve` return ‘false’ on failure instead of reverting. It is safer to wrap such calls into require() statements to handle these failures.
Some functions may not notice the error, not approve anything and such implementation can be malicious.

## Mitigation Steps
Check the return value and revert on 0/false, or use OpenZeppelin’s SafeERC20 wrapper functions.
