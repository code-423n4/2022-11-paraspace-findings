
# 1. Lines are too long

Usually, lines in source code are limited to 80 characters.

It's advised to keep lines lower than 120 characters.

Today’s screens are much larger so it’s reasonable to stretch this in some cases.

Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

There is 1 instance of this issue

- [LiquidationLogic.sol#L603](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L603)

# 2. Typos

### [File: LiquidationLogic.sol](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol)

- [LiquidationLogic.sol#L104](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L104)

```
Line 104:   //userDebt from liquadationReserve
```

### [File: NFTFloorOracle.sol](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol)

- [NFTFloorOracle.sol#L412](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L412), [NFTFloorOracle.sol#L53](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L53), [NFTFloorOracle.sol#L54](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/misc/NFTFloorOracle.sol#L54)

```
Line  53:    /// aggeregate prices which are not expired from different feeders, if number of valid/unexpired prices

Line  54:    /// not enough, we do not aggeregate and just use previous price

Line 412:    //aggeregate with price from all feeders
```

### [SignatureChecker.sol#L14](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol)

- [SignatureChecker.sol#L14](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol#L14), [SignatureChecker.sol#L15](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol#L15), [SignatureChecker.sol#L43](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol#L43), [SignatureChecker.sol#L48](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/looksrare/contracts/libraries/SignatureChecker.sol#L48)

```
Line 14:    * @param hash the hash containing the signed mesage

Line 15:    * @param v parameter (27 or 28). This prevents maleability since the public key recovery equation has two possible solutions.

Line 43:    * @param hash the hash containing the signed mesage

Line 48:    * @param domainSeparator paramer to prevent signature being executed in other chains and environments
```

Suggested changes:

- `liquadationReserve` => `liquidationReserve`
- `aggeregate` => `aggregate`
- `mesage` => `message`
- `maleability` => `malleability`
- `paramer` => `parameter`