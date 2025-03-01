# Lendefi
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/lender/Lendefi.sol)

**Inherits:**
[IPROTOCOL](/contracts/interfaces/IProtocol.sol/interface.IPROTOCOL.md), ERC20Upgradeable, ERC20PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable, [YodaMath](/contracts/lender/lib/YodaMath.sol/contract.YodaMath.md)

**Author:**
alexei@nebula-labs(dot)xyz

,,       ,,  ,,    ,,,    ,,   ,,,      ,,,    ,,,   ,,,          ,,,
███▄     ██  ███▀▀▀███▄   ██▄██▀▀██▄    ██▌     ██▌  ██▌        ▄▄███▄▄
█████,   ██  ██▌          ██▌     └██▌  ██▌     ██▌  ██▌        ╟█   ╙██
██ └███ ██  ██▌└██╟██   l███▀▄███╟█    ██      ╟██  ╟█i        ▐█▌█▀▄██╟
██   ╙████  ██▌          ██▌     ,██▀   ╙██    ▄█▀  ██▌        ▐█▌    ██
██     ╙██  █████▀▀▄██▀  ██▌██▌╙███▀`     ▀██▄██▌   █████▀▄██▀ ▐█▌    ██╟
¬─      ¬─   ¬─¬─  ¬─¬─'  ¬─¬─¬─¬ ¬─'       ¬─¬─    '¬─   '─¬   ¬─     ¬─'
,,,          ,,     ,,,    ,,,      ,,   ,,,  ,,,      ,,,    ,,,   ,,,    ,,,   ,,,
██▌          ███▀▀▀███▄   ███▄     ██   ██▄██▀▀██▄     ███▀▀▀███▄   ██▄██▀▀██▄  ▄██╟
██▌          ██▌          █████,   ██   ██▌     └██▌   ██▌          ██▌          ██
╟█l          ███▀▄███     ██ └███  ██   l██       ██╟  ███▀▄███     ██▌└██╟██    ╟█i
██▌         ██▌          ██    ╙████    ██▌     ,██▀  ██▌          ██▌           ██
█████▀▄██▀  █████▀▀▄██▀  ██      ╙██    ██▌██▌╙███▀`  █████▀▀▄██▀  ╙██           ╙██
¬─     ¬─   ¬─¬─  ¬─¬─'  ¬─¬─     ¬─'   ¬─¬─   '¬─    '─¬   ¬─      ¬─'           ¬─'

An efficient monolithic lending protocol

*Implements a secure and upgradeable collateralized lending protocol with Yield Token*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.
Core Features:
- Lending and borrowing with multiple collateral tiers
- Isolated and cross-collateral positions
- Dynamic interest rates based on utilization
- Flash loans with configurable fees
- Liquidation mechanism with tier-based bonuses
- Liquidity provider rewards system
- Price oracle integration with safety checks
Security Features:
- Role-based access control
- Pausable functionality
- Non-reentrant operations
- Upgradeable contract pattern
- Oracle price validation
- Supply and debt caps

- roles: 
- DEFAULT_ADMIN_ROLE: Contract administration
- PAUSER_ROLE: Emergency pause/unpause
- MANAGER_ROLE: Protocol parameter updates
- UPGRADER_ROLE: Contract upgrades

- tiers: Collateral tiers in ascending order of risk:
- STABLE: Lowest risk, stablecoins (5% liquidation bonus)
- CROSS_A: Low risk assets (8% liquidation bonus)
- CROSS_B: Medium risk assets (10% liquidation bonus)
- ISOLATED: High risk assets (15% liquidation bonus)

- inheritance: 
- IPROTOCOL: Protocol interface
- ERC20Upgradeable: Base token functionality
- ERC20PausableUpgradeable: Pausable token operations
- AccessControlUpgradeable: Role-based access
- ReentrancyGuardUpgradeable: Reentrancy protection
- UUPSUpgradeable: Upgrade pattern
- YodaMath: Interest calculations

- oz-upgrades: 


## State Variables
### PAUSER_ROLE

```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE

```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE

```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### usdcInstance
*USDC token instance*


```solidity
IERC20 internal usdcInstance;
```


### tokenInstance
*governance token instance*


```solidity
IERC20 internal tokenInstance;
```


### ecosystemInstance
*ecosystem contract instance*


```solidity
IECOSYSTEM internal ecosystemInstance;
```


### listedAsset

```solidity
EnumerableSet.AddressSet internal listedAsset;
```


### totalBorrow
Total amount of USDC borrowed from the protocol

*Tracks the sum of all active borrowing positions' principal*


```solidity
uint256 public totalBorrow;
```


### totalSuppliedLiquidity
Total liquidity supplied to the protocol by liquidity providers

*Denominated in USDC with 6 decimals, used for utilization calculations*


```solidity
uint256 public totalSuppliedLiquidity;
```


### totalAccruedBorrowerInterest
Total interest paid by borrowers over the protocol's lifetime

*Accumulated separately from principal for accounting purposes*


```solidity
uint256 public totalAccruedBorrowerInterest;
```


### totalAccruedSupplierInterest
Total interest earned by liquidity providers

*Includes all interest distributed to LPs since protocol inception*


```solidity
uint256 public totalAccruedSupplierInterest;
```


### withdrawnLiquidity
Total liquidity withdrawn from the protocol

*Cumulative tracking of all withdrawals, used for analytics*


```solidity
uint256 public withdrawnLiquidity;
```


### targetReward
Target reward amount for eligible liquidity providers

*Maximum reward amount achievable over a full reward interval*


```solidity
uint256 public targetReward;
```


### rewardInterval
Time interval for calculating rewards (in seconds)

*Typically set to 180 days, used as denominator in reward calculations*


```solidity
uint256 public rewardInterval;
```


### rewardableSupply
Minimum supply required to be eligible for rewards

*Users must supply at least this amount to qualify for ecosystem rewards*


```solidity
uint256 public rewardableSupply;
```


### baseBorrowRate
Base interest rate applied to all loans

*Expressed in parts per million (e.g., 0.06e6 = 6%)*


```solidity
uint256 public baseBorrowRate;
```


### baseProfitTarget
Target profit margin for the protocol

*Used to calculate fees and maintain protocol sustainability*


```solidity
uint256 public baseProfitTarget;
```


### liquidatorThreshold
Minimum governance tokens required to perform liquidations

*Ensures liquidators have skin in the game by requiring token holdings*


```solidity
uint256 public liquidatorThreshold;
```


### flashLoanFee
Fee charged for flash loans in basis points

*9 basis points = 0.09%, maximum allowed is 100 (1%)*


```solidity
uint256 public flashLoanFee;
```


### totalFlashLoanFees
Total fees collected from flash loans

*Accumulated since inception, part of protocol revenue*


```solidity
uint256 public totalFlashLoanFees;
```


### version
Current contract version, incremented on upgrades

*Used to track implementation versions for transparent upgrades*


```solidity
uint8 public version;
```


### treasury
Address of the treasury that receives protocol fees

*Fees are sent here for protocol sustainability and governance*


```solidity
address public treasury;
```


### assetInfo
Asset configuration details for supported collateral types

*Maps asset address to its full configuration struct*


```solidity
mapping(address => Asset) internal assetInfo;
```


### positions
User borrowing positions by address

*Maps user address to an array of all their positions*


```solidity
mapping(address => UserPosition[]) internal positions;
```


### positionCollateralAmounts
Collateral amounts by position and asset

*Maps user address, position ID, and asset address to collateral amount*


```solidity
mapping(address => mapping(uint256 => mapping(address => uint256))) internal positionCollateralAmounts;
```


### positionAssets
List of assets used as collateral in each position

*Maps user address and position ID to array of collateral asset addresses*


```solidity
mapping(address => mapping(uint256 => address[])) internal positionAssets;
```


### assetTVL
Total value locked of each supported asset

*Tracks how much of each asset is held by the protocol*


```solidity
mapping(address => uint256) public assetTVL;
```


### tierBaseBorrowRate
Base borrow rate for each collateral risk tier

*Higher tiers have higher interest rates due to increased risk*


```solidity
mapping(CollateralTier => uint256) public tierBaseBorrowRate;
```


### tierLiquidationBonus
Liquidation bonus percentage for each collateral tier

*Higher risk tiers have larger liquidation bonuses*


```solidity
mapping(CollateralTier => uint256) public tierLiquidationBonus;
```


### totalCollateral
Total amount of each asset used as collateral

*Used for tracking collateral usage and supply caps*


```solidity
mapping(address => uint256) public totalCollateral;
```


### liquidityAccrueTimeIndex
Timestamp of last reward accrual for each liquidity provider

*Used to calculate eligible rewards based on time elapsed*


```solidity
mapping(address src => uint256 time) internal liquidityAccrueTimeIndex;
```


### __gap

```solidity
uint256[50] private __gap;
```


## Functions
### validPosition

Validates that the provided position ID exists for the specified user

*Reverts with InvalidPosition error if the position ID exceeds the user's positions array length*

**Note:**
security: Position IDs are zero-indexed and must be less than the total number of positions


```solidity
modifier validPosition(address user, uint256 positionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user who owns the position|
|`positionId`|`uint256`|The ID of the position to validate|


### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

Initializes the contract with core dependencies and parameters

*Sets up ERC20 token details, access control roles, and default protocol parameters
including interest rates and liquidation bonuses for each collateral tier*

**Note:**
oz-upgrades-unsafe: initializer is used instead of constructor for proxy pattern


```solidity
function initialize(
    address usdc,
    address govToken,
    address ecosystem,
    address treasury_,
    address timelock_,
    address guardian
) external initializer;
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


### pause

Pauses all protocol operations in case of emergency

*Can only be called by accounts with PAUSER_ROLE*

**Notes:**
- security: This is a critical emergency function that should be carefully controlled

- access: Restricted to PAUSER_ROLE

- events: Emits a Paused event from ERC20Pausable


```solidity
function pause() external override onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses protocol operations, returning to normal functioning

*Can only be called by accounts with PAUSER_ROLE*

**Notes:**
- security: Should only be called after thorough security verification

- access: Restricted to PAUSER_ROLE

- events: Emits an Unpaused event from ERC20Pausable


```solidity
function unpause() external override onlyRole(PAUSER_ROLE);
```

### flashLoan

Executes a flash loan of USDC tokens

*This function enables atomic flash loans with the following flow:
1. Checks token is USDC and sufficient liquidity exists
2. Transfers tokens to receiver
3. Calls receiver's executeOperation function
4. Verifies funds are returned with fee*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- fee: Flash loan fee is configurable, default 9 basis points (0.09%)

- events: Emits FlashLoan event with loan details and fee


```solidity
function flashLoan(address receiver, address token, uint256 amount, bytes calldata params)
    external
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`receiver`|`address`|Address of the contract receiving the flash loan|
|`token`|`address`|Token to be flash loaned (must be USDC)|
|`amount`|`uint256`|Amount of tokens to flash loan|
|`params`|`bytes`|Arbitrary data to be passed to the receiver's executeOperation function|


### updateFlashLoanFee

Updates the flash loan fee percentage

*Can only be called by accounts with MANAGER_ROLE*

**Notes:**
- security: Maximum fee is capped at 100 basis points (1%) to protect users

- access: Restricted to MANAGER_ROLE

- events: Emits UpdateFlashLoanFee event with new fee value


```solidity
function updateFlashLoanFee(uint256 newFee) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newFee`|`uint256`|The new fee in basis points (1 = 0.01%)|


### supplyLiquidity

Allows users to supply USDC liquidity to the protocol in exchange for LYT tokens

*Mints LYT tokens representing share of the lending pool based on current exchange rate*

**Notes:**
- security: Non-reentrant to prevent reentrancy attacks

- validation: Checks:
- User has sufficient USDC balance

- calculations: 
- If pool is empty: 1:1 mint ratio
- Otherwise: amount * totalSupply / totalAssets
- Special case for 0 utilization: 1:1 mint ratio

- state: Updates:
- Increases totalSuppliedLiquidity
- Updates liquidityAccrueTimeIndex for rewards
- Mints LYT tokens to supplier

- events: Emits:
- SupplyLiquidity with supplier address and amount


```solidity
function supplyLiquidity(uint256 amount) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of USDC to supply (in USDC decimals - 6)|


### exchange

Allows users to exchange LYT tokens for underlying USDC with accrued interest

*Calculates redemption value based on current pool state and profit target*

**Notes:**
- security: Non-reentrant to prevent reentrancy attacks

- validation: Checks:
- User has sufficient LYT token balance

- calculations: 
- Base amount = (amount * totalSuppliedLiquidity) / totalSupply
- Protocol fee = baseAmount * baseProfitTarget if profitable
- Redemption value = (amount * (USDC balance + totalBorrow)) / totalSupply

- state: Updates:
- Decreases totalSuppliedLiquidity
- Updates withdrawnLiquidity
- Updates totalAccruedSupplierInterest
- Burns user's LYT tokens
- Mints fee to treasury if profitable

- events: Emits:
- Exchange with sender address, LYT amount, and USDC value


```solidity
function exchange(uint256 amount) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of LYT tokens to exchange|


### supplyCollateral

Allows users to supply collateral assets to a borrowing position

*Handles both isolated and cross-collateral positions with appropriate validations*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Asset is listed and active
- Position exists
- Isolation mode constraints
- Supply cap limits
- Maximum assets per position (20)

- events: Emits:
- TVLUpdated with new total value locked
- SupplyCollateral with supply details


```solidity
function supplyCollateral(address asset, uint256 amount, uint256 positionId)
    external
    validPosition(msg.sender, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the collateral asset to supply|
|`amount`|`uint256`|The amount of the asset to supply|
|`positionId`|`uint256`|The ID of the position to supply collateral to|


### withdrawCollateral

Allows users to withdraw collateral assets from their borrowing position

*Process:
1. Validates isolation mode constraints
2. Checks sufficient collateral balance
3. Updates collateral state
4. Verifies remaining collateral supports existing debt
5. Removes asset from position if fully withdrawn*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Position exists (via validPosition modifier)
- Isolation mode constraints
- Sufficient collateral balance
- Withdrawal doesn't exceed credit limit

- events: Emits:
- TVLUpdated with new total value locked
- WithdrawCollateral with withdrawal details

- access: Public function, but requires valid position ownership


```solidity
function withdrawCollateral(address asset, uint256 amount, uint256 positionId)
    external
    validPosition(msg.sender, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the collateral asset to withdraw|
|`amount`|`uint256`|The amount of the asset to withdraw|
|`positionId`|`uint256`|The ID of the position to withdraw from|


### createPosition

Creates a new borrowing position for the caller

*Creates a new position and optionally enables isolation mode with the specified asset*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Asset must be listed in the protocol
- If isolation mode is enabled, asset must be eligible

- events: Emits:
- PositionCreated with user address, position ID, and isolation mode status

- access: Public function, any user can create positions


```solidity
function createPosition(address asset, bool isIsolated) external nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The initial collateral asset address for the position|
|`isIsolated`|`bool`|Whether the position should use isolation mode|


### borrow

Allows users to borrow USDC against their collateral position

*Process:
1. Checks protocol liquidity
2. Validates isolation mode constraints
3. Verifies credit limit
4. Updates position state and protocol metrics*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Position exists (via validPosition modifier)
- Sufficient protocol liquidity
- Isolation debt cap (if applicable)
- Isolation collateral existence
- Credit limit not exceeded

- events: Emits:
- Borrow event with borrower address, position ID, and amount

- state: Updates:
- Position debt amount
- Last interest accrual timestamp
- Total protocol borrow amount


```solidity
function borrow(uint256 positionId, uint256 amount)
    external
    validPosition(msg.sender, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the borrower's position|
|`amount`|`uint256`|The amount of USDC to borrow|


### repay

Allows users to repay debt on their borrowing position

*Handles both partial and full repayments, with interest paid first*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Position exists (via validPosition modifier)
- Position has debt to repay

- calculations: 
- Calculates total debt with accrued interest
- Caps repayment at total debt amount
- Separates interest from principal repayment

- events: Emits:
- Repay with amount repaid
- InterestAccrued with interest amount

- state: Updates:
- Position debt amount
- Total protocol borrow amount
- Total accrued borrower interest
- Last interest accrual timestamp


```solidity
function repay(uint256 positionId, uint256 amount)
    external
    validPosition(msg.sender, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to repay debt for|
|`amount`|`uint256`|The amount of USDC to repay|


### exitPosition

Closes a borrowing position by repaying all debt and withdrawing collateral

*Process:
1. Repays any outstanding debt with interest
2. Withdraws all collateral assets
3. Clears isolation mode settings if active
4. Removes the position from user's positions array*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Position exists (via validPosition modifier)
- User has sufficient USDC balance to repay debt

- events: Emits:
- Repay when debt is repaid
- WithdrawCollateral for each asset withdrawn
- ExitedIsolationMode if position was in isolation mode
- PositionClosed when position is fully closed

- state: Updates:
- Clears position debt and collateral
- Updates total protocol borrow amount
- Updates total collateral amounts
- Removes position from storage


```solidity
function exitPosition(uint256 positionId) external validPosition(msg.sender, positionId) nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position to close|


### liquidate

Liquidates an undercollateralized borrowing position

*Process:
1. Verifies liquidator has sufficient governance tokens
2. Confirms position is actually liquidatable (health factor < 1)
3. Calculates total debt including accrued interest
4. Determines liquidation bonus based on collateral tier
5. Transfers required USDC from liquidator
6. Transfers all position collateral to liquidator*

**Notes:**
- security: Non-reentrant and pausable to prevent attack vectors

- validation: Checks:
- Position exists (via validPosition modifier)
- Liquidator has sufficient governance tokens
- Position is actually liquidatable
- Liquidator has sufficient USDC to cover debt + bonus

- rewards: Liquidator receives:
- All collateral in the position
- Liquidation bonus (varies by tier)

- events: Emits:
- Liquidated event with user, position ID, and debt amount
- WithdrawCollateral events for each collateral asset transferred

- state: Updates:
- Clears position debt
- Updates total protocol borrow amount
- Updates total accrued interest
- Transfers all collateral to liquidator


```solidity
function liquidate(address user, uint256 positionId)
    external
    validPosition(user, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner to liquidate|
|`positionId`|`uint256`|The ID of the position to liquidate|


### updateBaseProfitTarget

Updates the base profit target rate for the protocol

*Updates the profit target used for calculating protocol fees and rewards*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Rate must be at least 0.25% (0.0025e6)

- events: Emits:
- UpdateBaseProfitTarget with new rate value


```solidity
function updateBaseProfitTarget(uint256 rate) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|The new base profit target rate in parts per million (e.g., 0.0025e6 = 0.25%)|


### updateBaseBorrowRate

Updates the base borrow interest rate for the protocol

*Updates the minimum borrow rate applied to all loans*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Rate must be at least 1% (0.01e6)

- events: Emits:
- UpdateBaseBorrowRate with new rate value


```solidity
function updateBaseBorrowRate(uint256 rate) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|The new base borrow rate in parts per million (e.g., 0.01e6 = 1%)|


### updateTargetReward

Updates the target reward amount for liquidity providers

*Updates the maximum reward amount achievable over a full reward interval*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Amount must not exceed 10,000 tokens

- events: Emits:
- UpdateTargetReward with new amount


```solidity
function updateTargetReward(uint256 amount) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new target reward amount in LP tokens|


### updateRewardInterval

Updates the time interval for calculating liquidity provider rewards

*Updates the duration over which max rewards can be earned*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Interval must be at least 90 days

- events: Emits:
- UpdateRewardInterval with new interval value


```solidity
function updateRewardInterval(uint256 interval) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interval`|`uint256`|The new reward interval in seconds|


### updateRewardableSupply

Updates the minimum supply required for reward eligibility

*Updates the threshold for liquidity providers to be eligible for rewards*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Amount must be at least 20,000 WAD

- events: Emits:
- UpdateRewardableSupply with new amount


```solidity
function updateRewardableSupply(uint256 amount) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new minimum supply amount (scaled by WAD)|


### updateLiquidatorThreshold

Updates the minimum governance tokens required to perform liquidations

*Updates the minimum token requirement for liquidator eligibility*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Amount must be at least 10 tokens

- events: Emits:
- UpdateLiquidatorThreshold with new amount


```solidity
function updateLiquidatorThreshold(uint256 amount) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The new threshold amount in governance tokens (18 decimals)|


### updateTierParameters

Updates the risk parameters for a collateral tier

*Updates both interest rates and liquidation incentives for a specific risk tier*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Borrow rate must not exceed 25% (0.25e6)
- Liquidation bonus must not exceed 20% (0.2e6)

- events: Emits:
- TierParametersUpdated with tier, new borrow rate, and new liquidation bonus


```solidity
function updateTierParameters(CollateralTier tier, uint256 borrowRate, uint256 liquidationBonus)
    external
    onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to update|
|`borrowRate`|`uint256`|The new base borrow rate for the tier (in parts per million)|
|`liquidationBonus`|`uint256`|The new liquidation bonus for the tier (in parts per million)|


### updateAssetTier

Updates the risk tier classification for a listed asset

*Changes the risk classification which affects interest rates and liquidation parameters*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Checks:
- Asset must be listed in the protocol

- events: Emits:
- AssetTierUpdated with asset address and new tier

- impact: Changes:
- Asset's borrow rate via tierBaseBorrowRate
- Asset's liquidation bonus via tierLiquidationBonus


```solidity
function updateAssetTier(address asset, CollateralTier newTier) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to update|
|`newTier`|`CollateralTier`|The new CollateralTier to assign to the asset|


### updateAssetConfig

Updates or adds a new asset configuration in the protocol

*Manages all configuration parameters for an asset in a single function*

**Notes:**
- security: Can only be called by accounts with MANAGER_ROLE

- validation: Adds asset to listedAsset set if not already present

- state: Updates:
- Asset configuration in assetInfo mapping
- Listed assets enumerable set

- events: Emits:
- UpdateAssetConfig with asset address


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
) external onlyRole(MANAGER_ROLE);
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


### getTierRates

Retrieves the current borrow rates and liquidation bonuses for all collateral tiers

*Returns two fixed arrays containing rates for ISOLATED, CROSS_A, CROSS_B, and STABLE tiers in that order*

**Note:**
returns-description: Index mapping:
- [0] = ISOLATED tier rates
- [1] = CROSS_A tier rates
- [2] = CROSS_B tier rates
- [3] = STABLE tier rates


```solidity
function getTierRates() external view returns (uint256[4] memory borrowRates, uint256[4] memory liquidationBonuses);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`borrowRates`|`uint256[4]`|Array of borrow rates for each tier [ISOLATED, CROSS_A, CROSS_B, STABLE]|
|`liquidationBonuses`|`uint256[4]`|Array of liquidation bonuses for each tier [ISOLATED, CROSS_A, CROSS_B, STABLE]|


### getListedAssets

Returns an array of all listed asset addresses in the protocol

*Retrieves assets from the EnumerableSet storing listed assets*

**Notes:**
- complexity: O(n) where n is the number of listed assets

- state-access: Read-only function, no state modifications


```solidity
function getListedAssets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of addresses representing all listed assets|


### getUserPosition

Retrieves a user's borrowing position details

*Returns the full UserPosition struct containing position details*

**Notes:**
- security: Validates position exists via validPosition modifier

- access: Public view function, no state modifications


```solidity
function getUserPosition(address user, uint256 positionId)
    external
    view
    validPosition(user, positionId)
    returns (UserPosition memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition`|UserPosition struct containing: - isIsolated: Whether position is in isolation mode - isolatedAsset: Address of isolated asset (if applicable) - debtAmount: Current principal debt amount - lastInterestAccrual: Timestamp of last interest accrual|


### getUserCollateralAmount

Retrieves the amount of collateral asset a user has in a specific position

*Checks the collateral amount mapping for a specific user's position*

**Notes:**
- security: Validates position exists via validPosition modifier

- access: Public view function, no state modifications


```solidity
function getUserCollateralAmount(address user, uint256 positionId, address asset)
    external
    view
    validPosition(user, positionId)
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to query|
|`asset`|`address`|The address of the collateral asset to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The amount of collateral asset in the position (in asset's native decimals)|


### getProtocolSnapshot

Returns a comprehensive snapshot of the protocol's current state

*Aggregates key protocol metrics and parameters into a single struct*

**Notes:**
- state-access: Read-only view of protocol state

- calculations: Uses:
- getUtilization() for usage metrics
- getBorrowRate() for current rates
- getSupplyRate() for LP returns


```solidity
function getProtocolSnapshot() external view returns (ProtocolSnapshot memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ProtocolSnapshot`|ProtocolSnapshot A struct containing: - utilization: Current protocol utilization rate - borrowRate: Base borrow rate (using STABLE tier as reference) - supplyRate: Current supply rate for liquidity providers - totalBorrow: Total amount borrowed from protocol - totalSuppliedLiquidity: Total USDC supplied to protocol - targetReward: Current target reward amount - rewardInterval: Duration of reward period - rewardableSupply: Minimum supply for reward eligibility - baseProfitTarget: Protocol's base profit margin - liquidatorThreshold: Required governance tokens for liquidators - flashLoanFee: Current flash loan fee rate|


### calculateDebtWithInterest

Calculates the total debt amount including accrued interest for a position

*Process:
1. Returns 0 if position has no debt
2. Calculates elapsed time since last interest accrual
3. Determines applicable interest rate based on collateral tier
4. Compounds interest using ray math*

**Notes:**
- validation: Checks:
- Position exists (via validPosition modifier)

- calculations: Uses:
- For isolated positions: uses isolated asset's tier rate
- For cross-collateral: uses highest tier rate among assets
- Compounds interest based on time elapsed


```solidity
function calculateDebtWithInterest(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to calculate debt for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 Total debt including accrued interest|


### getAssetInfo

Retrieves complete configuration details for a listed asset

*Returns the full Asset struct from assetInfo mapping*


```solidity
function getAssetInfo(address asset) public view returns (Asset memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Asset`|Asset struct containing all configuration parameters|


### getAssetPrice

Gets the current USD price for an asset from its oracle

*Fetches price from the asset's configured Chainlink oracle*


```solidity
function getAssetPrice(address asset) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset to price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The asset price in USD (scaled by oracle decimals)|


### getUserPositionsCount

Gets the total number of positions for a user


```solidity
function getUserPositionsCount(address user) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The number of positions owned by the user|


### getUserPositions

Retrieves all positions for a user

*Returns complete array of UserPosition structs*


```solidity
function getUserPositions(address user) public view returns (UserPosition[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition[]`|UserPosition[] Array of all user's positions|


### getPositionLiquidationFee

Gets the liquidation bonus percentage for a specific position

*Returns tier-specific bonus for isolated positions, base fee for cross-collateral*

**Notes:**
- security: Validates position exists via validPosition modifier

- calculations: Uses:
- For isolated positions: returns tier's liquidation bonus
- For cross-collateral: returns base liquidation fee (1%)


```solidity
function getPositionLiquidationFee(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The liquidation bonus percentage in parts per million (e.g., 0.05e6 = 5%)|


### calculateCreditLimit

Calculates the maximum allowed borrowing amount for a position

*Considers collateral value, borrowing threshold, and isolation mode status*

**Notes:**
- security: Validates position exists via validPosition modifier

- calculations: 
- Isolated positions: uses single asset's value and threshold
- Cross-collateral: sums all assets' weighted values
- Formula: amount * price * borrowThreshold * WAD / decimals / 1000

- oracle: Uses Chainlink price feeds for asset valuations


```solidity
function calculateCreditLimit(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The maximum allowed borrowing amount in USDC|


### isLiquidatable

Determines if a position can be liquidated based on debt and collateral value

*A position is liquidatable if debt with interest >= collateral value*

**Notes:**
- validation: Checks:
- Position exists (via validPosition modifier)
- Position has non-zero debt

- calculations: Uses:
- calculateDebtWithInterest() for total debt
- calculateCreditLimit() for collateral value


```solidity
function isLiquidatable(address user, uint256 positionId) public view validPosition(user, positionId) returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if position can be liquidated, false otherwise|


### getPositionSummary

Provides a comprehensive overview of a position's current state

*Aggregates key position metrics into a single view*

**Notes:**
- validation: Checks position exists via validPosition modifier

- calculations: Uses:
- calculateCreditLimit() for collateral value
- calculateDebtWithInterest() for current debt


```solidity
function getPositionSummary(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
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
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to summarize|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalCollateralValue`|`uint256`|The total value of all collateral in the position|
|`currentDebt`|`uint256`|The current debt including accrued interest|
|`availableCredit`|`uint256`|The remaining borrowing capacity|
|`isIsolated`|`bool`|Whether the position is in isolation mode|
|`isolatedAsset`|`address`|The address of the isolated asset (if applicable)|


### getAssetDetails

Retrieves detailed information about a listed asset

*Aggregates asset configuration and current state into a single view*

**Notes:**
- calculations: Uses:
- getAssetPriceOracle() for current price
- getBorrowRate() for tier-specific rates

- state-access: Read-only access to:
- assetInfo mapping
- totalCollateral mapping
- tierBaseBorrowRate mapping
- tierLiquidationBonus mapping


```solidity
function getAssetDetails(address asset)
    public
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
|`asset`|`address`|The address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current USD price from oracle|
|`totalSupplied`|`uint256`|Total amount of asset supplied as collateral|
|`maxSupply`|`uint256`|Maximum supply threshold allowed|
|`borrowRate`|`uint256`|Current borrow rate for the asset's tier|
|`liquidationBonus`|`uint256`|Liquidation bonus percentage for the asset's tier|
|`tier`|`CollateralTier`|Risk classification tier of the asset|


### getLPInfo

Retrieves detailed information about a liquidity provider's position

*Calculates real-time LP token value and pending rewards*

**Notes:**
- calculations: 
- USDC value = (LP balance * total assets) / total supply
- Pending rewards = (targetReward * duration) / rewardInterval
- Rewards capped at ecosystem's maxReward

- validation: Checks:
- User must meet minimum supply threshold
- Sufficient time must have elapsed since last accrual


```solidity
function getLPInfo(address user)
    public
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
|`user`|`address`|The address of the liquidity provider to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lpTokenBalance`|`uint256`|The user's current balance of LP tokens|
|`usdcValue`|`uint256`|The current value of LP tokens in USDC|
|`lastAccrualTime`|`uint256`|Timestamp of last reward accrual|
|`isRewardEligible`|`bool`|Whether the user is eligible for rewards|
|`pendingRewards`|`uint256`|Amount of rewards currently available to claim|


### healthFactor

Calculates the health factor of a borrowing position

*The health factor represents the ratio of collateral value to debt
- Health factor > 1: Position is healthy
- Health factor < 1: Position can be liquidated
- Health factor = ∞: Position has no debt*

**Notes:**
- security: Validates position exists via validPosition modifier

- calculations: 
- For positions with no debt: returns type(uint256).max
- Otherwise: (collateral value * liquidation threshold) / total debt

- formula: 
healthFactor = (sum(asset amounts * prices * liquidation thresholds) * WAD) / debt


```solidity
function healthFactor(address user, uint256 positionId) public view validPosition(user, positionId) returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The position's health factor (scaled by WAD)|


### getPositionAssets

Gets the list of collateral assets in a position

*Retrieves the array of asset addresses used as collateral*

**Note:**
security: Validates position exists via validPosition modifier


```solidity
function getPositionAssets(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of addresses representing collateral assets|


### getPositionDebt

Gets the current debt amount for a position

*Returns raw debt amount without accrued interest*

**Notes:**
- security: Validates position exists via validPosition modifier

- note: For debt including interest, use calculateDebtWithInterest()


```solidity
function getPositionDebt(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The principal debt amount|


### getUtilization

Calculates the current protocol utilization rate

*Formula: utilization = (totalBorrow * WAD) / totalSuppliedLiquidity*

**Notes:**
- calculations: 
- Returns 0 if totalSuppliedLiquidity is 0
- Returns 0 if totalBorrow is 0
- Otherwise: (totalBorrow * WAD) / totalSuppliedLiquidity

- formula: 
utilization = (totalBorrow * 1e18) / totalSuppliedLiquidity


```solidity
function getUtilization() public view returns (uint256 u);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`u`|`uint256`|Current utilization rate scaled by WAD (1e18)|


### getSupplyRate

Calculates the current supply rate for liquidity providers

*Determines the rate based on protocol utilization and profit target*

**Notes:**
- calculations: 
- Returns 0 if totalSuppliedLiquidity is 0
- Target fee = (totalSupply * baseProfitTarget) / WAD
- Total value = USDC balance + totalBorrow
- Fee applied if total >= suppliedLiquidity + target
- Rate = ((WAD * total) / (totalSuppliedLiquidity + fee)) - WAD

- formula: 
supplyRate = ((totalAssets * WAD) / (totalSuppliedLiquidity + fee)) - WAD

- state-access: Read-only access to:
- totalSuppliedLiquidity
- totalSupply
- baseProfitTarget
- USDC balance
- totalBorrow


```solidity
function getSupplyRate() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The current supply rate in parts per million (e.g., 0.05e6 = 5%)|


### getBorrowRate

Calculates the current borrow rate for a specific collateral tier

*Combines base rate, utilization rate, and tier-specific premium*

**Notes:**
- calculations: 
- If utilization is 0: returns baseBorrowRate
- Otherwise calculates:
1. Supply rate converted to RAY format
2. Break-even rate based on supply interest
3. Base rate = max(breakEven + baseProfitTarget, baseBorrowRate)
4. Final rate = baseRate + (tierRate * utilization / WAD)

- formula: 
finalRate = baseRate + (tierBaseBorrowRate[tier] * utilization / WAD)

- state-access: Read-only access to:
- baseBorrowRate
- tierBaseBorrowRate mapping
- utilization rate


```solidity
function getBorrowRate(CollateralTier tier) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to calculate the rate for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The current borrow rate in parts per million (e.g., 0.05e6 = 5%)|


### isRewardable

Checks if a liquidity provider is eligible for rewards

*Determines reward eligibility based on time elapsed and supply amount*

**Notes:**
- validation: Checks:
- User has previously supplied liquidity (non-zero accrual time)
- Minimum reward interval has elapsed
- User meets minimum supply threshold

- calculations: 
- Base amount = (user LP balance * totalSuppliedLiquidity) / total supply
- Time check = current time - interval >= last accrual


```solidity
function isRewardable(address user) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the liquidity provider to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the user is eligible for rewards, false otherwise|


### getTierLiquidationFee

Gets the base liquidation bonus percentage for a specific collateral tier

*Direct accessor for tierLiquidationBonus mapping without additional calculations*

**Note:**
values: Typical values:
- STABLE: 5% (0.05e6)
- CROSS_A: 8% (0.08e6)
- CROSS_B: 10% (0.1e6)
- ISOLATED: 15% (0.15e6)


```solidity
function getTierLiquidationFee(CollateralTier tier) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`CollateralTier`|The collateral tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The liquidation bonus rate in parts per million (e.g., 0.05e6 = 5%)|


### getAssetPriceOracle

Gets the current price from a Chainlink oracle with additional safety checks

*Implements multiple safety checks for price validity:
1. Basic oracle health checks
2. Price volatility monitoring
3. Timestamp freshness verification*

**Notes:**
- validation: Checks:
- Price must be positive
- Round must be complete (answeredInRound >= roundId)
- Price must not be stale (< 8 hours old)
- For high volatility (>20% change):
Price must be fresh (< 1 hour old)

- errors: Throws:
- OracleInvalidPrice: If price <= 0
- OracleStalePrice: If round not complete
- OracleTimeout: If price too old
- OracleInvalidPriceVolatility: If high volatility with stale price


```solidity
function getAssetPriceOracle(address oracle) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|Address of the Chainlink price feed|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 Current price from the oracle (in oracle's decimals)|


### getHighestTier

Determines the highest risk tier among a position's collateral assets

*Iterates through position's assets and compares their tiers*

**Notes:**
- security: Validates position exists via validPosition modifier

- validation: Checks:
- Only considers assets with non-zero balances

- calculations: 
- Default tier is STABLE (lowest risk)
- Compares tier enum values numerically
- Higher enum value = higher risk tier

- tiers: Risk tiers in ascending order:
- STABLE (0)
- CROSS_A (1)
- CROSS_B (2)
- ISOLATED (3)


```solidity
function getHighestTier(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (CollateralTier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the position owner|
|`positionId`|`uint256`|The ID of the position to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CollateralTier`|CollateralTier The highest risk tier found among active collateral|


### balanceOf

Gets the token balance of an account

*Overrides ERC20Upgradeable and IERC20 implementations*


```solidity
function balanceOf(address account) public view virtual override(ERC20Upgradeable, IERC20) returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The number of tokens owned by the account|


### decimals

Returns the number of decimals used for token amounts

*Overrides ERC20Upgradeable implementation*


```solidity
function decimals() public view virtual override(ERC20Upgradeable) returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|uint8 The number of decimals (18)|


### totalSupply

Gets the total supply of tokens in circulation

*Overrides ERC20Upgradeable and IERC20 implementations*


```solidity
function totalSupply() public view virtual override(ERC20Upgradeable, IERC20) returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The total number of tokens|


### _transferCollateralToLiquidator

Internal function to transfer all collateral assets from a liquidated position

*Iterates through all position assets and transfers non-zero balances*

**Notes:**
- state: Updates:
- Clears collateral amounts in position
- Updates total collateral tracking
- Deletes position assets array

- events: Emits:
- WithdrawCollateral for each asset transferred


```solidity
function _transferCollateralToLiquidator(address user, uint256 positionId, address liquidator) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position being liquidated|
|`liquidator`|`address`|Address receiving the collateral assets|


### _rewardInternal

Internal function to process and distribute rewards for liquidity providers

*Calculates and distributes rewards based on time elapsed and amount supplied*

**Notes:**
- validation: Checks:
- Sufficient time has elapsed since last reward (>= rewardInterval)
- Amount meets minimum threshold (>= rewardableSupply)

- calculations: 
- Duration = current timestamp - last accrual time
- Base reward = (targetReward * duration) / rewardInterval
- Final reward = min(base reward, ecosystem maxReward)

- state: Updates:
- Clears liquidityAccrueTimeIndex for user

- events: Emits:
- Reward event with recipient and amount

- security: Only called internally during liquidity operations


```solidity
function _rewardInternal(uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of liquidity being withdrawn or managed|


### _update


```solidity
function _update(address from, address to, uint256 value)
    internal
    override(ERC20Upgradeable, ERC20PausableUpgradeable);
```

### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

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

### TransferFailed
Thrown when a token transfer fails


```solidity
error TransferFailed(address token, address from, address to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the token that failed to transfer|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`amount`|`uint256`|The amount that failed to transfer|

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

### NotIsolationEligible
Thrown when attempting to use ineligible asset in isolation mode


```solidity
error NotIsolationEligible(address asset);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The address of the asset not eligible for isolation|

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

### InvalidAssetForIsolation
Thrown when asset doesn't match isolation mode settings


```solidity
error InvalidAssetForIsolation(address user, uint256 positionId, address requestedAsset, address isolatedAsset);
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

### RewardTooHigh

```solidity
error RewardTooHigh(uint256 requested, uint256 max);
```

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

### BonusTooHigh
Thrown when trying to set a bonus above maximum allowed


```solidity
error BonusTooHigh(uint256 requested, uint256 maximum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested bonus|
|`maximum`|`uint256`|The maximum allowed bonus|

