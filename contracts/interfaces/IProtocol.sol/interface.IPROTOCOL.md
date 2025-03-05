# IPROTOCOL
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/IProtocol.sol)

**Inherits:**
IERC20


## Functions
### initialize

Initializes the protocol with core dependencies and parameters

*Sets up ERC20 token details, access control roles, and default protocol parameters*


```solidity
function initialize(
    address usdc,
    address govToken,
    address ecosystem,
    address treasury_,
    address timelock_,
    address guardian,
    address oracle_
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`usdc`|`address`|The address of the USDC stablecoin used for borrowing and liquidity|
|`govToken`|`address`|The address of the governance token used for liquidator eligibility|
|`ecosystem`|`address`|The address of the ecosystem contract that manages rewards|
|`treasury_`|`address`|The address of the treasury that collects protocol fees|
|`timelock_`|`address`|The address of the timelock contract for governance actions|
|`guardian`|`address`|The address of the initial admin with pausing capability|
|`oracle_`|`address`|The address of the oracle module for price feeds|


### pause

Pauses all protocol operations in case of emergency

*Can only be called by authorized governance roles*


```solidity
function pause() external;
```

### unpause

Unpauses the protocol to resume normal operations

*Can only be called by authorized governance roles*


```solidity
function unpause() external;
```

### flashLoan

Executes a flash loan, allowing borrowing without collateral if repaid in same transaction

*Receiver must implement IFlashLoanReceiver interface*


```solidity
function flashLoan(address receiver, address token, uint256 amount, bytes calldata params) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`receiver`|`address`|The contract address that will receive the flash loaned tokens|
|`token`|`address`|The address of the token to borrow (currently only supports USDC)|
|`amount`|`uint256`|The amount of tokens to flash loan|
|`params`|`bytes`|Arbitrary data to pass to the receiver contract|


### updateFlashLoanFee

Updates the fee charged for flash loans

*Can only be called by authorized governance roles*


```solidity
function updateFlashLoanFee(uint256 newFee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newFee`|`uint256`|The new flash loan fee (scaled by 1000, e.g., 5 = 0.5%)|


### updateBaseProfitTarget

Updates the target profit rate for the protocol

*Can only be called by authorized governance roles*


```solidity
function updateBaseProfitTarget(uint256 rate) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|The new base profit target rate (scaled by RAY)|


### updateBaseBorrowRate

Updates the base interest rate charged on borrowing

*Can only be called by authorized governance roles*


```solidity
function updateBaseBorrowRate(uint256 rate) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|The new base borrow rate (scaled by RAY)|


### updateTargetReward

Updates the target reward amount for liquidity providers

*Can only be called by authorized governance roles*


```solidity
function updateTargetReward(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new target reward amount per distribution interval|


### updateRewardInterval

Updates the time interval between reward distributions

*Can only be called by authorized governance roles*


```solidity
function updateRewardInterval(uint256 interval) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interval`|`uint256`|The new reward interval in seconds|


### updateRewardableSupply

Updates the minimum liquidity threshold required to be eligible for rewards

*Can only be called by authorized governance roles*


```solidity
function updateRewardableSupply(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new minimum liquidity threshold|


### updateLiquidatorThreshold

Updates the minimum governance token threshold required to be a liquidator

*Can only be called by authorized governance roles*


```solidity
function updateLiquidatorThreshold(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new liquidator threshold amount|


### updateTierParameters

Updates the borrowing rate and liquidation bonus parameters for a collateral tier

*Can only be called by authorized governance roles*


```solidity
function updateTierParameters(CollateralTier tier, uint256 borrowRate, uint256 liquidationFee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to update|
|`borrowRate`|`uint256`|The new borrow rate multiplier (scaled by RAY)|
|`liquidationFee`|`uint256`|The new liquidation bonus percentage (scaled by 1000)|


### updateAssetTier

Updates the risk classification tier of an asset

*Can only be called by authorized governance roles*


```solidity
function updateAssetTier(address asset, CollateralTier newTier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to update|
|`newTier`|`CollateralTier`|The new collateral tier to assign to the asset|


### updateAssetConfig

Updates or adds a new collateral asset with all configuration parameters

*Can only be called by authorized governance roles*


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
|`asset`|`address`|The address of the asset to configure|
|`oracle_`|`address`|The address of the price oracle for this asset|
|`oracleDecimals`|`uint8`|The decimal precision of the price oracle|
|`assetDecimals`|`uint8`|The decimal precision of the asset token|
|`active`|`uint8`|Whether the asset is active (1) or disabled (0)|
|`borrowThreshold`|`uint32`|The LTV ratio for borrowing (scaled by 1000)|
|`liquidationThreshold`|`uint32`|The LTV ratio for liquidation (scaled by 1000)|
|`maxSupplyLimit`|`uint256`|The maximum amount that can be supplied as collateral|
|`tier`|`CollateralTier`|The risk classification tier of the asset|
|`isolationDebtCap`|`uint256`|The maximum debt allowed when used in isolation mode|


### supplyLiquidity

Allows users to supply liquidity (USDC) to the protocol

*Mints LP tokens representing the user's share of the liquidity pool*


```solidity
function supplyLiquidity(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of USDC to supply|


### exchange

Allows users to withdraw liquidity by burning LP tokens


```solidity
function exchange(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of LP tokens to burn|


### supplyCollateral

Allows users to supply collateral assets to a borrowing position


```solidity
function supplyCollateral(address asset, uint256 amount, uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the collateral asset to supply|
|`amount`|`uint256`|The amount of the asset to supply|
|`positionId`|`uint256`|The ID of the position to supply collateral to|


### withdrawCollateral

Allows users to withdraw collateral assets from a borrowing position

*Will revert if withdrawal would make position undercollateralized*


```solidity
function withdrawCollateral(address asset, uint256 amount, uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the collateral asset to withdraw|
|`amount`|`uint256`|The amount of the asset to withdraw|
|`positionId`|`uint256`|The ID of the position to withdraw from|


### createPosition

Creates a new borrowing position for the caller


```solidity
function createPosition(address asset, bool isIsolated) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the initial collateral asset|
|`isIsolated`|`bool`|Whether to create the position in isolation mode|


### borrow

Allows borrowing stablecoins against collateral in a position

*Will revert if borrowing would exceed the position's credit limit*


```solidity
function borrow(uint256 positionId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to borrow against|
|`amount`|`uint256`|The amount of stablecoins to borrow|


### repay

Allows users to repay debt on a borrowing position


```solidity
function repay(uint256 positionId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to repay debt for|
|`amount`|`uint256`|The amount of debt to repay|


### exitPosition

Closes a position after all debt is repaid and withdraws remaining collateral

*Position must have zero debt to be closed*


```solidity
function exitPosition(uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to close|


### liquidate

Liquidates an undercollateralized position

*Caller must hold sufficient governance tokens to be eligible as a liquidator*


```solidity
function liquidate(address user, uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to liquidate|


### getAssetInfo

Retrieves the configuration data for a collateral asset


```solidity
function getAssetInfo(address asset) external view returns (Asset memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Asset`|Asset struct containing all configuration parameters|


### getAssetPrice

Gets the current USD price of an asset


```solidity
function getAssetPrice(address asset) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The asset price in USD (scaled by the asset's oracle decimals)|


### getUserPositionsCount

Gets the total number of positions created by a user


```solidity
function getUserPositionsCount(address user) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The number of positions the user has created|


### getUserPositions

Gets all positions created by a user


```solidity
function getUserPositions(address user) external view returns (UserPosition[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition[]`|An array of UserPosition structs|


### getUserPosition

Gets a specific position's data


```solidity
function getUserPosition(address user, uint256 positionId) external view returns (UserPosition memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition`|UserPosition struct containing position data|


### getUserCollateralAmount

Gets the amount of a specific asset in a position


```solidity
function getUserCollateralAmount(address user, uint256 positionId, address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|
|`asset`|`address`|The address of the collateral asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of the asset in the position|


### getProtocolSnapshot

Gets the current state of all protocol parameters


```solidity
function getProtocolSnapshot() external view returns (ProtocolSnapshot memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ProtocolSnapshot`|ProtocolSnapshot struct with current protocol state|


### calculateDebtWithInterest

Calculates the current debt amount including accrued interest


```solidity
function calculateDebtWithInterest(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total debt amount with interest|


### getPositionLiquidationFee

Calculates the liquidation fee for a position


```solidity
function getPositionLiquidationFee(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The liquidation fee amount|


### calculateCreditLimit

Calculates the maximum amount a user can borrow against their position


```solidity
function calculateCreditLimit(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum borrowing capacity (credit limit)|


### isLiquidatable

Checks if a position is eligible for liquidation


```solidity
function isLiquidatable(address user, uint256 positionId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the position can be liquidated, false otherwise|


### getAssetDetails

Gets detailed information about an asset


```solidity
function getAssetDetails(address asset)
    external
    view
    returns (
        uint256 price,
        uint256 totalSupplied,
        uint256 maxSupply,
        uint256 borrowRate,
        uint256 liquidationFee,
        CollateralTier tier
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|The current USD price|
|`totalSupplied`|`uint256`|The total amount supplied as collateral|
|`maxSupply`|`uint256`|The maximum supply threshold|
|`borrowRate`|`uint256`|The current borrow interest rate|
|`liquidationFee`|`uint256`|The liquidation bonus percentage|
|`tier`|`CollateralTier`|The collateral tier classification|


### getLPInfo

Gets information about a user's LP token position


```solidity
function getLPInfo(address user)
    external
    view
    returns (
        uint256 lpTokenBalance,
        uint256 usdcValue,
        uint256 lastAccrualTime,
        bool isRewardEligible,
        uint256 pendingRewards
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lpTokenBalance`|`uint256`|The user's LP token balance|
|`usdcValue`|`uint256`|The USDC value of the LP tokens|
|`lastAccrualTime`|`uint256`|The timestamp of last interest accrual|
|`isRewardEligible`|`bool`|Whether the user is eligible for rewards|
|`pendingRewards`|`uint256`|The pending reward amount|


### healthFactor

Calculates the health factor of a borrowing position

*Health factor > 1 means position is healthy, < 1 means liquidatable*


```solidity
function healthFactor(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The position's health factor (scaled by WAD)|


### getPositionCollateralAssets

Gets all collateral assets in a position


```solidity
function getPositionCollateralAssets(address user, uint256 positionId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|An array of asset addresses in the position|


### getPositionDebt

Gets the current debt amount for a position


```solidity
function getPositionDebt(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The current debt amount including interest|


### getUtilization

Calculates the current utilization rate of the protocol

*Utilization = totalBorrow / totalSuppliedLiquidity*


```solidity
function getUtilization() external view returns (uint256 u);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`u`|`uint256`|The utilization rate (scaled by WAD)|


### getSupplyRate

Gets the current supply interest rate


```solidity
function getSupplyRate() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The supply interest rate (scaled by RAY)|


### getBorrowRate

Gets the current borrow interest rate for a specific tier


```solidity
function getBorrowRate(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The borrow interest rate (scaled by RAY)|


### isRewardable

Checks if a user is eligible for rewards


```solidity
function isRewardable(address user) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if user is eligible for rewards, false otherwise|


### getTierLiquidationFee

Gets the liquidation fee percentage for a collateral tier


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
|`<none>`|`uint256`|The liquidation fee percentage (scaled by 1000)|


### getAssetPriceOracle

Gets the price from a specific oracle

*Validates the oracle data is fresh and within expected range*


```solidity
function getAssetPriceOracle(address oracle) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the price oracle|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The price returned by the oracle|


### getHighestTier

Determines the highest risk tier among collateral assets in a position


```solidity
function getHighestTier(address user, uint256 positionId) external view returns (CollateralTier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CollateralTier`|The highest risk CollateralTier in the position|


### getTierRates

Gets all configured rates for each collateral tier


```solidity
function getTierRates() external view returns (uint256[4] memory borrowRates, uint256[4] memory liquidationFeees);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`borrowRates`|`uint256[4]`|Array of borrow rates for each tier (scaled by RAY)|
|`liquidationFeees`|`uint256[4]`|Array of liquidation bonuses for each tier (scaled by 1000)|


### getListedAssets

Gets all assets listed in the protocol


```solidity
function getListedAssets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|An array of asset addresses|


### version

Gets the current protocol version


```solidity
function version() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The protocol version number|


### totalBorrow

Gets the total amount borrowed from the protocol


```solidity
function totalBorrow() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total borrowed amount|


### totalSuppliedLiquidity

Gets the total liquidity supplied to the protocol


```solidity
function totalSuppliedLiquidity() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total supplied liquidity|


### totalAccruedBorrowerInterest

Gets the total interest accrued by borrowers


```solidity
function totalAccruedBorrowerInterest() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total accrued borrower interest|


### totalAccruedSupplierInterest

Gets the total interest accrued by suppliers


```solidity
function totalAccruedSupplierInterest() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total accrued supplier interest|


### withdrawnLiquidity

Gets the total liquidity withdrawn from the protocol


```solidity
function withdrawnLiquidity() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total withdrawn liquidity|


### targetReward

Gets the target reward amount per distribution interval


```solidity
function targetReward() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The target reward amount|


### rewardInterval

Gets the time interval between reward distributions


```solidity
function rewardInterval() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The reward interval in seconds|


### rewardableSupply

Gets the minimum liquidity threshold required to be eligible for rewards


```solidity
function rewardableSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The rewardable supply threshold|


### baseBorrowRate

Gets the base interest rate charged on borrowing


```solidity
function baseBorrowRate() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The base borrow rate (scaled by RAY)|


### baseProfitTarget

Gets the target profit rate for the protocol


```solidity
function baseProfitTarget() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The base profit target (scaled by RAY)|


### liquidatorThreshold

Gets the minimum governance token threshold required to be a liquidator


```solidity
function liquidatorThreshold() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The liquidator threshold amount|


### flashLoanFee

Gets the current fee charged for flash loans


```solidity
function flashLoanFee() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The flash loan fee (scaled by 1000)|


### totalFlashLoanFees

Gets the total fees collected from flash loans


```solidity
function totalFlashLoanFees() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total flash loan fees collected|


### treasury

Gets the address of the treasury contract


```solidity
function treasury() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The treasury contract address|


### assetTVL

Gets the total value locked for a specific asset


```solidity
function assetTVL(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total value locked (TVL) amount|


### addAssetOracle

Adds an additional oracle data source for an asset

*Allows adding secondary or backup oracles to enhance price reliability*


```solidity
function addAssetOracle(address asset, address oracle, uint8 decimals) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the Chainlink price feed to add|
|`decimals`|`uint8`|Number of decimals in the oracle price feed|


### removeAssetOracle

Removes an oracle data source for an asset

*Allows removing unreliable or deprecated oracles*


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


```solidity
function updateOracleTimeThresholds(uint256 freshness, uint256 volatility) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint256`|Maximum age for all price data (in seconds)|
|`volatility`|`uint256`|Maximum age for volatile price data (in seconds)|


## Events
### Initialized
Emitted when protocol is initialized


```solidity
event Initialized(address indexed admin);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|Address of the admin who initialized the contract|

### Upgrade
Emitted when implementation contract is upgraded


```solidity
event Upgrade(address indexed admin, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|Address of the admin who performed the upgrade|
|`implementation`|`address`|Address of the new implementation|

### SupplyLiquidity
Emitted when a user supplies liquidity to the protocol


```solidity
event SupplyLiquidity(address indexed supplier, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`supplier`|`address`|Address of the liquidity supplier|
|`amount`|`uint256`|Amount of USDC supplied|

### Exchange
Emitted when LP tokens are exchanged for underlying assets


```solidity
event Exchange(address indexed exchanger, uint256 amount, uint256 value);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`exchanger`|`address`|Address of the user exchanging tokens|
|`amount`|`uint256`|Amount of LP tokens exchanged|
|`value`|`uint256`|Value received in exchange|

### SupplyCollateral
Emitted when collateral is supplied to a position


```solidity
event SupplyCollateral(address indexed user, uint256 indexed positionId, address indexed asset, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position|
|`asset`|`address`|Address of the supplied collateral asset|
|`amount`|`uint256`|Amount of collateral supplied|

### WithdrawCollateral
Emitted when collateral is withdrawn from a position


```solidity
event WithdrawCollateral(address indexed user, uint256 indexed positionId, address indexed asset, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position|
|`asset`|`address`|Address of the withdrawn collateral asset|
|`amount`|`uint256`|Amount of collateral withdrawn|

### PositionCreated
Emitted when a new borrowing position is created


```solidity
event PositionCreated(address indexed user, uint256 indexed positionId, bool isIsolated);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the newly created position|
|`isIsolated`|`bool`|Whether the position was created in isolation mode|

### PositionClosed
Emitted when a position is closed


```solidity
event PositionClosed(address indexed user, uint256 indexed positionId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the closed position|

### Borrow
Emitted when a user borrows from a position


```solidity
event Borrow(address indexed user, uint256 indexed positionId, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position|
|`amount`|`uint256`|Amount borrowed|

### Repay
Emitted when debt is repaid


```solidity
event Repay(address indexed user, uint256 indexed positionId, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position|
|`amount`|`uint256`|Amount repaid|

### InterestAccrued
Emitted when interest is accrued on a position


```solidity
event InterestAccrued(address indexed user, uint256 indexed positionId, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position|
|`amount`|`uint256`|Interest amount accrued|

### Reward
Emitted when rewards are distributed


```solidity
event Reward(address indexed user, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the reward recipient|
|`amount`|`uint256`|Reward amount distributed|

### FlashLoan
Emitted when a flash loan is executed


```solidity
event FlashLoan(
    address indexed initiator, address indexed receiver, address indexed token, uint256 amount, uint256 fee
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initiator`|`address`|Address that initiated the flash loan|
|`receiver`|`address`|Contract receiving the flash loan|
|`token`|`address`|Address of the borrowed token|
|`amount`|`uint256`|Amount borrowed|
|`fee`|`uint256`|Fee charged for the flash loan|

### UpdateBaseProfitTarget
Emitted when the base profit target is updated


```solidity
event UpdateBaseProfitTarget(uint256 rate);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|New base profit target rate|

### UpdateBaseBorrowRate
Emitted when the base borrow rate is updated


```solidity
event UpdateBaseBorrowRate(uint256 rate);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|New base borrow rate|

### UpdateTargetReward
Emitted when the target reward amount is updated


```solidity
event UpdateTargetReward(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|New target reward amount|

### UpdateRewardInterval
Emitted when the reward interval is updated


```solidity
event UpdateRewardInterval(uint256 interval);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interval`|`uint256`|New reward interval in seconds|

### UpdateRewardableSupply
Emitted when the rewardable supply threshold is updated


```solidity
event UpdateRewardableSupply(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|New rewardable supply threshold|

### UpdateLiquidatorThreshold
Emitted when the liquidator governance token threshold is updated


```solidity
event UpdateLiquidatorThreshold(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|New liquidator threshold|

### UpdateFlashLoanFee
Emitted when the flash loan fee is updated


```solidity
event UpdateFlashLoanFee(uint256 fee);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|New flash loan fee (scaled by 1000)|

### TierParametersUpdated
Emitted when tier parameters are updated


```solidity
event TierParametersUpdated(CollateralTier tier, uint256 borrowRate, uint256 liquidationFee);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|Collateral tier being updated|
|`borrowRate`|`uint256`|New borrow rate for the tier|
|`liquidationFee`|`uint256`|New liquidation bonus for the tier|

### AssetTierUpdated
Emitted when an asset's tier classification is updated


```solidity
event AssetTierUpdated(address indexed asset, CollateralTier indexed newTier);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset being updated|
|`newTier`|`CollateralTier`|New collateral tier assigned to the asset|

### UpdateAssetConfig
Emitted when an asset's configuration is updated


```solidity
event UpdateAssetConfig(address indexed asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the updated asset|

### TVLUpdated
Emitted when an asset's TVL is updated


```solidity
event TVLUpdated(address indexed asset, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`amount`|`uint256`|New TVL amount|

### OracleAdded
Emitted when an additional oracle is added for an asset


```solidity
event OracleAdded(address indexed asset, address indexed oracle, uint8 decimals);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle being added|
|`decimals`|`uint8`|Number of decimals in the oracle price feed|

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
Emitted when a new primary oracle is set for an asset


```solidity
event PrimaryOracleSet(address indexed asset, address indexed oracle);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset|
|`oracle`|`address`|Address of the oracle set as primary|

### OracleThresholdsUpdated
Emitted when oracle time thresholds are updated


```solidity
event OracleThresholdsUpdated(uint256 freshness, uint256 volatility);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`freshness`|`uint256`|New maximum age for all price data (in seconds)|
|`volatility`|`uint256`|New maximum age for volatile price data (in seconds)|

### Liquidated
Emitted when a position is liquidated


```solidity
event Liquidated(address indexed user, uint256 indexed positionId, address liquidator);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the inactive position|
|`liquidator`|`address`|The address of the liquidator|

### LiquidationMetrics
Emitted when a position is liquidated


```solidity
event LiquidationMetrics(
    address indexed user,
    uint256 indexed positionId,
    uint256 debtAmount,
    uint256 bonusAmount,
    uint256 collateralValue,
    uint256 healthFactor
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the inactive position|
|`debtAmount`|`uint256`|Debt amount|
|`bonusAmount`|`uint256`|Bonus Amount|
|`collateralValue`|`uint256`|Collateral value|
|`healthFactor`|`uint256`|Health Factor|

### InterPositionalTransfer
Emitted when collateral is transferred between positions


```solidity
event InterPositionalTransfer(
    address indexed user, uint256 indexed fromPositionId, uint256 indexed toPositionId, address asset, uint256 amount
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`fromPositionId`|`uint256`|Source position ID|
|`toPositionId`|`uint256`|Target position ID|
|`asset`|`address`|Address of the transferred asset|
|`amount`|`uint256`|Amount of the asset transferred|

## Errors
### InvalidPosition
Thrown when a position ID is invalid for a user


```solidity
error InvalidPosition(address user, uint256 positionId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The invalid position ID|

### InsufficientGovTokens
Thrown when a liquidator has insufficient governance tokens


```solidity
error InsufficientGovTokens(address liquidator, uint256 required, uint256 balance);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`liquidator`|`address`|The address attempting to liquidate|
|`required`|`uint256`|The required amount of governance tokens|
|`balance`|`uint256`|The liquidator's actual balance|

### NotLiquidatable
Thrown when attempting to liquidate a healthy position


```solidity
error NotLiquidatable(address user, uint256 positionId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position that can't be liquidated|

### InsufficientFlashLoanLiquidity
Thrown when there's insufficient liquidity for a flash loan


```solidity
error InsufficientFlashLoanLiquidity(address token, uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the requested token|
|`requested`|`uint256`|The amount requested|
|`available`|`uint256`|The actual available liquidity|

### FlashLoanFailed
Thrown when a flash loan execution fails


```solidity
error FlashLoanFailed();
```

### FlashLoanFundsNotReturned
Thrown when flash loan funds aren't fully returned with fees


```solidity
error FlashLoanFundsNotReturned(uint256 expected, uint256 actual);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`expected`|`uint256`|The expected amount to be returned|
|`actual`|`uint256`|The actual amount returned|

### OnlyUsdcSupported
Thrown when attempting flash loan with unsupported token


```solidity
error OnlyUsdcSupported(address token);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the unsupported token|

### FeeTooHigh
Thrown when attempting to set a fee higher than allowed


```solidity
error FeeTooHigh(uint256 requested, uint256 max);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested fee|
|`max`|`uint256`|The maximum allowed fee|

### FeeTooLow
Thrown when attempting to set a fee higher than allowed


```solidity
error FeeTooLow(uint256 requested, uint256 min);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested fee|
|`min`|`uint256`|The minimum allowed fee|

### InsufficientTokenBalance
Thrown when a user has insufficient token balance


```solidity
error InsufficientTokenBalance(address token, address user, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the token|
|`user`|`address`|The address of the user|
|`available`|`uint256`|The user's actual balance|

### AssetNotListed
Thrown when attempting to use an asset not listed in the protocol


```solidity
error AssetNotListed(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the unlisted asset|

### InsufficientLiquidity
Thrown when protocol has insufficient liquidity for borrowing


```solidity
error InsufficientLiquidity(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested amount|
|`available`|`uint256`|The actual available liquidity|

### IsolationDebtCapExceeded
Thrown when attempting to exceed isolation debt cap


```solidity
error IsolationDebtCapExceeded(address asset, uint256 requested, uint256 cap);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The isolated asset address|
|`requested`|`uint256`|The requested debt amount|
|`cap`|`uint256`|The maximum allowed debt in isolation mode|

### NoIsolatedCollateral
Thrown when no collateral is provided for isolated asset


```solidity
error NoIsolatedCollateral(address user, uint256 positionId, address isolatedAsset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|
|`isolatedAsset`|`address`|The address of the isolated asset|

### ExceedsCreditLimit
Thrown when attempting to borrow beyond credit limit


```solidity
error ExceedsCreditLimit(uint256 requested, uint256 creditLimit);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested borrow amount|
|`creditLimit`|`uint256`|The maximum allowed borrow amount|

### NoDebtToRepay
Thrown when attempting to repay a position with no debt


```solidity
error NoDebtToRepay(address user, uint256 positionId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position with no debt|

### InvalidPositionAsset
Thrown when asset doesn't match isolation mode settings


```solidity
error InvalidPositionAsset(address user, uint256 positionId, address requestedAsset, address isolatedAsset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|
|`requestedAsset`|`address`|The asset being added/withdrawn|
|`isolatedAsset`|`address`|The current isolated asset|

### InsufficientCollateralBalance
Thrown when user has insufficient collateral in position


```solidity
error InsufficientCollateralBalance(
    address user, uint256 positionId, address asset, uint256 requested, uint256 available
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|
|`asset`|`address`|The address of the collateral asset|
|`requested`|`uint256`|The requested withdrawal amount|
|`available`|`uint256`|The actual available collateral|

### WithdrawalExceedsCreditLimit
Thrown when withdrawal would make position undercollateralized


```solidity
error WithdrawalExceedsCreditLimit(address user, uint256 positionId, uint256 debtAmount, uint256 creditLimit);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|
|`debtAmount`|`uint256`|The position's current debt|
|`creditLimit`|`uint256`|The position's new credit limit after withdrawal|

### AssetDisabled
Thrown when attempting to use a disabled asset


```solidity
error AssetDisabled(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the disabled asset|

### IsolationModeRequired
Thrown when attempting to use asset that requires isolation mode


```solidity
error IsolationModeRequired(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset that requires isolation|

### SupplyCapExceeded
Thrown when attempting to exceed asset supply cap


```solidity
error SupplyCapExceeded(address asset, uint256 requested, uint256 cap);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|
|`requested`|`uint256`|The requested supply amount|
|`cap`|`uint256`|The maximum allowed supply|

### OracleInvalidPrice
Thrown when oracle returns invalid price data


```solidity
error OracleInvalidPrice(address oracle, int256 price);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the price oracle|
|`price`|`int256`|The invalid price value|

### OracleStalePrice
Thrown when oracle round is incomplete


```solidity
error OracleStalePrice(address oracle, uint80 roundId, uint80 answeredInRound);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the price oracle|
|`roundId`|`uint80`|The current round ID|
|`answeredInRound`|`uint80`|The round when answer was computed|

### OracleTimeout
Thrown when oracle data is too old


```solidity
error OracleTimeout(address oracle, uint256 timestamp, uint256 currentTimestamp, uint256 maxAge);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the price oracle|
|`timestamp`|`uint256`|The timestamp of the oracle data|
|`currentTimestamp`|`uint256`|The current block timestamp|
|`maxAge`|`uint256`|The maximum allowed age for oracle data|

### OracleInvalidPriceVolatility
Thrown when price has excessive volatility with stale data


```solidity
error OracleInvalidPriceVolatility(address oracle, int256 price, uint256 volatility);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the price oracle|
|`price`|`int256`|The current price|
|`volatility`|`uint256`|The calculated price change percentage|

### TooManyAssets
Thrown when trying to add more than 20 different assets to a position


```solidity
error TooManyAssets(address user, uint256 positionId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The position owner|
|`positionId`|`uint256`|The position ID|

### RewardTooHigh
Thrown when trying to set a reward above maximum allowed


```solidity
error RewardTooHigh(uint256 requested, uint256 max);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested rate|
|`max`|`uint256`|The maximum allowed rate|

### RewardIntervalTooShort
Thrown when trying to set a reward interval below minimum allowed


```solidity
error RewardIntervalTooShort(uint256 requested, uint256 minimum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested interval in seconds|
|`minimum`|`uint256`|The minimum allowed interval in seconds|

### RewardableSupplyTooLow
Thrown when trying to set rewardable supply below minimum allowed


```solidity
error RewardableSupplyTooLow(uint256 requested, uint256 minimum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested supply amount|
|`minimum`|`uint256`|The minimum allowed supply amount|

### LiquidatorThresholdTooLow
Thrown when trying to set liquidator threshold below minimum allowed


```solidity
error LiquidatorThresholdTooLow(uint256 requested, uint256 minimum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested threshold amount|
|`minimum`|`uint256`|The minimum allowed threshold amount|

### RateTooHigh
Thrown when trying to set a rate above maximum allowed


```solidity
error RateTooHigh(uint256 requested, uint256 maximum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested rate|
|`maximum`|`uint256`|The maximum allowed rate|

### RateTooLow
Thrown when trying to set a rate below minimum allowed


```solidity
error RateTooLow(uint256 requested, uint256 minimum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested rate|
|`minimum`|`uint256`|The minimum allowed rate|

### InactivePosition
Thrown when attempting to interact with an inactive position


```solidity
error InactivePosition(address user, uint256 positionId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the inactive position|

### OraclePriceDivergence
Thrown when multiple oracles report widely divergent prices


```solidity
error OraclePriceDivergence(address asset, uint256 minPrice, uint256 maxPrice, uint256 threshold);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|
|`minPrice`|`uint256`|The lowest reported price|
|`maxPrice`|`uint256`|The highest reported price|
|`threshold`|`uint256`|The maximum allowed divergence|

### CircuitBreakerTriggered
Thrown when a circuit breaker has been triggered due to extreme price movements


```solidity
error CircuitBreakerTriggered(address asset, uint256 currentPrice, uint256 previousPrice, uint256 changePercent);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset|
|`currentPrice`|`uint256`|The current price that triggered the circuit breaker|
|`previousPrice`|`uint256`|The previous valid price|
|`changePercent`|`uint256`|The percentage change that triggered the breaker|

### IsolationModeForbidden
Error thrown when an operation is not allowed in isolation mode

*Used for operations restricted to cross-collateral positions*


```solidity
error IsolationModeForbidden();
```

## Structs
### Asset
Configuration parameters for a collateral asset

*Contains all settings that define how an asset behaves within the protocol*


```solidity
struct Asset {
    uint8 active;
    uint8 oracleDecimals;
    uint8 decimals;
    uint32 borrowThreshold;
    uint32 liquidationThreshold;
    address oracleUSD;
    uint256 maxSupplyThreshold;
    uint256 isolationDebtCap;
    CollateralTier tier;
}
```

### UserPosition
User borrowing position data

*Core data structure tracking user's debt and position configuration*


```solidity
struct UserPosition {
    bool isIsolated;
    uint256 debtAmount;
    uint256 lastInterestAccrual;
    PositionStatus status;
}
```

### ProtocolSnapshot
Global protocol state variables

*Used for reporting and governance decisions*


```solidity
struct ProtocolSnapshot {
    uint256 utilization;
    uint256 borrowRate;
    uint256 supplyRate;
    uint256 totalBorrow;
    uint256 totalSuppliedLiquidity;
    uint256 targetReward;
    uint256 rewardInterval;
    uint256 rewardableSupply;
    uint256 baseProfitTarget;
    uint256 liquidatorThreshold;
    uint256 flashLoanFee;
}
```

## Enums
### CollateralTier
Classification of collateral assets by risk profile

*Used to determine borrowing parameters and liquidation thresholds*


```solidity
enum CollateralTier {
    STABLE,
    CROSS_A,
    CROSS_B,
    ISOLATED
}
```

### PositionStatus
Current status of a borrowing position

*Used to track position lifecycle and determine valid operations*


```solidity
enum PositionStatus {
    LIQUIDATED,
    ACTIVE,
    CLOSED
}
```

