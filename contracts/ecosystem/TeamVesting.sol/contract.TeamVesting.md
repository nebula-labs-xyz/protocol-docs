# TeamVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/TeamVesting.sol)

**Inherits:**
[ITEAMVESTING](/contracts/interfaces/ITeamVesting.sol/interface.ITEAMVESTING.md), Context, Ownable2Step, ReentrancyGuard

Cancellable Vesting contract

Offers flexible withdrawal schedule (gas efficient)

*Implements secure linear vesting for the DAO team*

**Note:**
security-contact: security@nebula-labs.xyz


## State Variables
### _start
*start timestamp*


```solidity
uint64 private immutable _start;
```


### _duration
*duration seconds*


```solidity
uint64 private immutable _duration;
```


### _token
*token address*


```solidity
address private immutable _token;
```


### _timelock
*timelock address*


```solidity
address public immutable _timelock;
```


### _tokensReleased
*amount of tokens released*


```solidity
uint256 private _tokensReleased;
```


## Functions
### onlyTimelock

*Throws if called by any account other than the timelock.*


```solidity
modifier onlyTimelock();
```

### constructor

*Sets the owner to beneficiary address, the start timestamp and the
vesting duration of the vesting contract.*


```solidity
constructor(address token, address timelock, address beneficiary, uint64 startTimestamp, uint64 durationSeconds)
    Ownable(beneficiary);
```

### cancelContract

*Allows the DAO to cancel the contract in case the team member leaves.
First releases any vested tokens to the beneficiary, then returns
the remaining unvested tokens to the timelock controller.
Can be called multiple times but will only transfer the remaining balance.*


```solidity
function cancelContract() external nonReentrant onlyTimelock;
```

### release

*Release the tokens that have already vested.
Emits a {ERC20Released} event.*


```solidity
function release() public virtual;
```

### start

*Getter for the start timestamp.*


```solidity
function start() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|start timestamp|


### duration

*Getter for the vesting duration.*


```solidity
function duration() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|duration seconds|


### end

*Getter for the end timestamp.*


```solidity
function end() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|end timestamp|


### released

*Getter for the amount of token already released*


```solidity
function released() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of tokens released so far|


### releasable

*Getter for the amount of releasable tokens.*


```solidity
function releasable() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested tokens|


### _checkTimelock

*Throws if the sender is not the timelock.*


```solidity
function _checkTimelock() internal view virtual;
```

### vestedAmount

*Calculates the amount of tokens that has already vested. Default implementation is a linear vesting curve.*


```solidity
function vestedAmount(uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount vested|


### _vestingSchedule

*Implementation of the vesting formula. This returns the amount vested, as a function of time, for
an asset given its total historical allocation.*


```solidity
function _vestingSchedule(uint256 totalAllocation, uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAllocation`|`uint256`|initial amount|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount vested|


## Events
### VestingInitialized
*Contract initialization event*


```solidity
event VestingInitialized(
    address indexed token, address indexed beneficiary, address indexed timelock, uint64 startTimestamp, uint64 duration
);
```

## Errors
### Unauthorized
*Custom errors*


```solidity
error Unauthorized();
```

### ZeroAddress

```solidity
error ZeroAddress();
```

