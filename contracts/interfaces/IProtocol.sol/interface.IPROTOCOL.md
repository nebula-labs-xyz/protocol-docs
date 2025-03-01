# IPROTOCOL
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IProtocol.sol)

**Inherits:**
IERC20

Interface for the Lendefi Protocol

*Defines the core functionality and state variables of the Lendefi lending protocol*


## Functions
### createPosition

Creates a new borrowing position

*Initializes a new position that can hold collateral and debt*


```solidity
function createPosition(address asset, bool isIsolated) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The initial collateral asset type for the position|
|`isIsolated`|`bool`|Whether the position should use isolation mode|


### supplyCollateral

Adds collateral to a position

*The user must have approved the contract to transfer the asset*


```solidity
function supplyCollateral(address asset, uint256 amount, uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The collateral asset to supply|
|`amount`|`uint256`|The amount of the asset to supply|
|`positionId`|`uint256`|The ID of the position to add collateral to|


### withdrawCollateral

Withdraws collateral from a position

*The withdrawal must not cause the position to become undercollateralized*


```solidity
function withdrawCollateral(address asset, uint256 amount, uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The collateral asset to withdraw|
|`amount`|`uint256`|The amount of the asset to withdraw|
|`positionId`|`uint256`|The ID of the position to withdraw from|


### borrow

Borrow USDC against a position's collateral

*The borrow must not exceed the position's credit limit*


```solidity
function borrow(uint256 positionId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to borrow against|
|`amount`|`uint256`|The amount of USDC to borrow|


### liquidate

Liquidates an undercollateralized position

*Liquidator must have sufficient governance tokens*


```solidity
function liquidate(address user, uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position to liquidate|
|`positionId`|`uint256`|The ID of the position to liquidate|


### repay

Repays part or all of a position's debt

*Repays interest first, then principal*


```solidity
function repay(uint256 positionId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to repay|
|`amount`|`uint256`|The amount of USDC to repay|


### exitPosition

Closes a position and withdraws all collateral after repaying any debt

*All debt must be repaid before position can be closed*


```solidity
function exitPosition(uint256 positionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to exit|


### supplyLiquidity

Supplies USDC liquidity to the protocol and receives LP tokens

*Mints LYT tokens representing share of the lending pool*


```solidity
function supplyLiquidity(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of USDC to supply|


### exchange

Exchanges LP tokens for underlying USDC

*Burns LYT tokens and returns USDC plus accrued interest*


```solidity
function exchange(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of LP tokens to exchange|


### flashLoan

Executes a flash loan

*Receiver must implement IFlashLoanReceiver interface*


```solidity
function flashLoan(address receiver, address token, uint256 amount, bytes calldata params) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`receiver`|`address`|The contract that will receive the flash loan|
|`token`|`address`|The token to flash loan (currently only USDC)|
|`amount`|`uint256`|The amount to flash loan|
|`params`|`bytes`|Additional data to pass to the receiver|


### updateBaseProfitTarget

Updates the base profit target rate

*Minimum allowed rate is 0.25%*


```solidity
function updateBaseProfitTarget(uint256 rate) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|The new base profit target rate (scaled by 1e6)|


### updateBaseBorrowRate

Updates the base borrow rate

*Minimum allowed rate is 1%*


```solidity
function updateBaseBorrowRate(uint256 rate) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|The new base borrow rate (scaled by 1e6)|


### updateTargetReward

Updates the target reward amount

*Used for LP reward calculations*


```solidity
function updateTargetReward(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new target reward amount|


### updateRewardInterval

Updates the reward interval

*Minimum allowed interval is 90 days*


```solidity
function updateRewardInterval(uint256 interval) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interval`|`uint256`|The new reward interval in seconds|


### updateRewardableSupply

Updates the minimum supply required to be eligible for rewards

*Minimum allowed value is 20,000 WAD*


```solidity
function updateRewardableSupply(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new minimum supply amount|


### updateLiquidatorThreshold

Updates the minimum governance tokens required to perform liquidations

*Minimum allowed value is 10 tokens*


```solidity
function updateLiquidatorThreshold(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new liquidator threshold|


### updateFlashLoanFee

Updates the flash loan fee

*Maximum allowed fee is 1% (100 basis points)*


```solidity
function updateFlashLoanFee(uint256 newFee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newFee`|`uint256`|The new fee in basis points (1 = 0.01%)|


### updateAssetConfig

Updates the configuration for an asset


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
|`asset`|`address`|Address of the asset to configure|
|`oracle_`|`address`|Address of the price oracle|
|`oracleDecimals`|`uint8`|Number of decimals in the oracle price feed|
|`assetDecimals`|`uint8`|Number of decimals in the asset token|
|`active`|`uint8`|Whether the asset is active (1) or disabled (0)|
|`borrowThreshold`|`uint32`|LTV ratio for borrowing (e.g. 800 = 80%)|
|`liquidationThreshold`|`uint32`|LTV ratio for liquidation (e.g. 850 = 85%)|
|`maxSupplyLimit`|`uint256`|Maximum amount of this asset allowed in protocol|
|`tier`|`CollateralTier`|Risk category of the asset|
|`isolationDebtCap`|`uint256`|Maximum debt allowed when used in isolation mode|


### updateTierParameters

Updates the risk parameters for a collateral tier

*Maximum allowed rate is 25%, maximum bonus is 20%*


```solidity
function updateTierParameters(CollateralTier tier, uint256 borrowRate, uint256 liquidationBonus) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The tier to update|
|`borrowRate`|`uint256`|The new base borrow rate for the tier|
|`liquidationBonus`|`uint256`|The new liquidation bonus for the tier|


### updateAssetTier

Updates the risk tier of an asset

*Asset must already be listed in the protocol*


```solidity
function updateAssetTier(address asset, CollateralTier newTier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to update|
|`newTier`|`CollateralTier`|The new tier to assign to the asset|


### pause

Pauses protocol operations

*Can only be called by addresses with PAUSER_ROLE*


```solidity
function pause() external;
```

### unpause

Unpauses protocol operations

*Can only be called by addresses with PAUSER_ROLE*


```solidity
function unpause() external;
```

### getProtocolSnapshot

Returns a snapshot of the protocol's current state

*Aggregates various protocol metrics into a single struct*


```solidity
function getProtocolSnapshot() external view returns (ProtocolSnapshot memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ProtocolSnapshot`|A struct containing key protocol metrics|


### getTierRates

Returns tier-specific interest rates and liquidation bonuses

*Returns rates for all tiers in a single call*


```solidity
function getTierRates() external view returns (uint256[4] memory borrowRates, uint256[4] memory liquidationBonuses);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`borrowRates`|`uint256[4]`|Array of borrow rates for each tier|
|`liquidationBonuses`|`uint256[4]`|Array of liquidation bonuses for each tier|


### getLPInfo

Returns information about a liquidity provider

*Calculates current LP token value and reward eligibility*


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
|`user`|`address`|The address of the liquidity provider|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lpTokenBalance`|`uint256`|The user's LP token balance|
|`usdcValue`|`uint256`|The USDC value of the user's LP tokens|
|`lastAccrualTime`|`uint256`|The timestamp of the last reward accrual|
|`isRewardEligible`|`bool`|Whether the user is eligible for rewards|
|`pendingRewards`|`uint256`|The amount of pending rewards|


### getUtilization

Returns the current utilization ratio of the protocol

*Calculated as totalBorrow / totalSuppliedLiquidity*


```solidity
function getUtilization() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The utilization ratio scaled by 1e18|


### getSupplyRate

Returns the current supply interest rate

*Based on protocol profit and utilization*


```solidity
function getSupplyRate() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The supply rate scaled by 1e18|


### getBorrowRate

Returns the borrow interest rate for a specific collateral tier

*Includes tier-specific premium over base rate*


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
|`<none>`|`uint256`|The borrow rate for the tier scaled by 1e18|


### getPositionLiquidationFee

Returns the liquidation bonus percentage for a position

*Based on the position's collateral tier*


```solidity
function getPositionLiquidationFee(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The liquidation bonus percentage scaled by 1e18|


### getTierLiquidationFee

Returns the base liquidation fee percentage for a tier

*Direct accessor for tierLiquidationBonus mapping*


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
|`<none>`|`uint256`|The base liquidation fee percentage for the specified tier|


### getListedAssets

Returns all currently supported assets in the protocol

*Assets that have been listed in the protocol*


```solidity
function getListedAssets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|An array of asset addresses|


### getUserPosition

Returns detailed information about a user position

*Returns the raw position struct data*


```solidity
function getUserPosition(address user, uint256 positionId) external view returns (UserPosition memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition`|The position struct containing position details|


### getUserCollateralAmount

Returns the amount of a specific collateral in a position

*Raw amount without any price calculations*


```solidity
function getUserCollateralAmount(address user, uint256 positionId, address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|
|`asset`|`address`|The collateral asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of the asset in the position|


### getUserPositions

Returns all positions for a user

*Returns the entire positions array for the user*


```solidity
function getUserPositions(address user) external view returns (UserPosition[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address to query positions for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition[]`|An array of user positions|


### getUserPositionsCount

Returns the number of positions a user has

*Count of positions in the user's array*


```solidity
function getUserPositionsCount(address user) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The number of positions|


### getPositionAssets

Returns all assets in a position

*List of all assets used as collateral in the position*


```solidity
function getPositionAssets(address user, uint256 positionId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|An array of asset addresses in the position|


### getPositionDebt

Returns the current debt of a position excluding interest

*Returns only principal without accrued interest*


```solidity
function getPositionDebt(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The raw debt amount in USDC|


### calculateDebtWithInterest

Calculates the total debt with accrued interest for a position

*Includes interest accrued since last update*


```solidity
function calculateDebtWithInterest(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The debt amount with interest in USDC|


### calculateCreditLimit

Calculates the available credit limit for a position

*Based on collateral value and borrowing thresholds*


```solidity
function calculateCreditLimit(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum borrowable amount in USDC|


### healthFactor

Calculates the health factor of a position

*Higher values indicate a safer position*


```solidity
function healthFactor(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The health factor (scaled by 1e18, >1 is healthy)|


### isLiquidatable

Checks if a position is eligible for liquidation

*Position is liquidatable if health factor falls below 1*


```solidity
function isLiquidatable(address user, uint256 positionId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the position can be liquidated|


### getPositionSummary

Returns a summary of a position's state

*Provides a complete overview of the position's status*


```solidity
function getPositionSummary(address user, uint256 positionId)
    external
    view
    returns (
        uint256 totalCollateralValue,
        uint256 currentDebt,
        uint256 availableCredit,
        bool isIsolated,
        address isolatedAsset
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalCollateralValue`|`uint256`|The total value of collateral in the position|
|`currentDebt`|`uint256`|The current debt including interest|
|`availableCredit`|`uint256`|The available credit to borrow|
|`isIsolated`|`bool`|Whether the position is in isolation mode|
|`isolatedAsset`|`address`|The isolated asset (if applicable)|


### getHighestTier

Returns the highest risk tier among a position's collateral assets

*Used to determine applicable interest rates*


```solidity
function getHighestTier(address user, uint256 positionId) external view returns (CollateralTier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The owner of the position|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CollateralTier`|The highest risk tier in the position|


### getAssetInfo

Returns the configuration for an asset

*Complete configuration struct for the asset*


```solidity
function getAssetInfo(address asset) external view returns (Asset memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Asset`|The asset configuration|


### getAssetPrice

Returns the current USD price of an asset

*Fetches price from Chainlink oracle*


```solidity
function getAssetPrice(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The price in USD (scaled by oracle decimals)|


### getAssetDetails

Returns detailed information about an asset

*Aggregates various asset metrics into a single call*


```solidity
function getAssetDetails(address asset)
    external
    view
    returns (
        uint256 price,
        uint256 totalSupplied,
        uint256 maxSupply,
        uint256 borrowRate,
        uint256 liquidationBonus,
        CollateralTier tier
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|The current USD price (scaled by oracle decimals)|
|`totalSupplied`|`uint256`|The total amount supplied to the protocol|
|`maxSupply`|`uint256`|The maximum supply threshold|
|`borrowRate`|`uint256`|The current borrow rate for the asset|
|`liquidationBonus`|`uint256`|The liquidation bonus for the asset|
|`tier`|`CollateralTier`|The risk tier of the asset|


### getAssetPriceOracle

Returns the raw price from an asset's oracle

*Validates oracle data for freshness and volatility*


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
|`<none>`|`uint256`|The price from the oracle|


### isRewardable

Checks if a user is eligible for rewards

*Based on time since last accrual and minimum supply*


```solidity
function isRewardable(address user) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the user is eligible for rewards|


### totalBorrow

Total amount borrowed from the protocol


```solidity
function totalBorrow() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Current total borrow amount in USDC|


### totalSuppliedLiquidity

Total liquidity supplied to the protocol


```solidity
function totalSuppliedLiquidity() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Current total supply in USDC|


### totalAccruedBorrowerInterest

Total interest paid by borrowers


```solidity
function totalAccruedBorrowerInterest() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Accumulated borrower interest|


### totalAccruedSupplierInterest

Total interest earned by suppliers


```solidity
function totalAccruedSupplierInterest() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Accumulated supplier interest|


### withdrawnLiquidity

Total liquidity withdrawn from the protocol


```solidity
function withdrawnLiquidity() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Cumulative withdrawn liquidity|


### targetReward

Target reward amount for eligible LPs


```solidity
function targetReward() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Maximum reward amount per full period|


### rewardInterval

Time interval for reward distribution


```solidity
function rewardInterval() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Reward period in seconds|


### rewardableSupply

Minimum supply to be eligible for rewards


```solidity
function rewardableSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Minimum USDC equivalent required|


### baseBorrowRate

Base interest rate for borrowing


```solidity
function baseBorrowRate() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Base rate in parts per million|


### baseProfitTarget

Target profit margin for the protocol


```solidity
function baseProfitTarget() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Profit target in parts per million|


### liquidatorThreshold

Minimum governance tokens required for liquidation


```solidity
function liquidatorThreshold() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Minimum token requirement|


### version

Current protocol version


```solidity
function version() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|Protocol version number|


### treasury

Treasury address for protocol fees


```solidity
function treasury() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Treasury contract address|


### flashLoanFee

Fee charged for flash loans


```solidity
function flashLoanFee() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Fee in basis points|


### totalFlashLoanFees

Total fees collected from flash loans


```solidity
function totalFlashLoanFees() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Accumulated flash loan fees|


### tierBaseBorrowRate

Base interest rate for a specific collateral tier


```solidity
function tierBaseBorrowRate(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Base rate for the tier|


### tierLiquidationBonus

Liquidation bonus for a specific collateral tier


```solidity
function tierLiquidationBonus(CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Liquidation bonus percentage|


### totalCollateral

Total amount of an asset used as collateral


```solidity
function totalCollateral(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total collateral amount|


### assetTVL

Total value locked of an asset


```solidity
function assetTVL(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|TVL of the asset|


## Events
### FlashLoan

```solidity
event FlashLoan(address indexed initiator, address indexed receiver, address token, uint256 amount, uint256 fee);
```

### Reward

```solidity
event Reward(address indexed to, uint256 amount);
```

### Exchange

```solidity
event Exchange(address indexed src, uint256 amountIn, uint256 amountOut);
```

### Initialized

```solidity
event Initialized(address indexed src);
```

### SupplyCollateral

```solidity
event SupplyCollateral(address indexed user, uint256 indexed positionId, address indexed asset, uint256 amount);
```

### WithdrawCollateral

```solidity
event WithdrawCollateral(address indexed user, uint256 indexed positionId, address indexed asset, uint256 amount);
```

### Borrow

```solidity
event Borrow(address indexed user, uint256 indexed positionId, uint256 amount);
```

### Liquidated

```solidity
event Liquidated(address indexed user, uint256 indexed positionId, uint256 amount);
```

### EnteredIsolationMode

```solidity
event EnteredIsolationMode(address indexed user, uint256 indexed positionId, address indexed asset);
```

### ExitedIsolationMode

```solidity
event ExitedIsolationMode(address indexed user, uint256 indexed positionId);
```

### TierParametersUpdated

```solidity
event TierParametersUpdated(CollateralTier indexed tier, uint256 borrowRate, uint256 liquidationBonus);
```

### AssetTierUpdated

```solidity
event AssetTierUpdated(address indexed asset, CollateralTier tier);
```

### UpdateAssetConfig

```solidity
event UpdateAssetConfig(address indexed asset);
```

### Upgrade

```solidity
event Upgrade(address indexed src, address indexed implementation);
```

### Repay

```solidity
event Repay(address indexed user, uint256 indexed positionId, uint256 amount);
```

### PositionClosed

```solidity
event PositionClosed(address indexed user, uint256 indexed positionId);
```

### SupplyLiquidity

```solidity
event SupplyLiquidity(address indexed user, uint256 amount);
```

### PositionCreated

```solidity
event PositionCreated(address indexed user, uint256 indexed positionId, bool isIsolated);
```

### UpdateBaseProfitTarget

```solidity
event UpdateBaseProfitTarget(uint256 rate);
```

### UpdateBaseBorrowRate

```solidity
event UpdateBaseBorrowRate(uint256 rate);
```

### UpdateTargetReward

```solidity
event UpdateTargetReward(uint256 amount);
```

### UpdateRewardInterval

```solidity
event UpdateRewardInterval(uint256 interval);
```

### UpdateRewardableSupply

```solidity
event UpdateRewardableSupply(uint256 amount);
```

### UpdateLiquidatorThreshold

```solidity
event UpdateLiquidatorThreshold(uint256 amount);
```

### UtilizationUpdated

```solidity
event UtilizationUpdated(uint256 newUtilization);
```

### SupplyRateUpdated

```solidity
event SupplyRateUpdated(uint256 newRate);
```

### CollateralValueChanged

```solidity
event CollateralValueChanged(address user, uint256 positionId, uint256 newValue);
```

### InterestAccrued

```solidity
event InterestAccrued(address indexed user, uint256 indexed positionId, uint256 interestAccrued);
```

### TVLUpdated

```solidity
event TVLUpdated(address indexed asset, uint256 newTVL);
```

### UpdateFlashLoanFee

```solidity
event UpdateFlashLoanFee(uint256 newFee);
```

## Structs
### Asset
*Configuration for an asset in the protocol*


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

### UserPosition
*Represents a user's borrowing position*


```solidity
struct UserPosition {
    bool isIsolated;
    address isolatedAsset;
    uint256 debtAmount;
    uint256 lastInterestAccrual;
}
```

### ProtocolSnapshot
*Overview of protocol state*


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
*Risk categorizations for different asset types*


```solidity
enum CollateralTier {
    STABLE,
    CROSS_A,
    CROSS_B,
    ISOLATED
}
```

