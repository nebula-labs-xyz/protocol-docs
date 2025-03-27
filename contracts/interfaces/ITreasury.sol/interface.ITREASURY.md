# ITREASURY
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/interfaces/ITreasury.sol)

Interface for the Treasury contract with linear vesting, timelock upgrades and multisig support

*Defines all external functions and events for the Treasury contract*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Functions
### pause

Pauses all token transfers and releases

*Can only be called by accounts with the PAUSER_ROLE*


```solidity
function pause() external;
```

### unpause

Unpauses token transfers and releases

*Can only be called by accounts with the PAUSER_ROLE*


```solidity
function unpause() external;
```

### releasable

Returns the amount of ETH that can be released now


```solidity
function releasable() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of releasable ETH|


### releasable

Returns the amount of a specific token that can be released now


```solidity
function releasable(address token) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of releasable tokens|


### release

Releases a specific amount of vested ETH

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function release(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address that will receive the ETH|
|`amount`|`uint256`|The amount of ETH to release|


### release

Releases a specific amount of vested tokens

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function release(address token, address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to release|
|`to`|`address`|The address that will receive the tokens|
|`amount`|`uint256`|The amount of tokens to release|


### updateVestingSchedule

Updates the vesting schedule parameters

*Can only be called by accounts with the DEFAULT_ADMIN_ROLE*


```solidity
function updateVestingSchedule(uint256 newStart, uint256 newDuration) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newStart`|`uint256`|The new start timestamp|
|`newDuration`|`uint256`|The new duration in seconds|


### emergencyWithdrawToken

Withdraws funds in case of emergency

*Can only be called by accounts with the MANAGER_ROLE*

*Always sends funds to the timelock controller*


```solidity
function emergencyWithdrawToken(address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token to withdraw|


### emergencyWithdrawEther

Withdraws funds in case of emergency

*Can only be called by accounts with the MANAGER_ROLE*

*Always sends funds to the timelock controller*


```solidity
function emergencyWithdrawEther() external;
```

### scheduleUpgrade

Schedules an upgrade to a new implementation

*Can only be called by accounts with the UPGRADER_ROLE*


```solidity
function scheduleUpgrade(address newImplementation) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


### upgradeTimelockRemaining

Returns the remaining time before a scheduled upgrade can be executed


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The time remaining in seconds, or 0 if no upgrade is scheduled or timelock has passed|


### timelockAddress

Get the timelock controller address


```solidity
function timelockAddress() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The timelock controller address|


### vestedAmount

Returns the pending upgrade information

Calculates the amount of ETH vested at a specific timestamp


```solidity
function vestedAmount(uint256 timestamp) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint256`|The timestamp to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Information about the pending upgrade|


### vestedAmount

Calculates the amount of tokens vested at a specific timestamp


```solidity
function vestedAmount(address token, uint256 timestamp) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to check|
|`timestamp`|`uint256`|The timestamp to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The vested amount of tokens|


### version

Returns the current contract version


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|Current version number|


### start

Returns the start timestamp of the vesting period


```solidity
function start() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Start timestamp|


### duration

Returns the duration of the vesting period


```solidity
function duration() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Duration in seconds|


### end

Returns the end timestamp of the vesting period


```solidity
function end() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|End timestamp (start + duration)|


### released

Returns the amount of ETH already released


```solidity
function released() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of ETH released so far|


### released

Returns the amount of a specific token already released


```solidity
function released(address token) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of tokens released so far|


### MANAGER_ROLE

Returns the MANAGER_ROLE identifier used for access control


```solidity
function MANAGER_ROLE() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The keccak256 hash of "MANAGER_ROLE"|


### PAUSER_ROLE

Returns the PAUSER_ROLE identifier used for access control


```solidity
function PAUSER_ROLE() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The keccak256 hash of "PAUSER_ROLE"|


### UPGRADER_ROLE

Returns the UPGRADER_ROLE identifier used for access control


```solidity
function UPGRADER_ROLE() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The keccak256 hash of "UPGRADER_ROLE"|


## Events
### Initialized
*Emitted when the contract is initialized*


```solidity
event Initialized(address indexed initializer, uint256 startTime, uint256 duration);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initializer`|`address`|Address that called the initialize function|
|`startTime`|`uint256`|Start timestamp of the vesting schedule|
|`duration`|`uint256`|Duration of the vesting period in seconds|

### EthReleased
*Emitted when ETH is released from the treasury*


```solidity
event EthReleased(address indexed to, uint256 amount, uint256 remainingReleasable);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Address receiving the ETH|
|`amount`|`uint256`|Amount of ETH released|
|`remainingReleasable`|`uint256`|Amount of ETH still available for release|

### TokenReleased
*Emitted when ERC20 tokens are released from the treasury*


```solidity
event TokenReleased(address indexed token, address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ERC20 token being released|
|`to`|`address`|Address receiving the tokens|
|`amount`|`uint256`|Amount of tokens released|

### VestingScheduleUpdated
*Emitted when the vesting schedule parameters are updated*


```solidity
event VestingScheduleUpdated(address indexed updater, uint256 newStart, uint256 newDuration);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|Address that updated the vesting schedule|
|`newStart`|`uint256`|New start timestamp of the vesting schedule|
|`newDuration`|`uint256`|New duration of the vesting period in seconds|

### EmergencyWithdrawal
*Emitted when funds are withdrawn via the emergency withdrawal function*


```solidity
event EmergencyWithdrawal(address indexed token, address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token withdrawn (address(0) for ETH)|
|`to`|`address`|Address receiving the funds|
|`amount`|`uint256`|Amount withdrawn|

### Upgraded
*Emitted when the contract implementation is upgraded*


```solidity
event Upgraded(address indexed upgrader, address indexed implementation, uint32 version);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`upgrader`|`address`|Address that performed the upgrade|
|`implementation`|`address`|Address of the new implementation|
|`version`|`uint32`|New version number after the upgrade|

### Received
*Emitted when ETH is received by the contract*


```solidity
event Received(address indexed src, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|Address that sent ETH to the contract|
|`amount`|`uint256`|Amount of ETH received|

### UpgradeScheduled
*Emitted when an upgrade is scheduled*


```solidity
event UpgradeScheduled(
    address indexed sender, address indexed implementation, uint64 scheduledTime, uint64 effectiveTime
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sender`|`address`|The address that scheduled the upgrade|
|`implementation`|`address`|The new implementation address|
|`scheduledTime`|`uint64`|The time when the upgrade was scheduled|
|`effectiveTime`|`uint64`|The time when the upgrade can be executed|

### UpgradeCancelled
Emitted when a scheduled upgrade is cancelled


```solidity
event UpgradeCancelled(address indexed canceller, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`canceller`|`address`|The address that cancelled the upgrade|
|`implementation`|`address`|The implementation address that was cancelled|

## Errors
### ZeroAddress
*Thrown when an operation receives the zero address where a non-zero address is required*


```solidity
error ZeroAddress();
```

### ZeroAmount
*Thrown when an operation receives a zero amount where a non-zero amount is required*


```solidity
error ZeroAmount();
```

### InsufficientVestedAmount
*Thrown when trying to release more funds than what's currently vested*


```solidity
error InsufficientVestedAmount(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The amount requested for release|
|`available`|`uint256`|The actual amount available for release|

### InvalidDuration
*Thrown when attempting to set an invalid vesting duration*


```solidity
error InvalidDuration(uint256 minimum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`minimum`|`uint256`|The minimum allowed duration|

### UpgradeTimelockActive
*Error thrown when trying to execute an upgrade too soon*


```solidity
error UpgradeTimelockActive(uint256 remainingTime);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`remainingTime`|`uint256`|The time remaining until upgrade can be executed|

### UpgradeNotScheduled
*Error thrown when trying to execute an upgrade that wasn't scheduled*


```solidity
error UpgradeNotScheduled();
```

### ImplementationMismatch
*Error thrown when trying to execute an upgrade with wrong implementation*


```solidity
error ImplementationMismatch(address expected, address provided);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`expected`|`address`|The expected implementation address|
|`provided`|`address`|The provided implementation address|

### ZeroBalance
*Error thrown when attempting operations with zero balance*


```solidity
error ZeroBalance();
```

## Structs
### UpgradeRequest
Upgrade request details

*Tracks pending contract upgrades with timelock*


```solidity
struct UpgradeRequest {
    address implementation;
    uint64 scheduledTime;
    bool exists;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`implementation`|`address`|New implementation contract address|
|`scheduledTime`|`uint64`|When the upgrade was requested|
|`exists`|`bool`|Whether this upgrade request is active|

