## Reuse the memory variable `vars.reserveConfiguration` instead of using again

On this [line](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L132), `vars.reserveConfiguration` is being initialized but then it's not utilized on this [line](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol#L140) which will cause extra storage read 

## Redundant calculation in the `DefaultReserveInterestRateStrategy`

`vars.borrowUsageRatio` and `vars.supplyUsageRatio` are the same but they are being calculated twice for not specific reason.