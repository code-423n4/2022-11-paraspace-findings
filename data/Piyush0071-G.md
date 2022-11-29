 Gas optimization Issue: IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED ON MULTIPLE PLACES.

Description:
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < numIterations; ++i) {
should be replaced with `for (uint256 i; i < numIterations; ++i) {

**Affected code:**

ui/WPunkGateway.sol:82          for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:109        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:113        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:136        for (uint256 i = 0; i < punkIndexes.length; i++) {
ui/WPunkGateway.sol:174        for (uint256 i = 0; i < punkIndexes.length; i++) {

Consider removing explicit initializations for default values.