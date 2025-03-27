# IPARTNERVESTING
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/IPartnerVesting.sol)

Interface for partner vesting contracts with cancellation capabilities


## Functions
### cancelContract

Cancels the vesting contract and returns unvested funds to timelock

*Only callable by timelock*


```solidity
function cancelContract() external returns (uint256);
```

### release

Releases vested tokens to the beneficiary (partner)


```solidity
function release() external;
```

### releasable

Returns the amount of tokens that can be released at the current time


```solidity
function releasable() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of releasable tokens|


### start

Returns the start time of the vesting period


```solidity
function start() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The start timestamp|


### duration

Returns the duration of the vesting period in seconds


```solidity
function duration() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The duration in seconds|


### end

Returns the end time of the vesting period


```solidity
function end() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The end timestamp|


### released

Returns the amount of tokens already released


```solidity
function released() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens released|


## Events
### VestingInitialized
Emitted when the vesting contract is initialized


```solidity
event VestingInitialized(
    address indexed token, address indexed beneficiary, address indexed timelock, uint64 startTimestamp, uint64 duration
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ERC20 token being vested|
|`beneficiary`|`address`|Address that will receive the vested tokens|
|`timelock`|`address`|Address of the governance timelock|
|`startTimestamp`|`uint64`|When the vesting schedule starts|
|`duration`|`uint64`|Length of the vesting period in seconds|

### Cancelled
Emitted when the vesting contract is cancelled


```solidity
event Cancelled(uint256 remainingTokens);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`remainingTokens`|`uint256`|Amount of unvested tokens returned to the creator|

### ERC20Released
Emitted when tokens are released to the beneficiary


```solidity
event ERC20Released(address indexed token, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ERC20 token being released|
|`amount`|`uint256`|Amount of tokens released|

## Errors
### Unauthorized
Unauthorized access attempt

*Thrown when a function restricted to the creator is called by someone else*


```solidity
error Unauthorized();
```

### ZeroAddress
Zero address provided for a critical parameter

*Thrown when token, timelock, or beneficiary address is zero*


```solidity
error ZeroAddress();
```

