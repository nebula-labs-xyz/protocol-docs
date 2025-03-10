# LendefiAssets
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/lender/LendefiAssets.sol)

**Inherits:**
[ILendefiAssets](/contracts/interfaces/ILendefiAssets.sol/interface.ILendefiAssets.md), Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, PausableUpgradeable, UUPSUpgradeable

**Author:**
alexei@nebula-labs(dot)xyz

Manages asset configurations, listings, and oracle integrations

*Extracted component for asset-related functionality*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

- oz-upgrades: 


## State Variables
### MANAGER_ROLE
Role for managing asset configurations and parameters


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
Role for upgrading contract implementations


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### PAUSER_ROLE
Role for emergency pause functionality


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### version
Current version of the contract


```solidity
uint8 public version;
```


### coreAddress
Address of the core Lendefi contract


```solidity
address public coreAddress;
```


### oracleModule
Oracle module for asset price feeds


```solidity
ILendefiOracle public oracleModule;
```


### LendefiInstance

```solidity
IPROTOCOL internal LendefiInstance;
```


### listedAsset
Set of all assets listed in the protocol


```solidity
EnumerableSet.AddressSet internal listedAsset;
```


### assetInfo
Detailed configuration for each asset


```solidity
mapping(address => Asset) internal assetInfo;
```


### tierJumpRate
Base borrow rate for each collateral risk tier

*Higher tiers have higher interest rates due to increased risk*


```solidity
mapping(CollateralTier => uint256) public tierJumpRate;
```


### tierLiquidationFee
Liquidation bonus percentage for each collateral tier

*Higher risk tiers have larger liquidation bonuses*


```solidity
mapping(CollateralTier => uint256) public tierLiquidationFee;
```


## Functions
### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

Initializes the asset management module


```solidity
function initialize(address timelock, address oracle_, address guardian) external initializer;
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

**Notes:**
- security: This is a critical emergency function that should be carefully controlled

- access: Restricted to PAUSER_ROLE

- events: Emits a Paused event


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses module operations, returning to normal functioning

*Can only be called by accounts with PAUSER_ROLE*

**Notes:**
- security: Should only be called after thorough security verification

- access: Restricted to PAUSER_ROLE

- events: Emits an Unpaused event


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### updateOracleModule

Updates the oracle module address

*Only callable by accounts with MANAGER_ROLE*


```solidity
function updateOracleModule(address newOracle) external onlyRole(MANAGER_ROLE) whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOracle`|`address`|New oracle module address|


### updateAssetConfig

Updates or adds a new asset configuration in the protocol

*Manages all configuration parameters for an asset in a single function*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Adds asset to listedAsset set if not already present


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
) external onlyRole(MANAGER_ROLE) whenNotPaused;
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
function updateAssetTier(address asset, CollateralTier newTier) external onlyRole(MANAGER_ROLE) whenNotPaused;
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
function addAssetOracle(address asset, address oracle, uint8 decimals_) external onlyRole(MANAGER_ROLE) whenNotPaused;
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
function removeAssetOracle(address asset, address oracle) external onlyRole(MANAGER_ROLE) whenNotPaused;
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
function setPrimaryAssetOracle(address asset, address oracle) external onlyRole(MANAGER_ROLE) whenNotPaused;
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
function updateOracleTimeThresholds(uint256 freshness, uint256 volatility)
    external
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint256`|Maximum age for all price data (in seconds)|
|`volatility`|`uint256`|Maximum age for volatile price data (in seconds)|


### setCoreAddress

Updates the core Lendefi contract address

*Only callable by accounts with DEFAULT_ADMIN_ROLE*


```solidity
function setCoreAddress(address newCore) external onlyRole(DEFAULT_ADMIN_ROLE) whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCore`|`address`|New Lendefi core address|


### updateTierParameters

Updates the risk parameters for a collateral tier

*Updates both interest rates and liquidation incentives for a specific risk tier*

**Note:**
security: Can only be called by accounts with MANAGER_ROLE


```solidity
function updateTierParameters(CollateralTier tier, uint256 jumpRate, uint256 liquidationFee)
    external
    onlyRole(MANAGER_ROLE)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to update|
|`jumpRate`|`uint256`|The new base borrow rate for the tier (in parts per million)|
|`liquidationFee`|`uint256`|The new liquidation bonus for the tier (in parts per million)|


### getTierRates

Retrieves the current borrow rates and liquidation bonuses for all collateral tiers

*Returns two fixed arrays containing rates for ISOLATED, CROSS_A, CROSS_B, and STABLE tiers in that order*


```solidity
function getTierRates() external view returns (uint256[4] memory jumpRates, uint256[4] memory liquidationFees);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`jumpRates`|`uint256[4]`|Array of borrow rates for each tier [ISOLATED, CROSS_A, CROSS_B, STABLE]|
|`liquidationFees`|`uint256[4]`|Array of liquidation bonuses for each tier [ISOLATED, CROSS_A, CROSS_B, STABLE]|


### getTierLiquidationFee

Gets the base liquidation bonus percentage for a specific collateral tier


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
|`<none>`|`uint256`|uint256 The liquidation bonus rate in parts per million (e.g., 0.05e6 = 5%)|


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
|`<none>`|`uint256`|uint256 The jump rate in parts per million|


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


### getAssetPrice

Gets the current USD price for an asset from the oracle module

*Uses the oracle module to get the median price from multiple sources*


```solidity
function getAssetPrice(address asset) public returns (uint256);
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
function getAssetPriceOracle(address oracle) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the Chainlink price feed oracle|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Price from the oracle (use getAssetPrice instead)|


### _initializeDefaultTierParameters

Initializes tier parameters with default values

*Sets up default interest rates and liquidation fees for all tiers*

**Note:**
security: Should only be called once during initialization


```solidity
function _initializeDefaultTierParameters() internal;
```

### _authorizeUpgrade

*Authorizes an upgrade to a new implementation*

**Note:**
access: Restricted to UPGRADER_ROLE


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


