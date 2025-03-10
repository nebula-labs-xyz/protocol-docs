# Lendefi
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/lender/Lendefi.sol)

**Inherits:**
[IPROTOCOL](/contracts/interfaces/IProtocol.sol/interface.IPROTOCOL.md), PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

**Author:**
alexei@nebula-labs(dot)xyz

## State Variables
### WAD
*Utility for set operations on address collections*

*Standard decimals for percentage calculations (1e6 = 100%)*


```solidity
uint256 internal constant WAD = 1e6;
```


### PAUSER_ROLE
*Role identifier for users authorized to pause/unpause the protocol*


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE
*Role identifier for users authorized to manage protocol parameters*


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
*Role identifier for users authorized to upgrade the contract*


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### usdcInstance
*Reference to the USDC stablecoin contract used for lending/borrowing*


```solidity
IERC20 internal usdcInstance;
```


### tokenInstance
*Reference to the protocol's governance token contract*


```solidity
IERC20 internal tokenInstance;
```


### ecosystemInstance
*Reference to the ecosystem contract for managing rewards*


```solidity
IECOSYSTEM internal ecosystemInstance;
```


### yieldTokenInstance
*Reference to the yield token (LP token) contract*


```solidity
ILendefiYieldToken internal yieldTokenInstance;
```


### assetsModule
*Reference to the assets module for collateral management*


```solidity
ILendefiAssets internal assetsModule;
```


### totalBorrow
Total amount borrowed from the protocol (in USDC)


```solidity
uint256 public totalBorrow;
```


### totalSuppliedLiquidity
Total liquidity supplied to the protocol (in USDC)


```solidity
uint256 public totalSuppliedLiquidity;
```


### totalAccruedBorrowerInterest
Cumulative interest accrued by borrowers since protocol inception


```solidity
uint256 public totalAccruedBorrowerInterest;
```


### totalAccruedSupplierInterest
Cumulative interest earned by suppliers since protocol inception


```solidity
uint256 public totalAccruedSupplierInterest;
```


### targetReward
Target amount of governance tokens for LP rewards per interval


```solidity
uint256 public targetReward;
```


### rewardInterval
Time period for LP reward eligibility in seconds


```solidity
uint256 public rewardInterval;
```


### rewardableSupply
Minimum supply amount required for reward eligibility (in USDC)


```solidity
uint256 public rewardableSupply;
```


### baseBorrowRate
Base annual interest rate for borrowing (in WAD format)


```solidity
uint256 public baseBorrowRate;
```


### baseProfitTarget
Target profit rate for the protocol (in WAD format)


```solidity
uint256 public baseProfitTarget;
```


### liquidatorThreshold
Minimum governance token balance required to perform liquidations


```solidity
uint256 public liquidatorThreshold;
```


### flashLoanFee
Fee percentage charged for flash loans (in basis points)


```solidity
uint256 public flashLoanFee;
```


### totalFlashLoanFees
Total fees collected from flash loans since protocol inception


```solidity
uint256 public totalFlashLoanFees;
```


### version
Current contract implementation version


```solidity
uint8 public version;
```


### treasury
Address of the treasury that receives protocol fees


```solidity
address public treasury;
```


### assetTVL
Total value locked per asset


```solidity
mapping(address => uint256) public assetTVL;
```


### positions
*Stores all borrowing positions for each user*

*Key: User address, Value: Array of positions*


```solidity
mapping(address => UserPosition[]) internal positions;
```


### positionCollateral
*Tracks collateral amounts for each asset in each position*

*Keys: User address, Position ID, Asset address, Value: Amount*


```solidity
mapping(address => mapping(uint256 => EnumerableMap.AddressToUintMap)) internal positionCollateral;
```


### liquidityAccrueTimeIndex
*Tracks the last time rewards were accrued for each liquidity provider*

*Key: User address, Value: Timestamp of last accrual*


```solidity
mapping(address src => uint256 time) internal liquidityAccrueTimeIndex;
```


### __gap
*Reserved storage gap for future upgrades*


```solidity
uint256[20] private __gap;
```


## Functions
### validPosition

*Ensures the position exists for the given user*


```solidity
modifier validPosition(address user, uint256 positionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to check|


### activePosition

*Ensures the position exists and is in active status*


```solidity
modifier activePosition(address user, uint256 positionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to check|


### validAsset

*Ensures the asset is whitelisted in the protocol*


```solidity
modifier validAsset(address asset);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to validate|


### validAmount

*Ensures the amount is greater than zero*


```solidity
modifier validAmount(uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount to check|


### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

Initializes the Lendefi protocol with core contract references and default parameters

*Sets up roles, connects to external contracts, and initializes protocol parameters*

**Notes:**
- access-control: Can only be called once during deployment

- events: Emits an Initialized event


```solidity
function initialize(
    address usdc,
    address govToken,
    address ecosystem,
    address treasury_,
    address timelock_,
    address yieldToken,
    address assetsModule_,
    address guardian
) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`usdc`|`address`|Address of the USDC token contract used for lending and borrowing|
|`govToken`|`address`|Address of the governance token contract used for protocol governance|
|`ecosystem`|`address`|Address of the ecosystem contract for managing rewards|
|`treasury_`|`address`|Address of the treasury for collecting protocol fees|
|`timelock_`|`address`|Address of the timelock contract for governance actions|
|`yieldToken`|`address`|Address of the yield token (LP token) contract|
|`assetsModule_`|`address`|Address of the assets module for managing supported collateral|
|`guardian`|`address`|Address of the protocol guardian with emergency powers|


### pause

Pauses the protocol in an emergency situation

*When paused, all state-changing functions will revert*

**Notes:**
- access-control: Restricted to PAUSER_ROLE

- events: Emits a Paused event from PausableUpgradeable


```solidity
function pause() external override onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses the protocol after an emergency is resolved

*Restores full functionality to all state-changing functions*

**Notes:**
- access-control: Restricted to PAUSER_ROLE

- events: Emits an Unpaused event from PausableUpgradeable


```solidity
function unpause() external override onlyRole(PAUSER_ROLE);
```

### flashLoan

Executes a flash loan of USDC to the specified receiver

*This function facilitates uncollateralized loans that must be repaid within the same transaction:
1. Validates available protocol liquidity
2. Calculates flash loan fee
3. Transfers requested amount to receiver
4. Calls receiver's executeOperation function
5. Verifies loan repayment plus fee
6. Updates protocol fee accounting
The receiver contract must implement IFlashLoanReceiver interface and handle the loan
in its executeOperation function. The loan plus fee must be repaid before the
transaction completes.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Amount must not exceed protocol's available liquidity
- Receiver must implement IFlashLoanReceiver interface
- Loan plus fee must be repaid within the same transaction

- state-changes: 
- Temporarily reduces protocol USDC balance by amount
- Increases totalFlashLoanFees by the fee amount after repayment
- Final protocol USDC balance increases by fee amount

- emits: 
- FlashLoan(msg.sender, receiver, address(usdcInstance), amount, fee)

- access-control: Available to any caller when protocol is not paused

- error-cases: 
- LowLiquidity: Thrown when amount exceeds available protocol liquidity
- FlashLoanFailed: Thrown when executeOperation returns false
- RepaymentFailed: Thrown when final balance is less than required amount
- ZeroAmount: Thrown when trying to borrow zero amount


```solidity
function flashLoan(address receiver, uint256 amount, bytes calldata params)
    external
    validAmount(amount)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`receiver`|`address`|Address of the contract receiving and handling the flash loan|
|`amount`|`uint256`|Amount of USDC to borrow|
|`params`|`bytes`|Arbitrary data to pass to the receiver for execution context|


### updateFlashLoanFee

Updates the fee percentage charged for flash loans

*Fee is expressed in basis points (e.g., 10 = 0.1%)*

**Notes:**
- access-control: Restricted to MANAGER_ROLE

- events: Emits an UpdateFlashLoanFee event

- error-cases: 
- InvalidFee: Thrown when fee exceeds the maximum allowed (100 basis points)


```solidity
function updateFlashLoanFee(uint256 newFee) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newFee`|`uint256`|New flash loan fee in basis points, capped at 1% (100 basis points)|


### supplyLiquidity

Supplies USDC liquidity to the lending pool

*This function handles the process of supplying liquidity to the protocol:
1. Calculating the appropriate amount of yield tokens to mint based on current exchange rate
2. Updating the protocol's total supplied liquidity accounting
3. Recording the timestamp for reward calculation purposes
4. Minting yield tokens to the supplier
5. Transferring USDC from the supplier to the protocol
The yield token amount (value) is calculated differently based on protocol state:
- Normal operation: (amount * existing_yield_token_supply) / total_protocol_assets
- Initial supply or zero utilization: value equals amount (1:1 ratio)
This ensures that early suppliers don't get diluted and later suppliers receive
tokens proportional to the current asset-to-yield-token ratio.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Caller must have approved sufficient USDC to the protocol

- state-changes: 
- Increases totalSuppliedLiquidity by amount
- Sets liquidityAccrueTimeIndex[msg.sender] to current timestamp
- Mints yield tokens to msg.sender based on calculated exchange rate
- Transfers USDC from msg.sender to the protocol

- emits: 
- SupplyLiquidity(msg.sender, amount)

- access-control: Available to any caller when protocol is not paused

- error-codes: 
- Reverts if protocol is paused (from whenNotPaused modifier)
- Reverts on reentrancy (from nonReentrant modifier)
- Reverts if USDC transfer fails


```solidity
function supplyLiquidity(uint256 amount) external validAmount(amount) nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of USDC to supply to the protocol|


### exchange

Exchanges yield tokens for USDC, withdrawing liquidity from the protocol

*This function handles the withdrawal of liquidity from the protocol, which includes:
1. Calculating the user's proportional share of the total protocol assets
2. Potentially taking protocol profit to the treasury
3. Updating protocol liquidity and interest accounting
4. Processing rewards if the user is eligible
5. Burning the yield tokens
6. Transferring USDC to the user
The redemption value is based on the current exchange rate determined by:
- Total protocol assets (USDC on hand + outstanding loans)
- Total yield token supply
This ensures users receive their proportional share of interest earned since deposit.
If the protocol has exceeded its profit target, a portion of the withdrawal amount
is minted as yield tokens to the treasury before calculating the user's redemption value.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Caller must have sufficient yield tokens
- Protocol must have sufficient USDC to fulfill the withdrawal

- state-changes: 
- Decreases totalSuppliedLiquidity by the base amount of the withdrawal
- Increases totalAccruedSupplierInterest by any interest earned
- Burns yield tokens from the caller
- Potentially mints yield tokens to the treasury (profit target)
- May reset the caller's liquidityAccrueTimeIndex if rewards are processed

- emits: 
- Exchange(msg.sender, amount, value) for the exchange operation
- Reward(msg.sender, rewardAmount) if rewards are issued

- access-control: Available to any caller when protocol is not paused

- error-cases: 
- ZeroAmount: Thrown when amount is zero


```solidity
function exchange(uint256 amount) external validAmount(amount) nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of yield tokens to exchange for underlying USDC|


### claimReward

Claims accumulated rewards for eligible liquidity providers

*Calculates time-based rewards and transfers them to the caller if eligible*

**Notes:**
- requirements: 
- Caller must have sufficient time since last claim (>= rewardInterval)
- Caller must have supplied minimum amount (>= rewardableSupply)

- state-changes: 
- Resets liquidityAccrueTimeIndex[msg.sender] if rewards are claimed

- emits: 
- Reward(msg.sender, rewardAmount) if rewards are issued

- access-control: Available to any caller when protocol is not paused


```solidity
function claimReward() external nonReentrant whenNotPaused returns (uint256 finalReward);
```

### createPosition

Creates a new borrowing position with specified isolation mode

*This function initializes a new borrowing position for the caller.
Two types of positions can be created:
1. Isolated positions: Limited to one specific collateral asset
2. Cross-collateral positions: Can use multiple asset types as collateral
For isolated positions, the asset parameter is immediately registered as the position's
collateral asset. For cross-collateral positions, no asset is registered initially;
assets must be added later via supplyCollateral().
The newly created position has no debt and is marked with ACTIVE status. The position ID
is implicitly the index of the position in the user's positions array (positions.length - 1).*

**Notes:**
- requirements: 
- Protocol must not be paused
- Asset must be whitelisted in the protocol (validAsset modifier)

- state-changes: 
- Creates new UserPosition entry in positions[msg.sender] array
- Sets position.isIsolated based on parameter
- Sets position.status to ACTIVE
- For isolated positions: adds the asset to the position's asset set

- emits: 
- PositionCreated(msg.sender, positionId, isIsolated)

- access-control: Available to any caller when protocol is not paused

- error-cases: 
- MaxPositionLimitReached: Thrown when user has reached the maximum position count
- NotListed: Thrown when asset is not whitelisted


```solidity
function createPosition(address asset, bool isIsolated) external validAsset(asset) nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the initial collateral asset for the position|
|`isIsolated`|`bool`|Whether the position uses isolation mode (true = single-asset only)|


### supplyCollateral

Supplies collateral assets to a position

*This function handles adding collateral to an existing position, which includes:
1. Processing the deposit via _processDeposit (validation and state updates)
2. Emitting the supply event
3. Transferring the assets from the caller to the protocol
The collateral can be used to either open new borrowing capacity or
strengthen the collateralization ratio of an existing debt position.
For isolated positions, only the initial asset type can be supplied.
For cross-collateral positions, multiple asset types can be added
(up to a maximum of 20 different assets per position).*

**Notes:**
- requirements: 
- Protocol must not be paused
- Position must exist and be in ACTIVE status
- Asset must be whitelisted in the protocol
- Asset must not be at its global capacity limit
- For isolated positions: asset must match the position's initial asset
- For isolated assets: position must be in isolation mode
- Position must have fewer than 20 different asset types (if adding a new asset type)

- state-changes: 
- Increases positionCollateralAmounts[msg.sender][positionId][asset] by amount
- Adds asset to positionCollateralAssets[msg.sender][positionId] if not already present
- Updates protocol-wide TVL for the asset
- Transfers asset tokens from msg.sender to the contract

- emits: 
- SupplyCollateral(msg.sender, positionId, asset, amount)

- access-control: Available to position owners when protocol is not paused

- error-cases: 
- ZeroAmount: Thrown when amount is zero
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status
- NotListed: Thrown when asset is not whitelisted
- AssetCapacityReached: Thrown when asset has reached global capacity limit
- IsolatedAssetViolation: Thrown when supplying isolated-tier asset to a cross position
- InvalidAssetForIsolation: Thrown when supplying an asset that doesn't match the isolated position's asset
- MaximumAssetsReached: Thrown when position already has 20 different asset types


```solidity
function supplyCollateral(address asset, uint256 amount, uint256 positionId)
    external
    validAmount(amount)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the collateral asset to supply|
|`amount`|`uint256`|Amount of the asset to supply as collateral|
|`positionId`|`uint256`|ID of the position to receive the collateral|


### withdrawCollateral

Withdraws collateral assets from a position

*This function handles removing collateral from an existing position, which includes:
1. Processing the withdrawal via _processWithdrawal (validation and state updates)
2. Emitting the withdrawal event
3. Transferring the assets from the protocol to the caller
The function ensures that the position remains sufficiently collateralized
after the withdrawal by checking that the remaining credit limit exceeds
the outstanding debt.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Position must exist and be in ACTIVE status
- For isolated positions: asset must match the position's initial asset
- Current balance must be greater than or equal to the withdrawal amount
- Position must remain sufficiently collateralized after withdrawal

- state-changes: 
- Decreases positionCollateralAmounts[msg.sender][positionId][asset] by amount
- Updates protocol-wide TVL for the asset
- For non-isolated positions: Removes asset entirely if balance becomes zero
- Transfers asset tokens from the contract to msg.sender

- emits: 
- WithdrawCollateral(msg.sender, positionId, asset, amount)

- access-control: Available to position owners when protocol is not paused

- error-cases: 
- ZeroAmount: Thrown when amount is zero
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status
- InvalidAssetForIsolation: Thrown when withdrawing an asset that doesn't match the isolated position's asset
- LowBalance: Thrown when not enough collateral balance to withdraw
- CreditLimitExceeded: Thrown when withdrawal would leave position undercollateralized


```solidity
function withdrawCollateral(address asset, uint256 amount, uint256 positionId)
    external
    validAmount(amount)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the collateral asset to withdraw|
|`amount`|`uint256`|Amount of the asset to withdraw|
|`positionId`|`uint256`|ID of the position from which to withdraw|


### borrow

Borrows USDC from the protocol against a collateralized position

*This function handles the borrowing process including:
1. Interest accrual on any existing debt
2. Multiple validation checks (liquidity, debt caps, credit limits)
3. Protocol and position accounting updates
4. USDC transfer to the borrower
The function first accrues any pending interest on existing debt to ensure
all calculations use up-to-date values. It then validates the borrow request
against protocol-wide constraints (available liquidity) and position-specific
constraints (isolation debt caps, credit limits).
After successful validation, it updates the position's debt, the protocol's
total debt accounting, and transfers the requested USDC amount to the borrower.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Position must exist and be in ACTIVE status
- Amount must be greater than zero
- Protocol must have sufficient liquidity
- For isolated positions: borrow must not exceed asset's isolation debt cap
- Total debt must not exceed position's credit limit

- state-changes: 
- Increases position.debtAmount by amount plus any accrued interest
- Updates position.lastInterestAccrual to current timestamp
- Increases totalBorrow by amount plus any accrued interest
- Increases totalAccruedBorrowerInterest if interest is accrued

- emits: 
- Borrow(msg.sender, positionId, amount)
- InterestAccrued(msg.sender, positionId, accruedInterest) if interest was accrued

- access-control: Available to position owners when protocol is not paused

- error-cases: 
- ZeroAmount: Thrown when amount is zero
- LowLiquidity: Thrown when protocol doesn't have enough available liquidity
- IsolationDebtCapExceeded: Thrown when exceeding an isolated asset's debt cap
- CreditLimitExceeded: Thrown when total debt would exceed position's credit limit
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status


```solidity
function borrow(uint256 positionId, uint256 amount)
    external
    validAmount(amount)
    activePosition(msg.sender, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|ID of the collateralized position to borrow against|
|`amount`|`uint256`|Amount of USDC to borrow (in base units with 6 decimals)|


### repay

Repays borrowed USDC for a position, including accrued interest

*This function handles the repayment process including:
1. Validation of the position's debt status
2. Processing the repayment via _processRepay (accounting updates)
3. Transferring USDC from the user to the protocol
The function supports both partial and full repayments. If the user attempts
to repay more than they owe, only the outstanding debt amount is taken.
Interest accrual is handled automatically by the _processRepay function.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Position must exist and be in ACTIVE status
- Position must have existing debt to repay

- state-changes: 
- Decreases position.debtAmount by the repayment amount
- Updates position.lastInterestAccrual to current timestamp
- Updates totalBorrow to reflect repayment and accrued interest
- Updates totalAccruedBorrowerInterest by adding newly accrued interest

- emits: 
- Repay(msg.sender, positionId, actualAmount)
- InterestAccrued(msg.sender, positionId, accruedInterest)

- access-control: Available to position owners when protocol is not paused

- error-cases: 
- ZeroAmount: Thrown when amount is zero
- NoDebt: Thrown when position has no debt to repay
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status


```solidity
function repay(uint256 positionId, uint256 amount)
    external
    activePosition(msg.sender, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|ID of the position with debt to repay|
|`amount`|`uint256`|Amount of USDC to repay (repays full debt if amount exceeds balance)|


### exitPosition

Closes a borrowing position by repaying all debt and withdrawing all collateral

*This function handles the complete position closure process, which includes:
1. Repaying any outstanding debt with interest (if debt exists)
2. Withdrawing all collateral assets back to the owner
3. Marking the position as CLOSED in the protocol
The function allows users to exit their positions with a single transaction
rather than calling separate repay and withdraw functions for each asset.
For full debt repayment, it uses the max uint256 value to signal "repay all".*

**Notes:**
- requirements: 
- Protocol must not be paused
- Position must exist and be in ACTIVE status
- If position has debt, caller must have approved sufficient USDC

- state-changes: 
- Repays any debt in the position (position.debtAmount = 0)
- Updates position.lastInterestAccrual if debt existed
- Removes all collateral assets from the position
- Updates protocol-wide TVL for each asset
- Sets position.status to CLOSED

- emits: 
- PositionClosed(msg.sender, positionId)
- Repay(msg.sender, positionId, actualAmount) if debt existed
- InterestAccrued(msg.sender, positionId, accruedInterest) if interest was accrued
- WithdrawCollateral(msg.sender, positionId, asset, amount) for each collateral asset

- access-control: Available to position owners when protocol is not paused

- error-cases: 
- ZeroAmount: Thrown when amount is zero (from validAmount modifier in _processRepay)
- InvalidPosition: Thrown when position doesn't exist (from activePosition modifier)
- InactivePosition: Thrown when position is not in ACTIVE status (from activePosition modifier)


```solidity
function exitPosition(uint256 positionId) external activePosition(msg.sender, positionId) nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|ID of the position to close|


### liquidate

Liquidates an undercollateralized position to maintain protocol solvency

*This function handles the liquidation process including:
1. Verification that the caller owns sufficient governance tokens
2. Confirmation that the position's health factor is below 1.0
3. Calculation of debt with accrued interest and liquidation fee
4. Position state updates (marking as liquidated, clearing debt)
5. Transferring debt+fee from liquidator to the protocol
6. Transferring all collateral assets to the liquidator
The liquidator receives all collateral assets in exchange for repaying
the position's debt plus a liquidation fee. The fee percentage varies
based on the collateral tier of the position's assets.*

**Notes:**
- requirements: 
- Protocol must not be paused
- Position must exist and be in ACTIVE status
- Caller must hold at least liquidatorThreshold amount of governance tokens
- Position's health factor must be below 1.0 (undercollateralized)

- state-changes: 
- Sets position.isIsolated to false
- Sets position.debtAmount to 0
- Sets position.lastInterestAccrual to 0
- Sets position.status to LIQUIDATED
- Decreases totalBorrow by the position's debt amount
- Increases totalAccruedBorrowerInterest by any accrued interest
- Removes all collateral assets from the position
- Updates protocol-wide TVL for each asset

- emits: 
- Liquidated(user, positionId, msg.sender)
- WithdrawCollateral(user, positionId, asset, amount) for each collateral asset

- access-control: Available to any caller with sufficient governance tokens when protocol is not paused

- error-cases: 
- NotEnoughGovernanceTokens: Thrown when caller doesn't have enough governance tokens
- NotLiquidatable: Thrown when position's health factor is above 1.0
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status


```solidity
function liquidate(address user, uint256 positionId)
    external
    activePosition(user, positionId)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner being liquidated|
|`positionId`|`uint256`|ID of the position to liquidate|


### interpositionalTransfer

Transfers collateral between two positions owned by the same user

*Validates both the withdrawal from the source position and the deposit to the destination position.
This function ensures that the collateral transfer adheres to the protocol's rules regarding
isolation and cross-collateral positions.*

**Notes:**
- access-control: Available to position owners when the protocol is not paused

- events: Emits an InterPositionalTransfer event

- requirements: 
- Protocol must not be paused
- Asset must be whitelisted in the protocol
- Source position must exist and be in ACTIVE status
- Destination position must exist and be in ACTIVE status
- Source position must have sufficient collateral balance of the specified asset
- Destination position must adhere to isolation rules if applicable

- state-changes: 
- Decreases the collateral amount of the specified asset in the source position
- Increases the collateral amount of the specified asset in the destination position
- Updates protocol-wide TVL for the asset

- error-cases: 
- ZeroAmount: Thrown when amount is zero
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status
- NotListed: Thrown when asset is not whitelisted
- LowBalance: Thrown when position doesn't have sufficient asset balance
- IsolatedAssetViolation: Thrown when transferring isolated-tier asset to a cross position
- InvalidAssetForIsolation: Thrown when asset doesn't match isolated position's asset
- MaximumAssetsReached: Thrown when destination position already has 20 different assets


```solidity
function interpositionalTransfer(uint256 fromPositionId, uint256 toPositionId, address asset, uint256 amount)
    external
    validAsset(asset)
    validAmount(amount)
    whenNotPaused
    nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fromPositionId`|`uint256`|The ID of the position to transfer collateral from|
|`toPositionId`|`uint256`|The ID of the position to transfer collateral to|
|`asset`|`address`|The address of the collateral asset to transfer|
|`amount`|`uint256`|The amount of the asset to transfer|


### updateProtocolMetrics

Updates multiple protocol parameters in a single transaction

*All parameters are validated against minimum or maximum constraints*

**Notes:**
- access-control: Restricted to MANAGER_ROLE

- events: Emits a ProtocolMetricsUpdated event

- error-cases: 
- InvalidProfitTarget: Thrown when profit target rate is below minimum
- InvalidBorrowRate: Thrown when borrow rate is below minimum
- InvalidRewardAmount: Thrown when reward amount exceeds maximum
- InvalidInterval: Thrown when interval is below minimum
- InvalidSupplyAmount: Thrown when supply amount is below minimum
- InvalidLiquidatorThreshold: Thrown when liquidator threshold is below minimum


```solidity
function updateProtocolMetrics(
    uint256 profitTargetRate,
    uint256 borrowRate,
    uint256 rewardAmount,
    uint256 interval,
    uint256 supplyAmount,
    uint256 liquidatorAmount
) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`profitTargetRate`|`uint256`|New base profit target rate (min 0.25%)|
|`borrowRate`|`uint256`|New base borrow rate (min 1%)|
|`rewardAmount`|`uint256`|New target reward amount (max 10,000 tokens)|
|`interval`|`uint256`|New reward interval in seconds (min 90 days)|
|`supplyAmount`|`uint256`|New minimum rewardable supply amount (min 20,000 USDC)|
|`liquidatorAmount`|`uint256`|New minimum liquidator token threshold (min 10 tokens)|


### getUserPosition

Retrieves a user's position data by ID

*Returns the full position struct including isolation status, debt, and status*

**Notes:**
- access-control: Available to any caller, read-only

- error-cases: 
- InvalidPosition: Thrown when position doesn't exist


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
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition`|UserPosition struct containing the position's details|


### getCollateralAmount

Gets the amount of a specific collateral asset in a position

*Returns zero for assets not used in the position*

**Note:**
access-control: Available to any caller, read-only


```solidity
function getCollateralAmount(address user, uint256 positionId, address asset)
    external
    view
    validPosition(user, positionId)
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to query|
|`asset`|`address`|Address of the collateral asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of the specified asset used as collateral|


### getPositionCollateralAssets

Gets all collateral asset addresses used in a position

*Returns an array of addresses that can be used to query amounts*


```solidity
function getPositionCollateralAssets(address user, uint256 positionId)
    external
    view
    validPosition(user, positionId)
    returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of addresses representing all collateral assets in the position|


### getLiquidityAccrueTimeIndex

Gets the timestamp of the last liquidity reward accrual for a user

*Used to determine reward eligibility and calculate reward amounts*


```solidity
function getLiquidityAccrueTimeIndex(address user) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The timestamp when rewards were last accrued (or 0 if never)|


### calculateDebtWithInterest

Calculates the current debt including accrued interest for a position

*Uses the appropriate interest rate based on the position's collateral tier*


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
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to calculate debt for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total debt amount including principal and accrued interest|


### getUserPositionsCount

Gets the number of positions owned by a user

*Includes all positions regardless of status (active, closed, liquidated)*


```solidity
function getUserPositionsCount(address user) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the user to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The number of positions created by the user|


### getUserPositions

Gets all positions owned by a user

*Returns the full array of position structs for the user*


```solidity
function getUserPositions(address user) public view returns (UserPosition[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the user to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UserPosition[]`|Array of UserPosition structs for all the user's positions|


### getPositionLiquidationFee

Gets the liquidation fee percentage for a position

*Based on the highest risk tier among the position's collateral assets*


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
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The liquidation fee percentage in WAD format (e.g., 0.05e6 = 5%)|


### calculateCreditLimit

Calculates the maximum borrowable amount for a position

*Based on collateral values and their respective borrow thresholds*

**Note:**
access-control: Available to any caller, read-only


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
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to calculate limit for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|credit - The maximum amount of USDC that can be borrowed against the position|


### calculateCollateralValue

Calculates the total USD value of all collateral in a position

*Uses oracle prices to convert collateral amounts to USD value*


```solidity
function calculateCollateralValue(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (uint256 value);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to calculate value for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`value`|`uint256`|- The total USD value of all collateral assets in the position|


### isLiquidatable

Determines if a position is eligible for liquidation

*Checks if health factor is below 1.0, indicating undercollateralization*


```solidity
function isLiquidatable(address user, uint256 positionId) public view activePosition(user, positionId) returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the position can be liquidated, false otherwise|


### healthFactor

Calculates the health factor of a position

*Health factor is the ratio of weighted collateral to debt, below 1.0 is liquidatable*

**Note:**
error-cases: 
- InvalidPosition: Thrown when position doesn't exist


```solidity
function healthFactor(address user, uint256 positionId) public view validPosition(user, positionId) returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to calculate health for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The position's health factor in WAD format (1.0 = 1e6)|


### getUtilization

Calculates the current protocol utilization rate

*Utilization = totalBorrow / totalSuppliedLiquidity, in WAD format*


```solidity
function getUtilization() public view returns (uint256 u);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`u`|`uint256`|The protocol's current utilization rate (0-1e6)|


### getSupplyRate

Calculates the current supply interest rate for liquidity providers

*Based on utilization, protocol fees, and available liquidity*


```solidity
function getSupplyRate() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The current annual supply interest rate in WAD format|


### getBorrowRate

Calculates the current borrow interest rate for a specific collateral tier

*Based on utilization, base rate, supply rate, and tier-specific jump rate*


```solidity
function getBorrowRate(ILendefiAssets.CollateralTier tier) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`ILendefiAssets.CollateralTier`|The collateral tier to calculate the borrow rate for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The current annual borrow interest rate in WAD format|


### isRewardable

Determines if a user is eligible for liquidity provider rewards

*Checks if the required time has passed and minimum supply amount is met*


```solidity
function isRewardable(address user) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the user to check for reward eligibility|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the user is eligible for rewards, false otherwise|


### getPositionTier

Determines the collateral tier of a position

*For cross-collateral positions, returns the highest risk tier among assets*


```solidity
function getPositionTier(address user, uint256 positionId)
    public
    view
    validPosition(user, positionId)
    returns (ILendefiAssets.CollateralTier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ILendefiAssets.CollateralTier`|The position's collateral tier (STABLE, CROSS_A, CROSS_B, or ISOLATED)|


### _processDeposit

Processes collateral deposit operations with validation and state updates

*Internal function that handles the core logic for adding collateral to a position,
enforcing protocol rules about isolation mode, asset capacity limits, and asset counts.
This function is called by both supplyCollateral and interpositionalTransfer.
The function performs several validations to ensure the deposit complies with
protocol rules:
1. Verifies the asset hasn't reached its global capacity limit
2. Enforces isolation mode rules (isolated assets can't be added to cross positions)
3. Ensures isolated positions only contain a single asset type
4. Limits positions to a maximum of 20 different asset types*

**Notes:**
- requirements: 
- Asset must be whitelisted in the protocol (validated by validAsset modifier)
- Position must exist and be in ACTIVE status (validated by activePosition modifier)
- Asset must not be at its global capacity limit
- For isolated assets: position must not be a cross-collateral position
- For isolated positions: asset must match the position's initial asset (if any exists)
- Position must have fewer than 20 different asset types (if adding a new asset type)

- state-changes: 
- Adds asset to positionCollateralAssets[msg.sender][positionId] if not already present
- Increases positionCollateralAmounts[msg.sender][positionId][asset] by amount
- Increases global assetTVL[asset] by amount

- emits: 
- TVLUpdated(asset, newTVL)

- error-cases: 
- NotListed: Thrown when asset is not whitelisted
- InvalidPosition: Thrown when position doesn't exist
- InactivePosition: Thrown when position is not in ACTIVE status
- AssetCapacityReached: Thrown when asset has reached its global capacity limit
- IsolatedAssetViolation: Thrown when supplying isolated-tier asset to a cross position
- InvalidAssetForIsolation: Thrown when asset doesn't match isolated position's asset
- MaximumAssetsReached: Thrown when position already has 20 different asset types


```solidity
function _processDeposit(address asset, uint256 amount, uint256 positionId)
    internal
    validAsset(asset)
    activePosition(msg.sender, positionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the collateral asset to deposit|
|`amount`|`uint256`|Amount of the asset to deposit (in the asset's native units)|
|`positionId`|`uint256`|ID of the position to receive the collateral|


### _processWithdrawal

Processes a collateral withdrawal operation with validation and state updates

*Internal function that handles the core logic for removing collateral from a position,
enforcing position solvency and validating withdrawal amounts.
This function is called by both withdrawCollateral and interpositionalTransfer.
The function performs several validations to ensure the withdrawal complies with
protocol rules:
1. For isolated positions, verifies the asset matches the position's designated asset
2. Checks that the position has sufficient balance of the asset
3. Updates the position's collateral tracking based on withdrawal amount
4. Ensures the position remains sufficiently collateralized after withdrawal*

**Notes:**
- requirements: 
- Position must exist and be in ACTIVE status (validated by activePosition modifier)
- For isolated positions: asset must match the position's designated asset
- Position must have sufficient balance of the specified asset
- After withdrawal, position must maintain sufficient collateral for any outstanding debt

- state-changes: 
- Decreases positionCollateralAmounts[msg.sender][positionId][asset] by amount
- For non-isolated positions: Removes asset entirely if balance becomes zero
- Decreases global assetTVL[asset] by amount

- emits: 
- TVLUpdated(asset, newTVL)

- error-cases: 
- InvalidAssetForIsolation: Thrown when trying to withdraw an asset that doesn't match the isolated position's asset
- LowBalance: Thrown when position doesn't have sufficient balance of the asset
- CreditLimitExceeded: Thrown when withdrawal would leave position undercollateralized


```solidity
function _processWithdrawal(address asset, uint256 amount, uint256 positionId)
    internal
    activePosition(msg.sender, positionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the collateral asset to withdraw|
|`amount`|`uint256`|Amount of the asset to withdraw (in the asset's native units)|
|`positionId`|`uint256`|ID of the position to withdraw from|


### _processRepay

Processes a repayment for a position and handles debt accounting

*This internal function manages all debt-related state changes during repayment:
1. Calculates the up-to-date debt including accrued interest
2. Tracks interest accrued since last update
3. Determines the actual amount to repay (capped at outstanding debt)
4. Updates the protocol's total debt accounting
5. Updates the position's debt and interest accrual timestamp
6. Emits relevant events
The function handles two key scenarios:
- Partial repayment: When proposedAmount < balance, repays that exact amount
- Full repayment: When proposedAmount >= balance, repays exactly the outstanding balance*

**Notes:**
- accounting: When calculating updated totalBorrow, the formula:
totalBorrow + (balance - actualAmount) - position.debtAmount
takes into account both newly accrued interest and the repayment amount

- events: Emits:
- Repay(msg.sender, positionId, actualAmount)
- InterestAccrued(msg.sender, positionId, accruedInterest)


```solidity
function _processRepay(uint256 positionId, uint256 proposedAmount, UserPosition storage position)
    internal
    validAmount(proposedAmount)
    returns (uint256 actualAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`positionId`|`uint256`|The ID of the position being repaid|
|`proposedAmount`|`uint256`|The amount the user is offering to repay (uncapped)|
|`position`|`UserPosition`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`actualAmount`|`uint256`|The actual amount that should be transferred from the user, which is the lesser of proposedAmount and the outstanding debt|


### _withdrawAllCollateral

Withdraws all collateral assets from a position to a specified recipient

*Internal function used during position closure and liquidation to remove and transfer
all collateral assets from a position. Iterates through all assets in the position,
clears them from the position's storage, and transfers them to the recipient.
The function handles the complete extraction of all assets regardless of the position's
state or collateralization ratio, and is intended for use only in terminal operations
like complete position closure or liquidation.*

**Notes:**
- state-changes: 
- Clears all assets from the position's collateral mapping
- Updates assetTVL for each asset withdrawn
- Transfers all assets to the recipient

- emits: 
- WithdrawCollateral(owner, positionId, asset, amount) for each asset
- TVLUpdated(asset, newTVL) for each asset


```solidity
function _withdrawAllCollateral(address owner, uint256 positionId, address recipient) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address of the position owner|
|`positionId`|`uint256`|ID of the position to withdraw all collateral from|
|`recipient`|`address`|Address that will receive the withdrawn collateral assets|


### _authorizeUpgrade

Authorizes an upgrade to a new implementation contract

*Increments the contract version and emits an event*

**Notes:**
- access-control: Restricted to UPGRADER_ROLE

- events: Emits an Upgrade event


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


