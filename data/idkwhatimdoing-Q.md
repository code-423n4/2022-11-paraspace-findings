  Avoid leaving a contract uninitialized.
 
  An uninitialized contract can be taken over by an attacker. This applies to both a proxy and its implementation
  contract, which may impact the proxy. To prevent the implementation contract from being used, you should invoke
  the {_disableInitializers} function in the constructor to automatically lock it when it is deployed:


https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/ui/WPunkGateway.sol#L57

https://github.com/code-423n4/2022-11-paraspace/blob/3e4e2e4e1322482dcdc6d342f8799cd44a3e42f5/paraspace-core/contracts/ui/WETHGateway.sol#L39