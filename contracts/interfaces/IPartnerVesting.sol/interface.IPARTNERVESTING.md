# IPARTNERVESTING
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IPartnerVesting.sol)

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
### Cancelled
*Emitted when partner vesting is cancelled and remaining tokens returned to timelock*


```solidity
event Cancelled(uint256 amount);
```

### ERC20Released
*Emitted when tokens are released to the beneficiary*


```solidity
event ERC20Released(address indexed token, uint256 amount);
```

