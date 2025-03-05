# LendefiOracle
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/oracle/LendefiOracle.sol)

# Lendefi Oracle: A Comprehensive Technical Overview

The LendefiOracle module is a sophisticated price oracle system designed to provide secure, reliable, and manipulation-resistant price data for the Lendefi Protocol. It implements multiple layers of protection to ensure the integrity of financial operations that depend on accurate market prices.

## Core Architecture

### Multi-Oracle Design
- **Redundant Data Sources**: Aggregates price data from multiple oracles per asset
- **Median Calculation**: Uses statistical median to eliminate outliers and price manipulation attempts
- **Configurable Requirements**: Customizable minimum oracle count per asset

### Price Feed Integration
- **Chainlink Compatible**: Primarily designed for Chainlink price feeds (AggregatorV3Interface)
- **Decimal Handling**: Normalizes different oracle decimal formats
- **Historical Data**: Leverages historical rounds for price change analysis

## Security Mechanisms

### Price Validation
1. **Basic Validation**
   - Ensures prices are positive
   - Verifies round completeness (answeredInRound >= roundId)

2. **Freshness Checks**
   - Configurable maximum age for price data (default: 8 hours)
   - Stricter freshness requirements during high volatility (default: 1 hour)
   - Prevents using outdated prices during critical market conditions

3. **Volatility Protection**
   - Detects significant price movements between rounds
   - Default threshold: 20% change triggers stricter freshness requirements
   - Rejects volatile prices that aren't sufficiently recent

### Circuit Breakers

1. **Automatic Circuit Breakers**
   - Triggered when price changes exceed configured threshold (default: 50%)
   - Prevents trading based on potentially manipulated or anomalous prices
   - Reverts with detailed diagnostics for investigation

2. **Manual Controls**
   - Authorized roles can manually trigger/reset circuit breakers
   - Provides emergency override during market anomalies
   - Governance control for extreme situations

### Fallback Mechanisms

Implements a cascading fallback strategy when primary price determination fails:
1. **Primary Oracle**: Attempts to use designated primary oracle if median calculation fails
2. **Historical Prices**: Falls back to last valid price if it's recent enough
3. **Graceful Failure**: Provides detailed error information when no valid price is available

## Admin Controls

### Role-Based Access
- **DEFAULT_ADMIN_ROLE**: Global administrative capabilities
- **ORACLE_MANAGER_ROLE**: Can add/remove oracles and adjust thresholds
- **CIRCUIT_BREAKER_ROLE**: Can manually trigger/reset circuit breakers
- **UPGRADER_ROLE**: Can authorize contract upgrades

### Configurable Thresholds
- Freshness threshold (8 hours default)
- Volatility threshold (1 hour default)
- Volatility percentage (20% default)
- Circuit breaker threshold (50% default)
- Minimum oracles required (2 default)

## Integration Points

### Main External Interface
- `getAssetPrice(address asset)`: Primary function for obtaining verified asset prices

### Oracle Management
- Functions to add, remove, and configure oracles
- Primary oracle designation
- Asset-specific configuration options

## Implementation Highlights

1. **QuickSort Algorithm**: Efficient median calculation for multiple price feeds
2. **Try/Catch Design**: Robust error handling to manage oracle failures gracefully
3. **Event Emission**: Comprehensive events for off-chain monitoring
4. **Upgradeable Design**: UUPS pattern for future enhancements
5. **Gas Optimizations**: Efficient data structures and algorithms for lower execution costs

This oracle system represents a production-grade solution that balances security, reliability, and gas efficiency while protecting against the various vulnerabilities that typically affect price oracle systems in DeFi.

**Inherits:**
AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Secure price feed provider for Lendefi Protocol with robust validation

*Implements multi-oracle architecture with multiple security features:
- Multiple price sources with median calculation
- Configurable freshness thresholds
- Volatility detection and protection
- Circuit breakers for extreme price movements*


## State Variables
### ORACLE_MANAGER_ROLE

```solidity
bytes32 public constant ORACLE_MANAGER_ROLE = keccak256("ORACLE_MANAGER_ROLE");
```


### UPGRADER_ROLE

```solidity
bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### CIRCUIT_BREAKER_ROLE

```solidity
bytes32 public constant CIRCUIT_BREAKER_ROLE = keccak256("CIRCUIT_BREAKER_ROLE");
```


### freshnessThreshold

```solidity
uint256 public freshnessThreshold;
```


### volatilityThreshold

```solidity
uint256 public volatilityThreshold;
```


### volatilityPercentage

```solidity
uint256 public volatilityPercentage;
```


### circuitBreakerThreshold

```solidity
uint256 public circuitBreakerThreshold;
```


### assetOracles

```solidity
mapping(address asset => address[] oracles) private assetOracles;
```


### primaryOracle

```solidity
mapping(address asset => address primary) public primaryOracle;
```


### oracleDecimals

```solidity
mapping(address oracle => uint8 decimals) public oracleDecimals;
```


### circuitBroken

```solidity
mapping(address asset => bool broken) public circuitBroken;
```


### lastValidPrice

```solidity
mapping(address asset => uint256 price) public lastValidPrice;
```


### lastUpdateTimestamp

```solidity
mapping(address asset => uint256 timestamp) public lastUpdateTimestamp;
```


### minimumOraclesRequired

```solidity
uint256 public minimumOraclesRequired;
```


### assetMinimumOracles

```solidity
mapping(address asset => uint256 minOraclesForAsset) public assetMinimumOracles;
```


## Functions
### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

Initializes the oracle module with default parameters


```solidity
function initialize(address admin, address manager) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|Address that will have DEFAULT_ADMIN_ROLE|
|`manager`|`address`|Address that will have ORACLE_MANAGER_ROLE|


### addOracle

Adds a new oracle for an asset

*Adds a new price oracle to the array of oracles for the asset*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function addOracle(address asset, address oracle, uint8 oracleDecimalsValue) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the Chainlink price feed|
|`oracleDecimalsValue`|`uint8`|Number of decimals in oracle price feed|


### removeOracle

Removes an oracle for an asset

*Removes oracle from the array of oracles for the asset*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function removeOracle(address asset, address oracle) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the Chainlink price feed to remove|


### setPrimaryOracle

Sets the primary oracle for an asset

*The primary oracle is used as a fallback when median calculation fails*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function setPrimaryOracle(address asset, address oracle) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle to set as primary|


### updateMinimumOracles

Updates the global minimum number of oracles required for reliable price

*Minimum value is 1, applies to all assets unless overridden*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function updateMinimumOracles(uint256 minimum) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`minimum`|`uint256`|New minimum number of oracles|


### updateAssetMinimumOracles

Updates the minimum number of oracles required for a specific asset

*Set to 0 to use the global default*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function updateAssetMinimumOracles(address asset, uint256 minimum) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`minimum`|`uint256`|New minimum number of oracles for this asset|


### updateFreshnessThreshold

Updates the freshness threshold for price feeds

*Controls how old price data can be before rejection*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function updateFreshnessThreshold(uint256 threshold) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`threshold`|`uint256`|New maximum age of price data in seconds|


### updateVolatilityThreshold

Updates the volatility time threshold

*Controls how old price data can be during high volatility*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function updateVolatilityThreshold(uint256 threshold) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`threshold`|`uint256`|New threshold for volatile asset prices in seconds|


### updateVolatilityPercentage

Updates the volatility percentage threshold

*Price changes above this percentage require fresher data*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function updateVolatilityPercentage(uint256 percentage) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`percentage`|`uint256`|New percentage that triggers the volatility check|


### updateCircuitBreakerThreshold

Updates the circuit breaker threshold

*Price changes above this percentage halt trading for the asset*

**Note:**
access: Restricted to ORACLE_MANAGER_ROLE


```solidity
function updateCircuitBreakerThreshold(uint256 percentage) external onlyRole(ORACLE_MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`percentage`|`uint256`|New percentage that triggers the circuit breaker|


### getAssetPrice

Gets the median price from multiple oracles for an asset

*Main function for external protocols to get verified asset prices*

**Note:**
security: Implements comprehensive validation and multiple oracle sources


```solidity
function getAssetPrice(address asset) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 Median price in USD with 8 decimals|


### getOracleCount

Gets the number of oracles for an asset


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
|`<none>`|`uint256`|uint256 Number of oracles configured for the asset|


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
|`<none>`|`address[]`|address[] Array of oracle addresses for the asset|


### triggerCircuitBreaker

Manually triggers the circuit breaker for an asset

*Used in emergency situations to pause asset trading*

**Note:**
access: Restricted to CIRCUIT_BREAKER_ROLE


```solidity
function triggerCircuitBreaker(address asset) external onlyRole(CIRCUIT_BREAKER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|


### resetCircuitBreaker

Resets the circuit breaker for an asset

*Allows trading to resume after manual review*

**Note:**
access: Restricted to CIRCUIT_BREAKER_ROLE


```solidity
function resetCircuitBreaker(address asset) external onlyRole(CIRCUIT_BREAKER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|


### _getSingleOraclePrice

Gets the price from a single oracle with safety validations

*Internal function with extensive validation*


```solidity
function _getSingleOraclePrice(address oracleAddress) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracleAddress`|`address`|Address of the Chainlink price feed|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 Price in USD with oracle's native decimal precision|


### _getMedianPrice

Calculates the median price from multiple oracles

*Uses multiple oracles and fallback mechanisms*


```solidity
function _getMedianPrice(address asset) internal returns (uint256 median);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`median`|`uint256`|Median price in USD with 8 decimals|


### _sortPrices

QuickSort implementation to sort price array

*Recursive quicksort used for finding median price*


```solidity
function _sortPrices(uint256[] memory arr, int256 left, int256 right) internal pure;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`arr`|`uint256[]`|Array of prices to sort|
|`left`|`int256`|Starting index|
|`right`|`int256`|Ending index|


### getSingleOraclePrice

External function to call internal methods for testing

*This is only used for try/catch operations within the contract*


```solidity
function getSingleOraclePrice(address oracle) external view returns (uint256);
```

### _authorizeUpgrade

Authorizes contract upgrades

**Note:**
security: Restricted to UPGRADER_ROLE


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


## Events
### OracleAdded

```solidity
event OracleAdded(address indexed asset, address indexed oracle);
```

### OracleRemoved

```solidity
event OracleRemoved(address indexed asset, address indexed oracle);
```

### PrimaryOracleSet

```solidity
event PrimaryOracleSet(address indexed asset, address indexed oracle);
```

### FreshnessThresholdUpdated

```solidity
event FreshnessThresholdUpdated(uint256 oldValue, uint256 newValue);
```

### VolatilityThresholdUpdated

```solidity
event VolatilityThresholdUpdated(uint256 oldValue, uint256 newValue);
```

### VolatilityPercentageUpdated

```solidity
event VolatilityPercentageUpdated(uint256 oldValue, uint256 newValue);
```

### CircuitBreakerThresholdUpdated

```solidity
event CircuitBreakerThresholdUpdated(uint256 oldValue, uint256 newValue);
```

### CircuitBreakerTriggered

```solidity
event CircuitBreakerTriggered(address indexed asset, uint256 currentPrice, uint256 previousPrice);
```

### CircuitBreakerReset

```solidity
event CircuitBreakerReset(address indexed asset);
```

### PriceUpdated

```solidity
event PriceUpdated(address indexed asset, uint256 price, uint256 median, uint256 numOracles);
```

### MinimumOraclesUpdated

```solidity
event MinimumOraclesUpdated(uint256 oldValue, uint256 newValue);
```

### AssetMinimumOraclesUpdated

```solidity
event AssetMinimumOraclesUpdated(address indexed asset, uint256 oldValue, uint256 newValue);
```

### NotEnoughOraclesWarning

```solidity
event NotEnoughOraclesWarning(address indexed asset, uint256 required, uint256 actual);
```

## Errors
### OracleInvalidPrice

```solidity
error OracleInvalidPrice(address oracle, int256 price);
```

### OracleStalePrice

```solidity
error OracleStalePrice(address oracle, uint80 roundId, uint80 answeredInRound);
```

### OracleTimeout

```solidity
error OracleTimeout(address oracle, uint256 timestamp, uint256 currentTimestamp, uint256 maxAge);
```

### OracleInvalidPriceVolatility

```solidity
error OracleInvalidPriceVolatility(address oracle, int256 price, uint256 volatility);
```

### OracleNotFound

```solidity
error OracleNotFound(address asset);
```

### PrimaryOracleNotSet

```solidity
error PrimaryOracleNotSet(address asset);
```

### CircuitBreakerActive

```solidity
error CircuitBreakerActive(address asset);
```

### InvalidThreshold

```solidity
error InvalidThreshold(string name, uint256 value, uint256 minValue, uint256 maxValue);
```

### NotEnoughOracles

```solidity
error NotEnoughOracles(address asset, uint256 required, uint256 actual);
```

### OracleAlreadyAdded

```solidity
error OracleAlreadyAdded(address asset, address oracle);
```

### InvalidOracle

```solidity
error InvalidOracle(address oracle);
```

### CircuitBreakerCooldown

```solidity
error CircuitBreakerCooldown(address asset, uint256 remainingTime);
```

### LargeDeviation

```solidity
error LargeDeviation(address asset, uint256 price, uint256 previousPrice, uint256 deviationPct);
```

