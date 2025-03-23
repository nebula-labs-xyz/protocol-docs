# LendefiAssets
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/a5a218c0db8bfb52cb836dc7d721fd999f5de3c1/contracts/lender/LendefiAssets.sol)

**Inherits:**
[IASSETS](/contracts/interfaces/IASSETS.sol/interface.IASSETS.md), Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, PausableUpgradeable, UUPSUpgradeable

**Author:**
alexei@nebula-labs(dot)xyz

Manages asset configurations, listings, and oracle integrations

*Extracted component for asset-related functionality*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

- oz-upgrades: 


## State Variables
### USDC_ETH_POOL

```solidity
address public constant USDC_ETH_POOL = 0x88e6A0c2dDD26FEEb64F039a2c41296FcB3f5640;
```


### MANAGER_ROLE
Role that allows managing asset configurations and oracle settings

*Hash of "MANAGER_ROLE"*


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
Role that allows initiating and executing contract upgrades

*Hash of "UPGRADER_ROLE"*


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### PAUSER_ROLE
Role that allows pausing and unpausing contract operations

*Hash of "PAUSER_ROLE"*


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### CIRCUIT_BREAKER_ROLE
Role that can activate or deactivate circuit breakers for assets

*Hash of "CIRCUIT_BREAKER_ROLE"*


```solidity
bytes32 internal constant CIRCUIT_BREAKER_ROLE = keccak256("CIRCUIT_BREAKER_ROLE");
```


### UPGRADE_TIMELOCK_DURATION
Duration of the timelock for upgrade operations

*Set to 3 days to allow sufficient time for review*


```solidity
uint256 public constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### version
Current version of the contract implementation

*Incremented on each upgrade*


```solidity
uint8 public version;
```


### coreAddress
Address of the core protocol contract

*Used for cross-contract calls and validation*


```solidity
address public coreAddress;
```


### usdc
Address of the usdc contract


```solidity
address internal usdc;
```


### pendingUpgrade
Information about the currently pending upgrade request

*Stores implementation address and scheduling details*


```solidity
UpgradeRequest public pendingUpgrade;
```


### lendefiInstance
Interface to interact with the core protocol

*Used to query protocol state and perform operations*


```solidity
IPROTOCOL internal lendefiInstance;
```


### listedAssets
Set of all listed asset addresses

*Uses OpenZeppelin's EnumerableSet for efficient membership checks*


```solidity
EnumerableSet.AddressSet internal listedAssets;
```


### assetInfo
Mapping of asset address to its configuration

*Stores complete asset settings including thresholds and oracle configs*


```solidity
mapping(address => Asset) internal assetInfo;
```


### tierConfig
Configuration of rates for each collateral tier

*Maps tier enum to its associated rates struct*


```solidity
mapping(CollateralTier => TierRates) public tierConfig;
```


### mainOracleConfig
Global oracle configuration parameters

*Controls oracle freshness, volatility checks, and circuit breaker thresholds*


```solidity
MainOracleConfig public mainOracleConfig;
```


### circuitBroken
Tracks whether circuit breaker is active for an asset

*True if price feed is considered unreliable*


```solidity
mapping(address asset => bool broken) public circuitBroken;
```


### __gap
Reserved storage gap for future upgrades

*Required by OpenZeppelin's upgradeable contracts pattern*


```solidity
uint256[22] private __gap;
```


## Functions
### onlyListedAsset


```solidity
modifier onlyListedAsset(address asset);
```

### nonZeroAddress

Checks that an address is not zero


```solidity
modifier nonZeroAddress(address addr);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|The address to check|


### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

Initializes the contract with core configuration and access control settings

*This can only be called once through the proxy's initializer*

**Notes:**
- security: Sets up the initial access control roles:
- DEFAULT_ADMIN_ROLE: timelock
- MANAGER_ROLE: timelock
- UPGRADER_ROLE: multisig, timelock
- PAUSER_ROLE: multisig, timelock
- CIRCUIT_BREAKER_ROLE: timelock, multisig

- oracle-config: Initializes oracle configuration with the following defaults:
- freshnessThreshold: 28800 (8 hours)
- volatilityThreshold: 3600 (1 hour)
- volatilityPercentage: 20%
- circuitBreakerThreshold: 50%

- version: Sets initial contract version to 1


```solidity
function initialize(address timelock, address multisig, address usdc_) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timelock`|`address`|Address of the timelock contract that will have admin privileges|
|`multisig`|`address`|Address of the multisig wallet for emergency controls|
|`usdc_`|`address`||


### updateUniswapOracle

Register a Uniswap V3 pool as an oracle for an asset


```solidity
function updateUniswapOracle(address asset, address uniswapPool, uint32 twapPeriod, uint8 active)
    public
    nonZeroAddress(uniswapPool)
    onlyListedAsset(asset)
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to register the oracle for|
|`uniswapPool`|`address`|The Uniswap V3 pool address (must contain the asset)|
|`twapPeriod`|`uint32`|The TWAP period in seconds|
|`active`|`uint8`|isActive flag|


### updateChainlinkOracle

Add an oracle with type specification


```solidity
function updateChainlinkOracle(address asset, address oracle, uint8 active)
    external
    nonZeroAddress(oracle)
    onlyListedAsset(asset)
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to add the oracle for|
|`oracle`|`address`|The oracle address|
|`active`|`uint8`||


### updateMainOracleConfig

Updates the global oracle configuration parameters


```solidity
function updateMainOracleConfig(uint80 freshness, uint80 volatility, uint40 volatilityPct, uint40 circuitBreakerPct)
    external
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint80`|Maximum age allowed for oracle data (15m-24h)|
|`volatility`|`uint80`|Time window for volatility checks (5m-4h)|
|`volatilityPct`|`uint40`|Maximum allowed price change percentage (5-30%)|
|`circuitBreakerPct`|`uint40`|Price deviation to trigger circuit breaker (25-70%)|


### updateTierConfig

Updates rate configuration for a collateral tier


```solidity
function updateTierConfig(CollateralTier tier, uint256 jumpRate, uint256 liquidationFee)
    external
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to update|
|`jumpRate`|`uint256`|New jump rate (max 0.25e6 = 25%)|
|`liquidationFee`|`uint256`|New liquidation fee (max 0.1e6 = 10%)|


### setCoreAddress

Updates the core protocol contract address

*This function can only be called by the DEFAULT_ADMIN_ROLE when the contract is not paused*

**Notes:**
- security: Validates that the new address is not zero

- access: Restricted to DEFAULT_ADMIN_ROLE

- emits: CoreAddressUpdated event with the new core address


```solidity
function setCoreAddress(address newCore) external nonZeroAddress(newCore) onlyRole(DEFAULT_ADMIN_ROLE) whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCore`|`address`|Address of the new core protocol contract|


### pause

Pauses all contract operations

*This function can only be called by addresses with PAUSER_ROLE*

**Notes:**
- access: Restricted to PAUSER_ROLE

- security: Critical function that stops all state-changing operations


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses all contract operations

*This function can only be called by addresses with PAUSER_ROLE*

**Notes:**
- access: Restricted to PAUSER_ROLE

- security: Resumes normal contract operations


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### updateAssetConfig

Updates or adds a new asset configuration

*Validates all configuration parameters before updating*

**Notes:**
- security: Includes comprehensive parameter validation

- access: Restricted to MANAGER_ROLE

- pausable: Operation not allowed when contract is paused

- validation: Asset address cannot be zero

- emits: UpdateAssetConfig when configuration is updated


```solidity
function updateAssetConfig(address asset, Asset calldata config)
    external
    nonZeroAddress(asset)
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to configure|
|`config`|`Asset`|The complete asset configuration|


### updateAssetTier

Updates the collateral tier for an existing asset

*Changes risk parameters associated with the asset*

**Notes:**
- security: Only modifies tier assignment

- access: Restricted to MANAGER_ROLE

- pausable: Operation not allowed when contract is paused

- validation: Asset must be previously listed

- emits: AssetTierUpdated when tier is changed


```solidity
function updateAssetTier(address asset, CollateralTier newTier)
    external
    onlyListedAsset(asset)
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the listed asset to modify|
|`newTier`|`CollateralTier`|The new collateral tier to assign|


### setPrimaryOracle

Sets the primary oracle type for an asset

*Changes which oracle is used as the primary price source*

**Notes:**
- access: Restricted to MANAGER_ROLE

- pausable: Operation not allowed when contract is paused

- validation: Asset must be previously listed

- emits: PrimaryOracleSet when primary oracle is changed


```solidity
function setPrimaryOracle(address asset, OracleType oracleType)
    external
    onlyListedAsset(asset)
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to update|
|`oracleType`|`OracleType`|The oracle type to set as primary|


### triggerCircuitBreaker

Activates the circuit breaker for an asset

*Prevents price queries when activated*

**Notes:**
- access: Restricted to CIRCUIT_BREAKER_ROLE

- security: Emergency function to prevent using potentially manipulated prices

- emits: CircuitBreakerTriggered when activated


```solidity
function triggerCircuitBreaker(address asset) external onlyRole(CIRCUIT_BREAKER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to trigger circuit breaker for|


### resetCircuitBreaker

Deactivates the circuit breaker for an asset

*Allows price queries to resume*

**Notes:**
- access: Restricted to CIRCUIT_BREAKER_ROLE

- security: Should only be called after verifying price feed reliability

- emits: CircuitBreakerReset when deactivated


```solidity
function resetCircuitBreaker(address asset) external onlyRole(CIRCUIT_BREAKER_ROLE);
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
|`<none>`|`address`|The oracle address for the specified type, or address(0) if none exists|


### getAssetPriceByType

Get the price from a specific oracle type for an asset


```solidity
function getAssetPriceByType(address asset, OracleType oracleType)
    external
    view
    onlyListedAsset(asset)
    returns (uint256);
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

Get asset price as a view function (no state changes)


```solidity
function getAssetPrice(address asset) public view onlyListedAsset(asset) returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to get price for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|price The current price of the asset|


### scheduleUpgrade

Schedules an upgrade to a new implementation with timelock

*Only callable by addresses with UPGRADER_ROLE*


```solidity
function scheduleUpgrade(address newImplementation)
    external
    nonZeroAddress(newImplementation)
    onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


### cancelUpgrade

Cancels a previously scheduled upgrade

*Only callable by addresses with UPGRADER_ROLE*


```solidity
function cancelUpgrade() external onlyRole(UPGRADER_ROLE);
```

### upgradeTimelockRemaining

Returns the remaining time before a scheduled upgrade can be executed

*Returns 0 if no upgrade is scheduled or if the timelock has expired*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|timeRemaining The time remaining in seconds|


### getAssetDetails

Retrieves detailed information about an asset

*Combines multiple data points into a single view call*

**Note:**
validation: Asset must be listed


```solidity
function getAssetDetails(address asset)
    external
    view
    onlyListedAsset(asset)
    returns (uint256 price, uint256 totalSupplied, uint256 maxSupply, CollateralTier tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current oracle price of the asset|
|`totalSupplied`|`uint256`|Total amount of asset supplied to protocol|
|`maxSupply`|`uint256`|Maximum supply threshold for the asset|
|`tier`|`CollateralTier`|Collateral tier classification|


### getTierRates

Retrieves rates configuration for all collateral tiers

*Returns parallel arrays for jump rates and liquidation fees*


```solidity
function getTierRates() external view returns (uint256[4] memory jumpRates, uint256[4] memory liquidationFees);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`jumpRates`|`uint256[4]`|Array of jump rates for each tier [STABLE, CROSS_A, CROSS_B, ISOLATED]|
|`liquidationFees`|`uint256[4]`|Array of liquidation fees for each tier [STABLE, CROSS_A, CROSS_B, ISOLATED]|


### getTierJumpRate

Gets the jump rate for a specific collateral tier


```solidity
function getTierJumpRate(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The jump rate for the specified tier|


### isAssetValid

Checks if an asset is valid and active in the protocol


```solidity
function isAssetValid(address asset) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|true if the asset is listed and active, false otherwise|


### isAssetAtCapacity

Checks if supplying an amount would exceed asset capacity

**Note:**
validation: Asset must be listed


```solidity
function isAssetAtCapacity(address asset, uint256 amount) external view onlyListedAsset(asset) returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to check|
|`amount`|`uint256`|The amount to be supplied|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|true if supply would exceed maximum threshold|


### poolLiquidityLimit

Checks if an amount exceeds pool liquidity limits

*Only applicable for assets with active Uniswap oracle*


```solidity
function poolLiquidityLimit(address asset, uint256 amount) external view returns (bool limitReached);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to check|
|`amount`|`uint256`|The amount to validate|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`limitReached`|`bool`|true if amount exceeds 3% of pool liquidity|


### getAssetInfo

Retrieves complete configuration for an asset

*Returns full Asset struct from storage*

**Note:**
validation: Asset must be listed in protocol


```solidity
function getAssetInfo(address asset) external view onlyListedAsset(asset) returns (Asset memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Asset`|Complete Asset struct containing all configuration parameters|


### getListedAssets

Retrieves array of all listed asset addresses

*Converts EnumerableSet to memory array*

**Notes:**
- complexity: O(n) where n is number of listed assets

- gas-note: May be expensive for large numbers of assets


```solidity
function getListedAssets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array containing addresses of all listed assets|


### getLiquidationFee

Gets the liquidation fee for a specific collateral tier


```solidity
function getLiquidationFee(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The liquidation fee percentage (scaled by 1e6)|


### getAssetTier

Gets the collateral tier assigned to an asset

**Note:**
validation: Asset must be listed


```solidity
function getAssetTier(address asset) external view onlyListedAsset(asset) returns (CollateralTier tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier classification|


### getAssetDecimals

Gets the decimal precision of an asset

**Note:**
validation: Asset must be listed


```solidity
function getAssetDecimals(address asset) external view onlyListedAsset(asset) returns (uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The number of decimals (e.g., 18 for ETH)|


### getAssetLiquidationThreshold

Gets the liquidation threshold for an asset

**Note:**
validation: Asset must be listed


```solidity
function getAssetLiquidationThreshold(address asset) external view onlyListedAsset(asset) returns (uint16);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|The liquidation threshold percentage (scaled by 1e4)|


### getAssetBorrowThreshold

Gets the borrow threshold for an asset

**Note:**
validation: Asset must be listed


```solidity
function getAssetBorrowThreshold(address asset) external view onlyListedAsset(asset) returns (uint16);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|The borrow threshold percentage (scaled by 1e4)|


### getIsolationDebtCap

Gets the maximum allowed debt for an isolated asset

**Note:**
validation: Asset must be listed


```solidity
function getIsolationDebtCap(address asset) external view onlyListedAsset(asset) returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum debt cap in asset's native units|


### getAssetCalculationParams

Gets all parameters needed for collateral calculations in a single call

*Consolidates multiple getter calls into a single cross-contract call*


```solidity
function getAssetCalculationParams(address asset)
    external
    view
    onlyListedAsset(asset)
    returns (AssetCalculationParams memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`AssetCalculationParams`|Struct containing price, thresholds and decimals|


### getOracleCount

Gets the number of active oracles for an asset

*Returns sum of active Chainlink and Uniswap oracles (0-2)*

**Note:**
oracle-config: Sum of chainlinkConfig.active and poolConfig.active


```solidity
function getOracleCount(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total number of active oracle price feeds|


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
|`<none>`|`bool`|Whether the asset has a large price deviation|
|`<none>`|`uint256`||


### _initializeDefaultTierParameters

Initializes default parameters for all collateral tiers

*Called once during contract initialization*

**Notes:**
- rates: Sets the following default rates:
- STABLE: 5% jump rate, 1% liquidation fee
- CROSS_A: 8% jump rate, 2% liquidation fee
- CROSS_B: 12% jump rate, 3% liquidation fee
- ISOLATED: 15% jump rate, 4% liquidation fee

- security: All rates are scaled by 1e6 (100% = 1e6)


```solidity
function _initializeDefaultTierParameters() internal;
```

### _calculateMedianPrice

Calculate median price from oracles without modifying state


```solidity
function _calculateMedianPrice(address asset) internal view returns (uint256 median, uint256 deviation);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to get price for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`median`|`uint256`|The median price across all valid oracles|
|`deviation`|`uint256`|Maximum deviation between any two prices (instead of vs stored price)|


### _validateAssetConfig

Validate asset configuration parameters

*Centralized validation to ensure consistent checks across all configuration updates*


```solidity
function _validateAssetConfig(Asset calldata config) internal pure;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`config`|`Asset`|The asset configuration to validate|


### _getChainlinkPrice

Get price from Chainlink oracle


```solidity
function _getChainlinkPrice(address asset) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The Chainlink oracle address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The price with the specified decimals|


### _getUniswapTWAPPrice

Retrieves the Time-Weighted Average Price (TWAP) of an asset in USD using Uniswap V3

*Validates the Uniswap pool configuration and fetches the price using the TWAP period*

**Notes:**
- oracle: Uses Uniswap V3 TWAP oracle

- reverts: InvalidUniswapConfig if the pool is not configured or inactive

- reverts: OracleInvalidPrice if the price is invalid or zero


```solidity
function _getUniswapTWAPPrice(address asset) internal view returns (uint256 tokenPriceInUSD);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to fetch the price for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenPriceInUSD`|`uint256`|The price of the asset in USD (scaled to 1e6)|


### getAnyPoolTokenPriceInUSD

Retrieves the price of a token in USD from any Uniswap V3 pool

*Supports both USDC-based pools and ETH-based pools with USDC as a reference*

**Notes:**
- oracle: Uses Uniswap V3 TWAP oracle

- reverts: OracleInvalidPrice if the price is invalid or zero


```solidity
function getAnyPoolTokenPriceInUSD(address poolAddress, address token, address ethUsdcPool, uint32 twapPeriod)
    internal
    view
    returns (uint256 tokenPriceInUSD);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`poolAddress`|`address`|The address of the Uniswap V3 pool|
|`token`|`address`|The address of the token to fetch the price for|
|`ethUsdcPool`|`address`|The address of the ETH/USDC Uniswap V3 pool|
|`twapPeriod`|`uint32`|The TWAP period in seconds|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenPriceInUSD`|`uint256`|The price of the token in USD (scaled to 1e6)|


### getOptimalUniswapConfig

Determines the optimal configuration for a Uniswap V3 pool

*Identifies whether the asset is token0, its decimals, and if the pool is USDC-based*

**Notes:**
- validation: Ensures the asset is part of the pool

- reverts: "Asset not in pool" if the asset is not in the pool


```solidity
function getOptimalUniswapConfig(address asset, IUniswapV3Pool pool)
    internal
    view
    returns (bool isToken0, uint8 assetDecimals, bool isUsdcPool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to configure|
|`pool`|`IUniswapV3Pool`|The Uniswap V3 pool instance|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isToken0`|`bool`|True if the asset is token0 in the pool|
|`assetDecimals`|`uint8`|The number of decimals for the asset|
|`isUsdcPool`|`bool`|True if the pool is USDC-based|


### _validatePool

Validates that both asset and quote token are present in a Uniswap V3 pool


```solidity
function _validatePool(address asset, address uniswapPool) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset token address to validate|
|`uniswapPool`|`address`|The Uniswap V3 pool address|


### _authorizeUpgrade

Validates and authorizes contract upgrades

*Internal function required by UUPSUpgradeable pattern*

**Notes:**
- security: Enforces timelock and validates implementation address

- access: Restricted to UPGRADER_ROLE

- validation: Requires:
- Upgrade must be scheduled
- Implementation must match scheduled upgrade
- Timelock duration must have elapsed

- emits: Upgrade event on successful authorization

- state-changes: Increments version and clears pending upgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


