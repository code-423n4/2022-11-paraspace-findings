1. `PoolStorage.sol` contract uses the `bytes32` type for the `POOL_STORAGE_POSITION` constant, which is then converted to a `uint256` type using the `uint256()` function. This conversion is unnecessary, as the `bytes32` type can be directly used in the `keccak256()` function to calculate the storage position. Removing the unnecessary type conversion will reduce the gas usage of the contract and make the code more readable.
		   **Mitigation:** Just use `bytes32(x)`. It's as easy as that because they are both 2^256 (unlike `bytes`).
		   
2. Use assambly to swap in quicksort function to further optmize gas in `NFTFloorOracle.sol`
```solidity
function _quickSort(
        uint256[] memory arr,
        int256 left,
        int256 right
    ) internal pure {
        int256 i = left;
        int256 j = right;
        if (i == j) return;
        uint256 pivot = arr[uint256(left + (right - left) / 2)];
        while (i <= j) {
            while (arr[uint256(i)] < pivot) i++;
            while (pivot < arr[uint256(j)]) j--;
            if (i <= j) {
                // Use the swap function from the Solidity assembly library
                // to swap the values in the array
                assembly {
                    let data := mload(arr)
                    let index1 := add(data, mul(i, 0x20))
                    let index2 := add(data, mul(j, 0x20))
                    swap(index1, index2)
                }
                i++;
                j--;
            }
        }
        if (left < j) _quickSort(arr, left, j);
        if (i < right) _quickSort(arr, i, right);
    }

```

3. Use `unchecked{++i/--i}` insted of `i++/i-- ` and cache `.length` in for and while loops to save gas.
4. Use bitshift to calculate power of `2` with `1 << x` instead of `2**x`
	**summary:** In code below to calculate `2 ` to the power of `96` using bitwise operators, we can use the left shift operator `(<<)` to shift the binary representation of `2` (which is `10`) `96 `times to the left.
```solidity
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol-246-                        (oracleData.token1Price))
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:247:                ) * 2**96) / 1E9
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol-248-            );
--
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol-258-                        (oracleData.token1Price)
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:259:                ) * 2**96) / 1E9
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol-260-            );
--
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol-270-                        (oracleData.token1Price)
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol:271:                ) * 2**96) /
paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol-272-                    10 **
```
