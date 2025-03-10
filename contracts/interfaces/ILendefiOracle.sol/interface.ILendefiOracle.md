# ILendefiOracle
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/interfaces/ILendefiOracle.sol)

**Author:**
alexei@nebula-labs(dot)xyz

Interface for the Lendefi Oracle price feed system

*Implements the oracle module interface*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Functions
### freshnessThreshold

Gets the maximum age allowed for price data


```solidity
function freshnessThreshold() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Maximum age in seconds|


### volatilityThreshold

Gets the time threshold for volatile price checks


```solidity
function volatilityThreshold() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Time threshold in seconds|


### volatilityPercentage

Gets the percentage that triggers volatility checks


```solidity
function volatilityPercentage() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Percentage in basis points|


### circuitBreakerThreshold

Gets the percentage that triggers circuit breaker


```solidity
function circuitBreakerThreshold() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Percentage in basis points|


### minimumOraclesRequired

Gets minimum oracles required for price feeds


```solidity
function minimumOraclesRequired() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Minimum number of oracles|


### primaryOracle

Gets primary oracle for an asset


```solidity
function primaryOracle(address asset) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of primary oracle|


### oracleDecimals

Gets decimals for an oracle


```solidity
function oracleDecimals(address oracle) external view returns (uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the oracle|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|Number of decimals|


### circuitBroken

Checks if circuit breaker is active for asset


```solidity
function circuitBroken(address asset) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if circuit breaker is active|


### lastValidPrice

Gets last valid price for an asset


```solidity
function lastValidPrice(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Last valid price|


### lastUpdateTimestamp

Gets timestamp of last price update


```solidity
function lastUpdateTimestamp(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Timestamp of last update|


### assetMinimumOracles

Gets minimum oracles required for specific asset


```solidity
function assetMinimumOracles(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Minimum oracles required|


### getAssetPrice

Gets validated price for an asset


```solidity
function getAssetPrice(address asset) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Current validated price|


### getSingleOraclePrice

Gets price from a single oracle


```solidity
function getSingleOraclePrice(address oracle) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the oracle|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Current price from oracle|


### addOracle

Adds new oracle for an asset


```solidity
function addOracle(address asset, address oracle, uint8 decimals) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle|
|`decimals`|`uint8`|Number of decimals for oracle|


### removeOracle

Removes oracle from an asset


```solidity
function removeOracle(address asset, address oracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle|


### setPrimaryOracle

Sets primary oracle for an asset


```solidity
function setPrimaryOracle(address asset, address oracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle|


### getOracleCount

Gets number of oracles for an asset


```solidity
function getOracleCount(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of configured oracles|


### getAssetOracles

Gets all oracles for an asset


```solidity
function getAssetOracles(address asset) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of oracle addresses|


### updateFreshnessThreshold

Updates price freshness threshold


```solidity
function updateFreshnessThreshold(uint256 freshness) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint256`|New threshold in seconds|


### updateVolatilityThreshold

Updates volatility time threshold


```solidity
function updateVolatilityThreshold(uint256 volatility) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`volatility`|`uint256`|New threshold in seconds|


### updateVolatilityPercentage

Updates volatility percentage threshold


```solidity
function updateVolatilityPercentage(uint256 percentage) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`percentage`|`uint256`|New threshold in basis points|


### updateCircuitBreakerThreshold

Updates circuit breaker threshold


```solidity
function updateCircuitBreakerThreshold(uint256 percentage) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`percentage`|`uint256`|New threshold in basis points|


### updateMinimumOracles

Updates minimum required oracles


```solidity
function updateMinimumOracles(uint256 minimum) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`minimum`|`uint256`|New minimum number of oracles|


### updateAssetMinimumOracles

Updates minimum oracles for specific asset


```solidity
function updateAssetMinimumOracles(address asset, uint256 minimum) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`minimum`|`uint256`|New minimum number of oracles|


### triggerCircuitBreaker

Manually triggers circuit breaker


```solidity
function triggerCircuitBreaker(address asset) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|


### resetCircuitBreaker

Resets circuit breaker


```solidity
function resetCircuitBreaker(address asset) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|


## Events
### OracleAdded
Emitted when a new oracle is added for an asset


```solidity
event OracleAdded(address indexed asset, address indexed oracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle being added|

### OracleRemoved
Emitted when an oracle is removed from an asset


```solidity
event OracleRemoved(address indexed asset, address indexed oracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle being removed|

### PrimaryOracleSet
Emitted when primary oracle is set for an asset


```solidity
event PrimaryOracleSet(address indexed asset, address indexed oracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the new primary oracle|

### FreshnessThresholdUpdated
Emitted when price freshness threshold is updated


```solidity
event FreshnessThresholdUpdated(uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldValue`|`uint256`|Previous threshold value|
|`newValue`|`uint256`|New threshold value|

### VolatilityThresholdUpdated
Emitted when volatility time threshold is updated


```solidity
event VolatilityThresholdUpdated(uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldValue`|`uint256`|Previous threshold value|
|`newValue`|`uint256`|New threshold value|

### VolatilityPercentageUpdated
Emitted when volatility percentage threshold is updated


```solidity
event VolatilityPercentageUpdated(uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldValue`|`uint256`|Previous percentage value|
|`newValue`|`uint256`|New percentage value|

### CircuitBreakerThresholdUpdated
Emitted when circuit breaker threshold is updated


```solidity
event CircuitBreakerThresholdUpdated(uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldValue`|`uint256`|Previous threshold value|
|`newValue`|`uint256`|New threshold value|

### CircuitBreakerTriggered
Emitted when circuit breaker is triggered for an asset


```solidity
event CircuitBreakerTriggered(address indexed asset, uint256 currentPrice, uint256 previousPrice);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the affected asset|
|`currentPrice`|`uint256`|Price that triggered the circuit breaker|
|`previousPrice`|`uint256`|Last valid price before trigger|

### CircuitBreakerReset
Emitted when circuit breaker is reset for an asset


```solidity
event CircuitBreakerReset(address indexed asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset being reset|

### PriceUpdated
Emitted when price is updated for an asset


```solidity
event PriceUpdated(address indexed asset, uint256 price, uint256 median, uint256 numOracles);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`price`|`uint256`|New price value|
|`median`|`uint256`|Median price from all oracles|
|`numOracles`|`uint256`|Number of oracles used in calculation|

### MinimumOraclesUpdated
Emitted when minimum required oracles is updated


```solidity
event MinimumOraclesUpdated(uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldValue`|`uint256`|Previous minimum value|
|`newValue`|`uint256`|New minimum value|

### AssetMinimumOraclesUpdated
Emitted when asset-specific minimum oracles is updated


```solidity
event AssetMinimumOraclesUpdated(address indexed asset, uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the affected asset|
|`oldValue`|`uint256`|Previous minimum value|
|`newValue`|`uint256`|New minimum value|

### NotEnoughOraclesWarning
Emitted when there are insufficient oracles for an asset


```solidity
event NotEnoughOraclesWarning(address indexed asset, uint256 required, uint256 actual);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the affected asset|
|`required`|`uint256`|Required number of oracles|
|`actual`|`uint256`|Actual number of oracles available|

## Errors
### OracleInvalidPrice
Oracle returned an invalid or negative price


```solidity
error OracleInvalidPrice(address oracle, int256 price);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the oracle|
|`price`|`int256`|Invalid price value|

### OracleStalePrice
Oracle round ID mismatch indicates stale price


```solidity
error OracleStalePrice(address oracle, uint80 roundId, uint80 answeredInRound);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the oracle|
|`roundId`|`uint80`|Current round ID|
|`answeredInRound`|`uint80`|Round when price was updated|

### OracleTimeout
Oracle price update is older than allowed threshold


```solidity
error OracleTimeout(address oracle, uint256 timestamp, uint256 currentTimestamp, uint256 maxAge);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the oracle|
|`timestamp`|`uint256`|Price timestamp|
|`currentTimestamp`|`uint256`|Current block timestamp|
|`maxAge`|`uint256`|Maximum allowed age|

### OracleInvalidPriceVolatility
Price change exceeds volatility threshold


```solidity
error OracleInvalidPriceVolatility(address oracle, int256 price, uint256 volatility);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the oracle|
|`price`|`int256`|Current price|
|`volatility`|`uint256`|Maximum allowed volatility|

### OracleNotFound
No oracle configured for asset


```solidity
error OracleNotFound(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

### PrimaryOracleNotSet
No primary oracle set for asset


```solidity
error PrimaryOracleNotSet(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

### CircuitBreakerActive
Circuit breaker is currently active for asset


```solidity
error CircuitBreakerActive(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

### InvalidThreshold
Threshold value outside allowed range


```solidity
error InvalidThreshold(string name, uint256 value, uint256 minValue, uint256 maxValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the threshold|
|`value`|`uint256`|Provided value|
|`minValue`|`uint256`|Minimum allowed value|
|`maxValue`|`uint256`|Maximum allowed value|

### NotEnoughOracles
Insufficient number of valid oracles


```solidity
error NotEnoughOracles(address asset, uint256 required, uint256 actual);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`required`|`uint256`|Required number of oracles|
|`actual`|`uint256`|Available valid oracles|

### OracleAlreadyAdded
Oracle already configured for asset


```solidity
error OracleAlreadyAdded(address asset, address oracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle|

### InvalidOracle
Oracle address is invalid or not supported


```solidity
error InvalidOracle(address oracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the invalid oracle|

### CircuitBreakerCooldown
Circuit breaker cooldown period not elapsed


```solidity
error CircuitBreakerCooldown(address asset, uint256 remainingTime);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`remainingTime`|`uint256`|Time remaining until reset allowed|

### LargeDeviation
Price deviation exceeds allowed threshold


```solidity
error LargeDeviation(address asset, uint256 price, uint256 previousPrice, uint256 deviationPct);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`price`|`uint256`|Current price|
|`previousPrice`|`uint256`|Previous price|
|`deviationPct`|`uint256`|Percentage deviation|

