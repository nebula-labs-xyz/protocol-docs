# ITEAMMANAGER
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/ITeamManager.sol)

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

*Getter for the  address of vesting contract created for team member.*


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

## Errors
### CustomError
*Custom Error.*


```solidity
error CustomError(string msg);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`msg`|`string`|error desription|

