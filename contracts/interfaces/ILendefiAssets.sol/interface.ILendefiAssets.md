# ILendefiAssets
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/interfaces/ILendefiAssets.sol)

Interface for the LendefiAssets module which manages asset configurations and oracle integrations

*Contains all external facing functions, events, errors and data structures*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Functions
### initialize

Initializes the asset management module


```solidity
function initialize(address timelock, address oracle_, address guardian) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timelock`|`address`|Address with MANAGER_ROLE for asset configuration|
|`oracle_`|`address`|Address of oracle module for price feeds|
|`guardian`|`address`|Address with PAUSER_ROLE for emergency functions|


### pause

Pauses all module operations in case of emergency

*Can only be called by accounts with PAUSER_ROLE*

**Note:**
security: This is a critical emergency function that should be carefully controlled


```solidity
function pause() external;
```

### unpause

Unpauses module operations, returning to normal functioning

*Can only be called by accounts with PAUSER_ROLE*

**Note:**
security: Should only be called after thorough security verification


```solidity
function unpause() external;
```

### getLiquidationFee

Gets the base liquidation bonus percentage for a specific collateral tier

*Alias of getTierLiquidationFee for backward compatibility*


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
|`<none>`|`uint256`|uint256 The liquidation bonus rate in parts per million (e.g., 0.05e6 = 5%)|


### updateOracleModule

Updates the oracle module address

*Only callable by accounts with MANAGER_ROLE*


```solidity
function updateOracleModule(address newOracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOracle`|`address`|New oracle module address|


### updateAssetConfig

Updates or adds a new asset configuration in the protocol

*Manages all configuration parameters for an asset in a single function*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function updateAssetConfig(
    address asset,
    address oracle_,
    uint8 oracleDecimals,
    uint8 assetDecimals,
    uint8 active,
    uint32 borrowThreshold,
    uint32 liquidationThreshold,
    uint256 maxSupplyLimit,
    CollateralTier tier,
    uint256 isolationDebtCap
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the token to configure|
|`oracle_`|`address`|Address of the Chainlink price feed for asset/USD|
|`oracleDecimals`|`uint8`|Number of decimals in the oracle price feed|
|`assetDecimals`|`uint8`|Number of decimals in the asset token|
|`active`|`uint8`|Whether the asset is enabled (1) or disabled (0)|
|`borrowThreshold`|`uint32`|LTV ratio for borrowing (e.g., 870 = 87%)|
|`liquidationThreshold`|`uint32`|LTV ratio for liquidation (e.g., 920 = 92%)|
|`maxSupplyLimit`|`uint256`|Maximum amount of this asset allowed in protocol|
|`tier`|`CollateralTier`|Risk category of the asset (STABLE, CROSS_A, CROSS_B, ISOLATED)|
|`isolationDebtCap`|`uint256`|Maximum debt allowed when used in isolation mode|


### updateAssetTier

Updates the risk tier classification for a listed asset

*Changes the risk classification which affects interest rates and liquidation parameters*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function updateAssetTier(address asset, CollateralTier newTier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to update|
|`newTier`|`CollateralTier`|The new CollateralTier to assign to the asset|


### addAssetOracle

Adds an additional oracle data source for an asset

*Allows adding secondary or backup oracles to enhance price reliability*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function addAssetOracle(address asset, address oracle, uint8 decimals_) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the Chainlink price feed to add|
|`decimals_`|`uint8`|Number of decimals in the oracle price feed|


### removeAssetOracle

Removes an oracle data source for an asset

*Allows removing unreliable or deprecated oracles*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function removeAssetOracle(address asset, address oracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the Chainlink price feed to remove|


### setPrimaryAssetOracle

Sets the primary oracle for an asset

*The primary oracle is used as a fallback when median calculation fails*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function setPrimaryAssetOracle(address asset, address oracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the Chainlink price feed to set as primary|


### updateOracleTimeThresholds

Updates oracle time thresholds

*Controls how old price data can be before rejection*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function updateOracleTimeThresholds(uint256 freshness, uint256 volatility) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint256`|Maximum age for all price data (in seconds)|
|`volatility`|`uint256`|Maximum age for volatile price data (in seconds)|


### isAssetValid

Validates asset is listed and active

*Core contracts should call this before accepting asset deposits*


```solidity
function isAssetValid(address asset) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if asset exists and is active|


### isAssetAtCapacity

Checks if a given asset is at capacity


```solidity
function isAssetAtCapacity(address asset, uint256 additionalAmount) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to check|
|`additionalAmount`|`uint256`|Amount potentially being added|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if adding amount would exceed max supply threshold|


### getAssetPrice

Gets the current USD price for an asset from the oracle module

*Uses the oracle module to get the median price from multiple sources*


```solidity
function getAssetPrice(address asset) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The asset price in USD (scaled by oracle decimals)|


### getAssetPriceOracle

DEPRECATED: Direct oracle price access

*This function is maintained for backward compatibility*


```solidity
function getAssetPriceOracle(address oracle) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the Chainlink price feed oracle|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Price from the oracle (use getAssetPrice instead)|


### getAssetInfo

Retrieves complete configuration details for a listed asset

*Returns the full Asset struct from assetInfo mapping*


```solidity
function getAssetInfo(address asset) external view returns (Asset memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Asset`|Asset struct containing all configuration parameters|


### getAssetDetails

Retrieves detailed information about a listed asset

*Aggregates asset configuration and current state into a single view*


```solidity
function getAssetDetails(address asset)
    external
    view
    returns (uint256 price, uint256 totalSupplied, uint256 maxSupply, CollateralTier tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current USD price from oracle|
|`totalSupplied`|`uint256`|Total amount of asset supplied as collateral|
|`maxSupply`|`uint256`|Maximum supply threshold allowed|
|`tier`|`CollateralTier`|Risk classification tier of the asset|


### getListedAssets

Returns an array of all listed asset addresses in the protocol

*Retrieves assets from the EnumerableSet storing listed assets*

**Note:**
complexity: O(n) where n is the number of listed assets


```solidity
function getListedAssets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of addresses representing all listed assets|


### isIsolationAsset

Checks if an asset is in isolation mode


```solidity
function isIsolationAsset(address asset) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if asset is configured for ISOLATED tier|


### getIsolationDebtCap

Gets the isolation debt cap for an asset


```solidity
function getIsolationDebtCap(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum debt allowed in isolation mode|


### version

Returns the current version of the contract


```solidity
function version() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|Current version number|


### oracleModule

Returns the address of the oracle module


```solidity
function oracleModule() external view returns (ILendefiOracle);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ILendefiOracle`|Address of the oracle module contract|


### setCoreAddress

Updates the core Lendefi contract address

*Only callable by accounts with DEFAULT_ADMIN_ROLE*


```solidity
function setCoreAddress(address newCore) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCore`|`address`|New Lendefi core address|


### tierJumpRate

Gets the interest rate jump multiplier for a specific collateral tier


```solidity
function tierJumpRate(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Jump rate multiplier applied to base interest rate|


### tierLiquidationFee

Gets the liquidation fee percentage for a specific collateral tier


```solidity
function tierLiquidationFee(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Liquidation fee as a percentage in basis points|


### updateTierParameters

Updates the risk parameters for a specific collateral tier

*Only callable by accounts with MANAGER_ROLE*


```solidity
function updateTierParameters(CollateralTier tier, uint256 jumpRate, uint256 liquidationFee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The tier to update|
|`jumpRate`|`uint256`|New jump rate multiplier for interest calculation|
|`liquidationFee`|`uint256`|New liquidation fee percentage for the tier|


### getTierRates

Gets all tier rates in a single call

*Returns arrays indexed by tier enum values (0=STABLE, 1=CROSS_A, etc.)*


```solidity
function getTierRates() external view returns (uint256[4] memory jumpRates, uint256[4] memory liquidationFees);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`jumpRates`|`uint256[4]`|Array of jump rates for all tiers|
|`liquidationFees`|`uint256[4]`|Array of liquidation fees for all tiers|


### getTierLiquidationFee

Gets the liquidation fee for a specific collateral tier

*Main function for getting liquidation fees, preferred over getLiquidationFee*


```solidity
function getTierLiquidationFee(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Liquidation fee percentage in basis points|


### getTierJumpRate

Gets the jump rate multiplier for a specific collateral tier

*Used in interest rate model calculations*


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
|`<none>`|`uint256`|Jump rate multiplier for interest calculations|


## Events
### UpdateAssetConfig
Emitted when an asset's configuration is updated


```solidity
event UpdateAssetConfig(address indexed asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset that was updated|

### AssetTierUpdated
Emitted when an asset's risk tier is updated


```solidity
event AssetTierUpdated(address indexed asset, CollateralTier tier);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset that was updated|
|`tier`|`CollateralTier`|The new tier assigned to the asset|

### CoreAddressUpdated
Emitted when the core protocol address is updated


```solidity
event CoreAddressUpdated(address indexed newCore);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCore`|`address`|The address of the new core protocol contract|

### OracleModuleUpdated
Emitted when the oracle module address is updated


```solidity
event OracleModuleUpdated(address indexed newOracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOracle`|`address`|The address of the new oracle module|

### Upgrade
Emitted when the contract implementation is upgraded


```solidity
event Upgrade(address indexed upgrader, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`upgrader`|`address`|The address that initiated the upgrade|
|`implementation`|`address`|The address of the new implementation|

### TierParametersUpdated
Emitted when tier parameters are updated


```solidity
event TierParametersUpdated(CollateralTier tier, uint256 borrowRate, uint256 liquidationFee);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier that was updated|
|`borrowRate`|`uint256`|The new borrow rate for the tier|
|`liquidationFee`|`uint256`|The new liquidation fee for the tier|

## Errors
### RateTooHigh
Error thrown when a proposed interest rate exceeds maximum allowed value


```solidity
error RateTooHigh(uint256 provided, uint256 maximum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint256`|The provided rate value|
|`maximum`|`uint256`|The maximum allowed rate value|

### FeeTooHigh
Error thrown when a proposed fee exceeds maximum allowed value


```solidity
error FeeTooHigh(uint256 provided, uint256 maximum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint256`|The provided fee value|
|`maximum`|`uint256`|The maximum allowed fee value|

### AssetNotListed
Error thrown when attempting to interact with an asset that is not listed in the protocol


```solidity
error AssetNotListed(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the non-listed asset|

### ZeroAddressNotAllowed
Error thrown when attempting to set a critical address to the zero address


```solidity
error ZeroAddressNotAllowed();
```

## Structs
### Asset
Asset configuration parameters

*Compact struct for gas efficiency using smaller uint types where possible*


```solidity
struct Asset {
    uint8 active;
    uint8 decimals;
    uint8 oracleDecimals;
    uint32 borrowThreshold;
    uint32 liquidationThreshold;
    address oracleUSD;
    uint256 maxSupplyThreshold;
    CollateralTier tier;
    uint256 isolationDebtCap;
}
```

## Enums
### CollateralTier
Risk tiers for collateral assets in ascending order of risk

*Higher enum value = higher risk tier*

**Note:**
values: 
- STABLE (0): Lowest risk, stablecoins
- CROSS_A (1): Low risk assets
- CROSS_B (2): Medium risk assets
- ISOLATED (3): High risk assets


```solidity
enum CollateralTier {
    STABLE,
    CROSS_A,
    CROSS_B,
    ISOLATED
}
```

