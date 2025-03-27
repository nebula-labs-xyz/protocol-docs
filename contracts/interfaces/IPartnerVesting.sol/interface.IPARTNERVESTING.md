# IPARTNERVESTING
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/interfaces/IPartnerVesting.sol)

**Author:**
alexei@nebula-labs(dot)xyz

Interface for PartnerVesting.sol

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### cancelContract

Cancels the vesting contract and returns unvested funds to the creator

*Only callable by the contract creator (typically the Ecosystem contract)*


```solidity
function cancelContract() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens returned to the creator|


### release

Releases vested tokens to the beneficiary (partner)

*Can be called by anyone, but tokens are always sent to the contract owner (beneficiary)*


```solidity
function release() external;
```

### releasable

Calculates the amount of tokens that can be released at the current time

*Subtracts already released tokens from the total vested amount*


```solidity
function releasable() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens currently available for release|


### start

Returns the timestamp when vesting begins

*This value is immutable and set during contract creation*


```solidity
function start() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The start timestamp of the vesting period|


### duration

Returns the length of the vesting period

*This value is immutable and set during contract creation*


```solidity
function duration() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The duration in seconds of the vesting period|


### end

Returns the timestamp when vesting ends

*Calculated as start() + duration()*


```solidity
function end() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The end timestamp of the vesting period|


### released

Returns the total amount of tokens that have been released so far

*Used in vesting calculations to determine how many more tokens can be released*


```solidity
function released() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The cumulative amount of tokens released to the beneficiary|


## Events
### VestingInitialized
Emitted when a new vesting contract is initialized

*Triggered during contract creation with the vesting parameters*


```solidity
event VestingInitialized(address indexed token, address indexed beneficiary, uint64 startTimestamp, uint64 duration);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ERC20 token being vested|
|`beneficiary`|`address`|Address of the partner receiving the vested tokens|
|`startTimestamp`|`uint64`|UNIX timestamp when vesting begins|
|`duration`|`uint64`|Length of the vesting period in seconds|

### Cancelled
Emitted when a vesting contract is cancelled

*Triggered when the contract creator cancels the vesting and reclaims unvested tokens*


```solidity
event Cancelled(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of unvested tokens returned to the creator|

### ERC20Released
Emitted when vested tokens are released to the beneficiary

*Triggered each time tokens are claimed or automatically released during cancellation*


```solidity
event ERC20Released(address indexed token, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token that was released|
|`amount`|`uint256`|The amount of tokens released|

## Errors
### Unauthorized
Error thrown when an unauthorized address attempts a restricted action

*Used to restrict functions that should only be callable by the contract creator*


```solidity
error Unauthorized();
```

### ZeroAddress
Error thrown when a zero address is provided where a valid address is required

*Used in validation of constructor parameters*


```solidity
error ZeroAddress();
```

