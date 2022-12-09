## Redundant check
Redundant check `feederIndex >= 0` in `removeFeeder(...)`. The `feederIndex` is of type `uint8` and therefore can't have other values than in range `[0, 255]`. Having unnecessary checks consumes gas and increases code complexity.

## Redundant data stored in list `assets`
The data stored in `assets[]` is not used in the protocol. The `assets[]` is only accessed to update its' values, but this data is never used. Having redundant data stored consumes gas and increases code complexity which may decrease readability and lead to bugs during further development.