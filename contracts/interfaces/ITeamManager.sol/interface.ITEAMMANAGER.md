# ITEAMMANAGER
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/interfaces/ITeamManager.sol)

Interface for the Lendefi DAO Team Manager

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Functions
### pause

*Pause contract.*


```solidity
function pause() external;
```

### unpause

*Unpause contract.*


```solidity
function unpause() external;
```

### addTeamMember

*Create and fund a vesting contract for a new team member*


```solidity
function addTeamMember(address beneficiary, uint256 amount, uint256 cliff, uint256 duration) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`beneficiary`|`address`|beneficiary address|
|`amount`|`uint256`|token amount|
|`cliff`|`uint256`|cliff period in seconds|
|`duration`|`uint256`|vesting duration in seconds after cliff  (e.g. 24 months)|


### scheduleUpgrade

*Schedules an upgrade to a new implementation*


```solidity
function scheduleUpgrade(address newImplementation) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


### upgradeTimelockRemaining

*Returns the remaining time before a scheduled upgrade can be executed*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The time remaining in seconds, or 0 if no upgrade is scheduled or timelock has passed|


### version

*Getter for the UUPS version, incremented each time an upgrade occurs.*


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|version number (1,2,3)|


### allocations

*Getter for the amount of tokens allocated to team member.*


```solidity
function allocations(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of tokens allocated to member|


### vestingContracts

*Getter for the address of vesting contract created for team member.*


```solidity
function vestingContracts(address account) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|vesting contract address|


### supply

*Starting supply allocated to team.*


```solidity
function supply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount|


### totalAllocation

*Total amount of token allocated so far.*


```solidity
function totalAllocation() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount|


### pendingUpgrade

*Access the pending upgrade information*


```solidity
function pendingUpgrade() external view returns (address implementation, uint64 scheduledTime, bool exists);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`implementation`|`address`|The address of the pending implementation|
|`scheduledTime`|`uint64`|The timestamp when the upgrade was scheduled|
|`exists`|`bool`|Whether a pending upgrade exists|


### timelock

*Get the timelock address.*


```solidity
function timelock() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the timelock controller|


### cancelUpgrade

Cancels a previously scheduled upgrade

*Removes a pending upgrade from the schedule*

**Notes:**
- access: Restricted to UPGRADER_ROLE

- state-changes: 
- Clears the pendingUpgrade data
- Emits an UpgradeCancelled event


```solidity
function cancelUpgrade() external;
```

## Events
### Initialized
*Initialized Event.*


```solidity
event Initialized(address indexed src);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|

### Upgrade
*Upgrade Event.*


```solidity
event Upgrade(address indexed src, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|
|`implementation`|`address`|address|

### AddTeamMember
*AddTeamMember Event.*


```solidity
event AddTeamMember(address indexed account, address indexed vesting, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|member address|
|`vesting`|`address`|contract address|
|`amount`|`uint256`|of tokens allocated to vesting|

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
*Error thrown when an address parameter is zero*


```solidity
error ZeroAddress();
```

### ZeroAmount
*Error thrown when an amount parameter is zero*


```solidity
error ZeroAmount();
```

### BeneficiaryAlreadyExists
*Error thrown when a beneficiary already has an allocation*


```solidity
error BeneficiaryAlreadyExists(address beneficiary);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`beneficiary`|`address`|The address that already has an allocation|

### InvalidCliff
*Error thrown when cliff is outside allowed range*


```solidity
error InvalidCliff(uint256 provided, uint256 minAllowed, uint256 maxAllowed);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint256`|The provided cliff duration|
|`minAllowed`|`uint256`|The minimum allowed cliff duration|
|`maxAllowed`|`uint256`|The maximum allowed cliff duration|

### InvalidDuration
*Error thrown when duration is outside allowed range*


```solidity
error InvalidDuration(uint256 provided, uint256 minAllowed, uint256 maxAllowed);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint256`|The provided vesting duration|
|`minAllowed`|`uint256`|The minimum allowed vesting duration|
|`maxAllowed`|`uint256`|The maximum allowed vesting duration|

### SupplyExceeded
*Error thrown when allocation exceeds remaining supply*


```solidity
error SupplyExceeded(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested allocation amount|
|`available`|`uint256`|The available supply|

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

### ValidationFailed
*Error thrown for general validation failures*


```solidity
error ValidationFailed(string reason);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`reason`|`string`|Description of the validation failure|

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

