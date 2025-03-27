# LendefiView
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/lender/LendefiView.sol)

**Inherits:**
[ILendefiView](/contracts/interfaces/ILendefiView.sol/interface.ILendefiView.md)

**Author:**
Lendefi Protocol Team

View-only module for Lendefi protocol providing user-friendly data accessors

LendefiView provides consolidated view functions for the protocol's state

*Separating these functions reduces the main contract's size while providing
convenient aggregated views of protocol state for front-end applications*

*This contract doesn't hold any assets or modify state, it only aggregates data*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## State Variables
### protocol
Main protocol contract reference


```solidity
IPROTOCOL public protocol;
```


### usdcInstance
USDC token contract reference


```solidity
IERC20 public usdcInstance;
```


### yieldTokenInstance
Yield token (LP token) contract reference


```solidity
ILendefiYieldToken public yieldTokenInstance;
```


### ecosystemInstance
Ecosystem contract reference for rewards calculation


```solidity
IECOSYSTEM public ecosystemInstance;
```


## Functions
### constructor

Initializes the LendefiView contract with required contract references

*All address parameters must be non-zero to ensure proper functionality*


```solidity
constructor(address _protocol, address _usdc, address _yieldToken, address _ecosystem);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_protocol`|`address`|Address of the main Lendefi protocol contract|
|`_usdc`|`address`|Address of the USDC token contract|
|`_yieldToken`|`address`|Address of the LP token contract|
|`_ecosystem`|`address`|Address of the ecosystem contract for rewards|


### getPositionSummary

Provides a comprehensive summary of a user's position

*Aggregates multiple protocol calls into one convenient view function*


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
|`user`|`address`|The address of the position owner|
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

Provides detailed information about a user's liquidity provision

*Calculates the current value of LP tokens and pending rewards*


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
|`lpTokenBalance`|`uint256`|The user's balance of LP tokens|
|`usdcValue`|`uint256`|The current USDC value of the user's LP tokens|
|`lastAccrualTime`|`uint256`|The timestamp of the last interest accrual for the user|
|`isRewardEligible`|`bool`|Whether the user is eligible for rewards|
|`pendingRewards`|`uint256`|The amount of pending rewards available to the user|


### getProtocolSnapshot

Gets a comprehensive snapshot of the entire protocol's state

*Aggregates multiple protocol metrics into a single convenient struct*


```solidity
function getProtocolSnapshot() external view returns (ProtocolSnapshot memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ProtocolSnapshot`|A ProtocolSnapshot struct containing all key protocol metrics and parameters|


