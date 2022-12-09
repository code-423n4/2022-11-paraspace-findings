# [L-1] Lack zero address check.
There need some address zero check to protect user from accidently losing funds in the `WETHGateway` contract.
```solidity
function repayETH(uint256 amount, address onBehalfOf)
```
The `onBehalfOf` address should be checked if it is zero.

```solidity
function repayETH(uint256 amount, address onBehalfOf)
```
The `onBehalfOf` address should be checked if it is zero.

```solidity
function withdrawETHWithPermit(
        uint256 amount,
        address to,
        uint256 deadline,
        uint8 permitV,
        bytes32 permitR,
        bytes32 permitS
    ) external override nonReentrant {
```
The `to` address should be checked if it is zero.

There are three locations:
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WETHGateway.sol#L66
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WETHGateway.sol#L92
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WETHGateway.sol#L146

# [N-01] meaningless revert string
Revert string should clearly describe the revert reason.
```solidity
    receive() external payable {
        //only contracts can send ETH to the core
        require(msg.sender.isContract(), "22");
    }
```
* https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WalletBalanceProvider.sol#L35

# [L-02] `ParaReentrancyGuard` storage variable isn't initialized in `PoolApeStaking` and `PoolMarketplace`.
The construtor in `ParaReentrancyGuard` was commentted out.

```solidity
    // constructor() {
    //     _status = _NOT_ENTERED;
    // }
```
This indicates that contract inherited from `ParaReentrancyGuard` must initialize the `_status` variable itself. The `PoolCore.sol` contract initialize it in the `initialize` function

```solidity
function initialize(IPoolAddressesProvider provider)
        external
        virtual
        initializer
    {
        require(
            provider == ADDRESSES_PROVIDER,
            Errors.INVALID_ADDRESSES_PROVIDER
        );

        RGStorage storage rgs = rgStorage();

        rgs._status = _NOT_ENTERED;
    }
```

But we didn't find it is initialized like `PoolCore` do in the `PoolApeStaking` and `PoolMarketplace` contract. 

The impact is that `ParaReentrancyGuard`'s usage doesn't follow design spec, which could result in unexpected behavior in the future.
### recommendation: 
Either
1. initialize the `_status` variable in the construtor of `ParaReentrancyGuard`
or
2. initilize the `_status` variable like `PoolCore` do in the `PoolApeStaking` and `PoolMarketplace` contract.
