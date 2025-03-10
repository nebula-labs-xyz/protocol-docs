# ILendefiView
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/interfaces/ILendefiView.sol)

**Author:**
Lendefi Protocol

Interface for the LendefiView contract that provides user-friendly data accessors

*This interface defines all the view functions available in the LendefiView module*


## Functions
### protocol

Returns the protocol contract address


```solidity
function protocol() external view returns (IPROTOCOL);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`IPROTOCOL`|The address of the main protocol contract|


### usdcInstance

Returns the USDC token contract address


```solidity
function usdcInstance() external view returns (IERC20);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`IERC20`|The address of the USDC token contract|


### yieldTokenInstance

Returns the yield token contract address


```solidity
function yieldTokenInstance() external view returns (ILendefiYieldToken);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ILendefiYieldToken`|The address of the yield token contract|


### ecosystemInstance

Returns the ecosystem contract address


```solidity
function ecosystemInstance() external view returns (IECOSYSTEM);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`IECOSYSTEM`|The address of the ecosystem contract|


### getPositionSummary

Gets a summary of a user's position


```solidity
function getPositionSummary(address user, uint256 positionId)
    external
    view
    returns (
        uint256 totalCollateralValue,
        uint256 currentDebt,
        uint256 availableCredit,
        bool isIsolated,
        IPROTOCOL.PositionStatus status
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|
|`positionId`|`uint256`|The ID of the position to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalCollateralValue`|`uint256`|The total USD value of all collateral in the position|
|`currentDebt`|`uint256`|The current debt amount including accrued interest|
|`availableCredit`|`uint256`|The remaining credit available to borrow|
|`isIsolated`|`bool`|Whether the position is in isolation mode|
|`status`|`IPROTOCOL.PositionStatus`|The current status of the position (Active, Liquidated, etc.)|


### getLPInfo

Gets information about a user's liquidity provider (LP) status


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
|`lpTokenBalance`|`uint256`|The user's balance of LP tokens|
|`usdcValue`|`uint256`|The current USDC value of the user's LP tokens|
|`lastAccrualTime`|`uint256`|The timestamp of the last interest accrual for the user|
|`isRewardEligible`|`bool`|Whether the user is eligible for rewards|
|`pendingRewards`|`uint256`|The amount of pending rewards available to the user|


### getProtocolSnapshot

Gets a snapshot of the entire protocol's state


```solidity
function getProtocolSnapshot() external view returns (ProtocolSnapshot memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ProtocolSnapshot`|A ProtocolSnapshot struct containing protocol-wide metrics and parameters|


## Structs
### ProtocolSnapshot
Structure containing a comprehensive snapshot of the protocol's state


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

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`utilization`|`uint256`|The current utilization rate of the protocol (WAD format)|
|`borrowRate`|`uint256`|The current borrow rate for CROSS_A tier assets (WAD format)|
|`supplyRate`|`uint256`|The current supply rate for liquidity providers (WAD format)|
|`totalBorrow`|`uint256`|The total amount borrowed from the protocol (in USDC)|
|`totalSuppliedLiquidity`|`uint256`|The total liquidity supplied to the protocol (in USDC)|
|`targetReward`|`uint256`|The target reward amount for LPs (per reward interval)|
|`rewardInterval`|`uint256`|The time interval for LP rewards (in seconds)|
|`rewardableSupply`|`uint256`|The minimum supply amount to be eligible for rewards (in USDC)|
|`baseProfitTarget`|`uint256`|The base profit target percentage (WAD format)|
|`liquidatorThreshold`|`uint256`|The minimum token balance required for liquidators|
|`flashLoanFee`|`uint256`|The fee percentage charged for flash loans (WAD format)|

