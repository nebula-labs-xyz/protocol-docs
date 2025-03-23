# IASSETS
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/a5a218c0db8bfb52cb836dc7d721fd999f5de3c1/contracts/interfaces/IASSETS.sol)

Interface for the LendefiAssets contract

*Manages asset configurations, listings, and oracle integrations*


## Functions
### initialize

Initialize the contract


```solidity
function initialize(address timelock, address multisig, address usdc) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timelock`|`address`|Address with manager role|
|`multisig`|`address`|Address with admin roles|
|`usdc`|`address`|USDC address|


### updateUniswapOracle

Register a Uniswap V3 pool as an oracle for an asset


```solidity
function updateUniswapOracle(address asset, address uniswapPool, uint32 twapPeriod, uint8 active) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to register the oracle for|
|`uniswapPool`|`address`|The Uniswap V3 pool address (must contain the asset)|
|`twapPeriod`|`uint32`|The TWAP period in seconds|
|`active`|`uint8`|Whether this oracle is active (0 = inactive, 1 = active)|


### updateChainlinkOracle

Add a Chainlink oracle for an asset


```solidity
function updateChainlinkOracle(address asset, address oracle, uint8 active) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to add the oracle for|
|`oracle`|`address`|The oracle address|
|`active`|`uint8`|Whether this oracle is active (0 = inactive, 1 = active)|


### updateMainOracleConfig

Update main oracle configuration parameters


```solidity
function updateMainOracleConfig(uint80 freshness, uint80 volatility, uint40 volatilityPct, uint40 circuitBreakerPct)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint80`|Maximum staleness threshold in seconds|
|`volatility`|`uint80`|Volatility monitoring period in seconds|
|`volatilityPct`|`uint40`|Maximum price change percentage allowed|
|`circuitBreakerPct`|`uint40`|Percentage difference that triggers circuit breaker|


### updateTierConfig

Update rate configuration for a collateral tier


```solidity
function updateTierConfig(CollateralTier tier, uint256 jumpRate, uint256 liquidationFee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to update|
|`jumpRate`|`uint256`|The new jump rate (in basis points * 100)|
|`liquidationFee`|`uint256`|The new liquidation fee (in basis points * 100)|


### setCoreAddress

Set the core protocol address


```solidity
function setCoreAddress(address newCore) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCore`|`address`|The new core protocol address|


### pause

Pause the contract


```solidity
function pause() external;
```

### unpause

Unpause the contract


```solidity
function unpause() external;
```

### updateAssetConfig

Update or add an asset configuration


```solidity
function updateAssetConfig(address asset, Asset calldata config) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to update|
|`config`|`Asset`|The complete asset configuration|


### updateAssetTier

Update the tier of an existing asset


```solidity
function updateAssetTier(address asset, CollateralTier newTier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to update|
|`newTier`|`CollateralTier`|The new collateral tier|


### setPrimaryOracle

Set the primary oracle type for an asset


```solidity
function setPrimaryOracle(address asset, OracleType oracleType) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|
|`oracleType`|`OracleType`|The oracle type to set as primary|


### triggerCircuitBreaker

Update the minimum oracle count for an asset

Trigger circuit breaker for an asset (suspend price feeds)


```solidity
function triggerCircuitBreaker(address asset) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|


### resetCircuitBreaker

Reset circuit breaker for an asset (resume price feeds)


```solidity
function resetCircuitBreaker(address asset) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to reset circuit breaker for|


### getOracleByType

Get the oracle address for a specific asset and oracle type


```solidity
function getOracleByType(address asset, OracleType oracleType) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|
|`oracleType`|`OracleType`|The oracle type to retrieve|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The oracle address for the specified type|


### getAssetPriceByType

Get the price from a specific oracle type for an asset


```solidity
function getAssetPriceByType(address asset, OracleType oracleType) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to get price for|
|`oracleType`|`OracleType`|The specific oracle type to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The price from the specified oracle type|


### getAssetPrice

Get asset price using optimal oracle configuration


```solidity
function getAssetPrice(address asset) external view returns (uint256 price);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to get price for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|The current price of the asset|


### scheduleUpgrade

Schedules an upgrade to a new implementation with timelock


```solidity
function scheduleUpgrade(address newImplementation) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


### cancelUpgrade

Cancels a previously scheduled upgrade


```solidity
function cancelUpgrade() external;
```

### upgradeTimelockRemaining

Returns the remaining time before a scheduled upgrade can be executed


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The time remaining in seconds|


### getAssetDetails

Get comprehensive details about an asset


```solidity
function getAssetDetails(address asset)
    external
    view
    returns (uint256 price, uint256 totalSupplied, uint256 maxSupply, CollateralTier tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current asset price|
|`totalSupplied`|`uint256`|Total amount supplied to the protocol|
|`maxSupply`|`uint256`|Maximum supply threshold|
|`tier`|`CollateralTier`|Collateral tier of the asset|


### getTierRates

Get rates for all tiers


```solidity
function getTierRates() external view returns (uint256[4] memory jumpRates, uint256[4] memory liquidationFees);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`jumpRates`|`uint256[4]`|Array of jump rates for all tiers|
|`liquidationFees`|`uint256[4]`|Array of liquidation fees for all tiers|


### getTierJumpRate

Get jump rate for a specific tier


```solidity
function getTierJumpRate(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The jump rate for the tier|


### isAssetValid

Check if an asset is valid (listed and active)


```solidity
function isAssetValid(address asset) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the asset is valid|


### isAssetAtCapacity

Check if adding more supply would exceed an asset's capacity


```solidity
function isAssetAtCapacity(address asset, uint256 additionalAmount) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to check|
|`additionalAmount`|`uint256`|The additional amount to supply|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the asset would be at capacity after adding the amount|


### getAssetInfo

Get full asset configuration


```solidity
function getAssetInfo(address asset) external view returns (Asset memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Asset`|The complete asset configuration|


### getListedAssets

Get all listed assets


```solidity
function getListedAssets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of listed asset addresses|


### getLiquidationFee

Get liquidation fee for a collateral tier


```solidity
function getLiquidationFee(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The liquidation fee for the tier|


### getAssetTier

Check if an asset is in the isolated tier


```solidity
function getAssetTier(address asset) external view returns (CollateralTier tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|Whether the asset is isolated|


### getAssetDecimals

Get the asset decimals


```solidity
function getAssetDecimals(address asset) external view returns (uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|decimals|


### getAssetLiquidationThreshold

Get the asset liquidation threshold


```solidity
function getAssetLiquidationThreshold(address asset) external view returns (uint16);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|liq threshold|


### getAssetBorrowThreshold

Get the asset borrow threshold


```solidity
function getAssetBorrowThreshold(address asset) external view returns (uint16);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|borrow threshold|


### getIsolationDebtCap

Get the debt cap for an isolated asset


```solidity
function getIsolationDebtCap(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The isolation debt cap|


### getOracleCount

Get the number of active oracles for an asset


```solidity
function getOracleCount(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of active oracles|


### checkPriceDeviation

Check for price deviation without modifying state


```solidity
function checkPriceDeviation(address asset) external view returns (bool, uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the asset has a large price deviation and the deviation percentage|
|`<none>`|`uint256`||


### version

Get protocol version


```solidity
function version() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The current protocol version|


### coreAddress

Get the core protocol address


```solidity
function coreAddress() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the core protocol|


### circuitBroken

Get circuit breaker status for an asset


```solidity
function circuitBroken(address asset) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the circuit breaker is active|


### UPGRADE_TIMELOCK_DURATION

Get tier configuration for a specific collateral tier

Get the current upgrade timelock duration


```solidity
function UPGRADE_TIMELOCK_DURATION() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The tier rates configuration|


### getAssetCalculationParams

Get information about a pending upgrade

Get the main oracle configuration

Gets all parameters needed for collateral calculations in a single call

*Consolidates multiple getter calls into a single cross-contract call*


```solidity
function getAssetCalculationParams(address asset) external view returns (AssetCalculationParams memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`AssetCalculationParams`|The pending upgrade request|


### poolLiquidityLimit

Checks if an amount exceeds pool liquidity limits

*Only applicable for assets with active Uniswap oracle*


```solidity
function poolLiquidityLimit(address asset, uint256 amount) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to check|
|`amount`|`uint256`|The amount to validate against pool liquidity|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Boolean - If amount exceeds 3% of the Uniswap v3 pool liquidity|


## Events
### CoreAddressUpdated

```solidity
event CoreAddressUpdated(address indexed newCore);
```

### UpdateAssetConfig

```solidity
event UpdateAssetConfig(Asset config);
```

### AssetTierUpdated

```solidity
event AssetTierUpdated(address indexed asset, CollateralTier tier);
```

### PrimaryOracleSet

```solidity
event PrimaryOracleSet(address indexed asset, OracleType oracleType);
```

### AssetMinimumOraclesUpdated

```solidity
event AssetMinimumOraclesUpdated(address indexed asset, uint256 oldValue, uint256 newValue);
```

### CircuitBreakerTriggered

```solidity
event CircuitBreakerTriggered(address indexed asset, uint256 oldPrice, uint256 newPrice);
```

### CircuitBreakerReset

```solidity
event CircuitBreakerReset(address indexed asset);
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

### TierParametersUpdated

```solidity
event TierParametersUpdated(CollateralTier indexed tier, uint256 jumpRate, uint256 liquidationFee);
```

### UpgradeScheduled

```solidity
event UpgradeScheduled(
    address indexed sender, address indexed implementation, uint64 scheduledTime, uint64 effectiveTime
);
```

### UpgradeCancelled

```solidity
event UpgradeCancelled(address indexed sender, address indexed implementation);
```

### Upgrade

```solidity
event Upgrade(address indexed sender, address indexed implementation);
```

### ChainlinkOracleUpdated

```solidity
event ChainlinkOracleUpdated(address indexed asset, address indexed oracle, uint8 active);
```

## Errors
### ZeroAddressNotAllowed

```solidity
error ZeroAddressNotAllowed();
```

### AssetNotListed

```solidity
error AssetNotListed(address asset);
```

### AssetNotInUniswapPool

```solidity
error AssetNotInUniswapPool(address asset, address pool);
```

### CircuitBreakerActive

```solidity
error CircuitBreakerActive(address asset);
```

### InvalidParameter

```solidity
error InvalidParameter(string param, uint256 value);
```

### InvalidThreshold

```solidity
error InvalidThreshold(string param, uint256 value, uint256 min, uint256 max);
```

### RateTooHigh

```solidity
error RateTooHigh(uint256 rate, uint256 maxAllowed);
```

### FeeTooHigh

```solidity
error FeeTooHigh(uint256 fee, uint256 maxAllowed);
```

### NotEnoughValidOracles

```solidity
error NotEnoughValidOracles(address asset, uint8 required, uint8 available);
```

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
error OracleTimeout(address oracle, uint256 timestamp, uint256 blockTimestamp, uint256 threshold);
```

### OracleInvalidPriceVolatility

```solidity
error OracleInvalidPriceVolatility(address oracle, int256 price, uint256 changePercent);
```

### InvalidUniswapConfig

```solidity
error InvalidUniswapConfig(address asset);
```

### InvalidLiquidationThreshold

```solidity
error InvalidLiquidationThreshold(uint256 threshold);
```

### InvalidBorrowThreshold

```solidity
error InvalidBorrowThreshold(uint256 threshold);
```

### UpgradeNotScheduled

```solidity
error UpgradeNotScheduled();
```

### ImplementationMismatch

```solidity
error ImplementationMismatch(address expected, address actual);
```

### UpgradeTimelockActive

```solidity
error UpgradeTimelockActive(uint256 timeRemaining);
```

## Structs
### UpgradeRequest
Information about a scheduled contract upgrade


```solidity
struct UpgradeRequest {
    address implementation;
    uint64 scheduledTime;
    bool exists;
}
```

### UniswapPoolConfig
Configuration for Uniswap V3 pool-based oracle


```solidity
struct UniswapPoolConfig {
    address pool;
    uint32 twapPeriod;
    uint8 active;
}
```

### ChainlinkOracleConfig
Configuration for Chainlink oracle


```solidity
struct ChainlinkOracleConfig {
    address oracleUSD;
    uint8 active;
}
```

### Asset
Asset configuration


```solidity
struct Asset {
    uint8 active;
    uint8 decimals;
    uint16 borrowThreshold;
    uint16 liquidationThreshold;
    uint256 maxSupplyThreshold;
    uint256 isolationDebtCap;
    uint8 assetMinimumOracles;
    OracleType primaryOracleType;
    CollateralTier tier;
    ChainlinkOracleConfig chainlinkConfig;
    UniswapPoolConfig poolConfig;
}
```

### TierRates
Rate configuration for each collateral tier


```solidity
struct TierRates {
    uint256 jumpRate;
    uint256 liquidationFee;
}
```

### MainOracleConfig
Global oracle configuration


```solidity
struct MainOracleConfig {
    uint80 freshnessThreshold;
    uint80 volatilityThreshold;
    uint40 volatilityPercentage;
    uint40 circuitBreakerThreshold;
}
```

### AssetCalculationParams

```solidity
struct AssetCalculationParams {
    uint256 price;
    uint16 borrowThreshold;
    uint16 liquidationThreshold;
    uint8 decimals;
}
```

## Enums
### CollateralTier
Collateral tiers for assets


```solidity
enum CollateralTier {
    STABLE,
    CROSS_A,
    CROSS_B,
    ISOLATED
}
```

### OracleType
Oracle types


```solidity
enum OracleType {
    CHAINLINK,
    UNISWAP_V3_TWAP
}
```

