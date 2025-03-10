# IPROTOCOL
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/interfaces/IProtocol.sol)


## Functions
### initialize

Initializes the protocol with core dependencies and parameters

*Sets up access control roles and default protocol parameters*


```solidity
function initialize(
    address usdc,
    address govToken,
    address ecosystem,
    address treasury_,
    address timelock_,
    address yieldToken,
    address assetsModule,
    address guardian
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
|`yieldToken`|`address`|The address of the yield token contract|
|`assetsModule`|`address`||
|`guardian`|`address`|The address of the initial admin with pausing capability|


### assetTVL

Returns the total value locked for a specific asset


```solidity
function assetTVL(address asset) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total amount of the asset held in the protocol|


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
function flashLoan(address receiver, uint256 amount, bytes calldata params) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`receiver`|`address`|The contract address that will receive the flash loaned tokens|
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


### getCollateralAmount

Gets the amount of a specific asset in a position


```solidity
function getCollateralAmount(address user, uint256 positionId, address asset) external view returns (uint256);
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


### calculateCollateralValue

Calculates the total USD value of all collateral in a position

*Aggregates values across all collateral assets using oracle price feeds*


```solidity
function calculateCollateralValue(address user, uint256 positionId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total value of all collateral assets in the position in USD terms|


### getLiquidityAccrueTimeIndex

Gets the timestamp of the last liquidity reward accrual for a user


```solidity
function getLiquidityAccrueTimeIndex(address user) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The timestamp when rewards were last accrued|


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
function getBorrowRate(ILendefiAssets.CollateralTier tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`ILendefiAssets.CollateralTier`|The collateral tier to query|

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


### getPositionTier

Determines the collateral tier of a position for risk assessment

*For cross-collateral positions, returns the tier of the riskiest asset*

*For isolated positions, returns ISOLATED tier regardless of asset*


```solidity
function getPositionTier(address user, uint256 positionId) external view returns (ILendefiAssets.CollateralTier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ILendefiAssets.CollateralTier`|The position's collateral tier (STABLE, CROSS_A, CROSS_B, or ISOLATED)|


### interpositionalTransfer

Transfers collateral between two positions owned by the same user


```solidity
function interpositionalTransfer(uint256 fromPositionId, uint256 toPositionId, address asset, uint256 amount)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fromPositionId`|`uint256`|The ID of the position to transfer collateral from|
|`toPositionId`|`uint256`|The ID of the position to transfer collateral to|
|`asset`|`address`|The address of the collateral asset to transfer|
|`amount`|`uint256`|The amount of the asset to transfer|


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

### ProtocolMetricsUpdated

```solidity
event ProtocolMetricsUpdated(
    uint256 profitTargetRate,
    uint256 borrowRate,
    uint256 rewardAmount,
    uint256 interval,
    uint256 supplyAmount,
    uint256 liquidatorAmount
);
```

### UpdateFlashLoanFee
Emitted when the flash loan fee is updated


```solidity
event UpdateFlashLoanFee(uint256 fee);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|New flash loan fee (scaled by 1000)|

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

### InterPositionalTransfer
Emitted when collateral is transferred between positions


```solidity
event InterPositionalTransfer(address indexed user, address asset, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`asset`|`address`|Address of the transferred asset|
|`amount`|`uint256`|Amount of the asset transferred|

### TVLUpdated
Emitted when an asset's total value locked (TVL) is updated


```solidity
event TVLUpdated(address indexed asset, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset that was updated|
|`amount`|`uint256`|The new TVL amount|

## Errors
### MaxPositionLimitReached
Thrown when attempting to create more positions than the protocol limit

*The protocol enforces a maximum number of positions per user*


```solidity
error MaxPositionLimitReached();
```

### InvalidPosition
Thrown when attempting to interact with a non-existent position

*Functions that require a valid position ID will throw this error*


```solidity
error InvalidPosition();
```

### InactivePosition
Thrown when attempting to modify a position that is closed or liquidated

*Operations are only allowed on positions with ACTIVE status*


```solidity
error InactivePosition();
```

### NotListed
Thrown when attempting to use an asset that isn't listed in the protocol

*Assets must be configured through governance before they can be used*


```solidity
error NotListed();
```

### ZeroAmount
Thrown when an operation is attempted with a zero amount

*Used in borrow, repay, supply and withdraw operations*


```solidity
error ZeroAmount();
```

### InvalidFee
Thrown when attempting to set an invalid flash loan fee

*Fee must be within the allowed range (0-100 basis points)*


```solidity
error InvalidFee();
```

### LowLiquidity
Thrown when the protocol does not have enough liquidity for an operation

*Used in borrow and flash loan operations*


```solidity
error LowLiquidity();
```

### FlashLoanFailed
Thrown when a flash loan operation fails to execute properly

*The receiver contract must return true from executeOperation*


```solidity
error FlashLoanFailed();
```

### RepaymentFailed
Thrown when a flash loan isn't repaid in the same transaction

*The full amount plus fee must be returned to the protocol*


```solidity
error RepaymentFailed();
```

### AssetCapacityReached
Thrown when attempting to supply an asset beyond its protocol-wide capacity

*Each asset has a maximum supply limit for risk management*


```solidity
error AssetCapacityReached();
```

### IsolatedAssetViolation
Thrown when attempting to violate isolation mode rules

*Isolated assets cannot be used in cross-collateralized positions*


```solidity
error IsolatedAssetViolation();
```

### InvalidAssetForIsolation
Thrown when attempting to add an incompatible asset to an isolated position

*Isolated positions can only contain one asset type*


```solidity
error InvalidAssetForIsolation();
```

### MaximumAssetsReached
Thrown when attempting to add more assets than the position limit

*Each position has a maximum number of different asset types it can hold*


```solidity
error MaximumAssetsReached();
```

### LowBalance
Thrown when attempting to withdraw more than the available balance

*Used in withdrawCollateral and interpositionalTransfer operations*


```solidity
error LowBalance();
```

### NotEnoughGovernanceTokens
Thrown when a liquidator doesn't hold enough governance tokens

*Liquidators must meet a minimum governance token threshold*


```solidity
error NotEnoughGovernanceTokens();
```

### NotLiquidatable
Thrown when attempting to liquidate a healthy position

*Positions must be below liquidation threshold to be liquidated*


```solidity
error NotLiquidatable();
```

### InvalidProfitTarget
Thrown when attempting to set an invalid profit target

*Profit target must be above the minimum threshold*


```solidity
error InvalidProfitTarget();
```

### InvalidBorrowRate
Thrown when attempting to set an invalid borrow rate

*Borrow rate must be above the minimum threshold*


```solidity
error InvalidBorrowRate();
```

### InvalidRewardAmount
Thrown when attempting to set an invalid reward amount

*Reward amount must be within allowed limits*


```solidity
error InvalidRewardAmount();
```

### InvalidInterval
Thrown when attempting to set an invalid reward interval

*Reward interval must be above the minimum threshold*


```solidity
error InvalidInterval();
```

### InvalidSupplyAmount
Thrown when attempting to set an invalid rewardable supply amount

*Rewardable supply must be above the minimum threshold*


```solidity
error InvalidSupplyAmount();
```

### InvalidLiquidatorThreshold
Thrown when attempting to set an invalid liquidator threshold

*Liquidator threshold must be above the minimum value*


```solidity
error InvalidLiquidatorThreshold();
```

### IsolationDebtCapExceeded
Thrown when attempting to borrow beyond an isolated asset's debt cap

*Isolated assets have maximum protocol-wide debt limits*


```solidity
error IsolationDebtCapExceeded();
```

### CreditLimitExceeded
Thrown when attempting to borrow or withdraw beyond a position's credit limit

*The operation would make the position undercollateralized*


```solidity
error CreditLimitExceeded();
```

### NoDebt
Thrown when attempting an operation that requires debt on a position with no debt

*Used in operations that require an active debt position*


```solidity
error NoDebt();
```

## Structs
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

## Enums
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

