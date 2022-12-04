## Description

The ERC721OracleWrapper contract contains a potential vulnerability in the "_onlyAssetListingOrPoolAdmins" function. This function is intended to allow the contract to be called only by authorized users (asset listing or pool admin accounts). However, the code does not properly check if the caller is actually an authorized user, and as a result, an attacker could potentially call the contract's functions without being an authorized user.

## Impact

If this vulnerability is exploited, an attacker could potentially call the contract's functions without being an authorized user. This could allow the attacker to manipulate the contract's behavior and potentially cause harm to the users of the contract.

## Proof of Concept

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/ERC721OracleWrapper.sol

## Recommended Mitigation Steps

To mitigate this vulnerability, the contract's "_onlyAssetListingOrPoolAdmins" function should be updated to properly verify the caller's credentials before allowing them to access the contract's functions. This can be done by using the contract's "IACLManager" interface to verify the caller's credentials before allowing them to access the contract's functions. Additionally, the error message thrown by the "require()" function should be more informative to assist users in understanding and addressing the issue.