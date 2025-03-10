# Lendefi.sol Functions List

## Setup & Administration Functions
- `initialize(...)` - Initializes the contract with dependencies and parameters
- `pause()` - Pauses all protocol operations in emergencies
- `unpause()` - Unpauses protocol operations
- `_authorizeUpgrade(address)` - Authorization for contract upgrades with version tracking

## Flash Loan Functions
- `flashLoan(address receiver, uint256 amount, bytes calldata params)` - Executes a flash loan
- `updateFlashLoanFee(uint256 newFee)` - Updates the flash loan fee percentage

## Liquidity Provider Functions
- `supplyLiquidity(uint256 amount)` - Supplies USDC to the protocol for LP tokens
- `exchange(uint256 amount)` - Exchanges LP tokens for USDC with interest
- `claimReward()` - Claims accumulated rewards for eligible liquidity providers

## Position Management Functions
- `createPosition(address asset, bool isIsolated)` - Creates a new borrowing position with 1000 position limit
- `exitPosition(uint256 positionId)` - Closes a borrowing position completely
- `supplyCollateral(address asset, uint256 amount, uint256 positionId)` - Adds collateral to a position
- `withdrawCollateral(address asset, uint256 amount, uint256 positionId)` - Removes collateral from a position
- `interpositionalTransfer(uint256 fromPositionId, uint256 toPositionId, address asset, uint256 amount)` - Transfers collateral between positions

## Borrowing Functions
- `borrow(uint256 positionId, uint256 amount)` - Borrows USDC against collateral
- `repay(uint256 positionId, uint256 amount)` - Repays borrowed USDC with interest

## Liquidation Functions
- `liquidate(address user, uint256 positionId)` - Liquidates an undercollateralized position
- `_withdrawAllCollateral(address user, uint256 positionId, address recipient)` - Transfers liquidated collateral

## Protocol Parameter Update Functions
- `updateProtocolMetrics(uint256 profitTargetRate, uint256 borrowRate, uint256 rewardAmount, uint256 interval, uint256 supplyAmount, uint256 liquidatorAmount)` - Updates multiple protocol parameters in a single transaction

## Calculation Functions
- `calculateDebtWithInterest(address user, uint256 positionId)` - Calculates debt with interest
- `calculateCreditLimit(address user, uint256 positionId)` - Calculates borrowing limit
- `calculateCollateralValue(address user, uint256 positionId)` - Calculates collateral value
- `healthFactor(address user, uint256 positionId)` - Calculates position health factor
- `getUtilization()` - Calculates protocol utilization rate
- `getSupplyRate()` - Calculates current supply rate
- `getBorrowRate(ILendefiAssets.CollateralTier tier)` - Calculates borrow rate for a tier
- `getPositionTier(address user, uint256 positionId)` - Determines highest risk tier in a position
- `isLiquidatable(address user, uint256 positionId)` - Checks if a position can be liquidated
- `isRewardable(address user)` - Checks if user is eligible for rewards

## Position Collateral Functions
- `_processDeposit(address asset, uint256 amount, uint256 positionId)` - Internal function to handle collateral deposits
- `_processWithdrawal(address asset, uint256 amount, uint256 positionId)` - Internal function to handle collateral withdrawals
- `_processRepay(uint256 positionId, uint256 proposedAmount, UserPosition storage position)` - Internal function to handle debt repayments

## View Functions
- `getUserPosition(address user, uint256 positionId)` - Gets position details
- `getUserPositions(address user)` - Gets all positions for a user (limited by 1000 position cap)
- `getUserPositionsCount(address user)` - Gets number of positions for a user
- `getCollateralAmount(address user, uint256 positionId, address asset)` - Gets collateral amount
- `getPositionCollateralAssets(address user, uint256 positionId)` - Gets collateral assets list using EnumerableMap
- `getLiquidityAccrueTimeIndex(address user)` - Gets the timestamp of last reward accrual
- `getPositionLiquidationFee(address user, uint256 positionId)` - Gets position liquidation fee

## State Variables
- `maxPositionsPerUser` - Maximum number of positions a user can create (1000)
- `positionCollateral` - Mapping using EnumerableMap.AddressToUintMap for efficient collateral tracking
- `totalBorrow`, `totalSuppliedLiquidity` - Core protocol accounting variables
- `targetReward`, `rewardInterval`, `rewardableSupply` - Reward system parameters
- `baseBorrowRate`, `baseProfitTarget` - Interest rate parameters
- `liquidatorThreshold` - Minimum governance tokens required for liquidations
- `flashLoanFee`, `totalFlashLoanFees` - Flash loan parameters and accounting
- `version` - Implementation version counter

## Core Modifiers
- `validPosition(address user, uint256 positionId)` - Ensures position exists
- `activePosition(address user, uint256 positionId)` - Ensures position exists and is active
- `validAsset(address asset)` - Ensures asset is whitelisted
- `validAmount(uint256 amount)` - Ensures amount is greater than zero