# TeamVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/TeamVesting.sol)

**Inherits:**
[ITEAMVESTING](/contracts/interfaces/ITeamVesting.sol/interface.ITEAMVESTING.md), Context, Ownable2Step, ReentrancyGuard

Cancellable Vesting contract

Offers flexible withdrawal schedule (gas efficient)

*Implements secure linear vesting for the DAO team*

**Note:**
security-contact: security@nebula-labs.xyz


## State Variables
### _start
*Start timestamp of the vesting period*


```solidity
uint64 private immutable _start;
```


### _duration
*Duration of the vesting period in seconds*


```solidity
uint64 private immutable _duration;
```


### _token
*Address of the token being vested*


```solidity
address private immutable _token;
```


### _timelock
*Address of the timelock controller that can cancel vesting*


```solidity
address public immutable _timelock;
```


### _tokensReleased
*Running total of tokens that have been released*


```solidity
uint256 private _tokensReleased;
```


## Functions
### onlyTimelock

Restricts function access to the timelock controller only

*Used for cancellation functionality*


```solidity
modifier onlyTimelock();
```

### constructor

Creates a new vesting contract for a team member

*Sets the beneficiary as the owner, initializes immutable vesting parameters*


```solidity
constructor(address token, address timelock, address beneficiary, uint64 startTimestamp, uint64 durationSeconds)
    Ownable(beneficiary);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ERC20 token to be vested|
|`timelock`|`address`|Address of the timelock controller|
|`beneficiary`|`address`|Address that will receive the vested tokens|
|`startTimestamp`|`uint64`|UNIX timestamp when vesting begins|
|`durationSeconds`|`uint64`|Duration of vesting period in seconds|


### cancelContract

Cancels the vesting contract, releasing vested tokens and returning unvested tokens

*First releases any vested tokens to the beneficiary, then returns remaining tokens to timelock*


```solidity
function cancelContract() external nonReentrant onlyTimelock returns (uint256 remainder);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remainder`|`uint256`|The amount of unvested tokens returned to the timelock|


### release

Releases vested tokens to the beneficiary

*Can be called by anyone but tokens are always sent to the owner (beneficiary)*


```solidity
function release() public virtual nonReentrant onlyOwner;
```

### start

Returns the timestamp when vesting starts

*This value is immutable and set during contract creation*


```solidity
function start() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The start timestamp of the vesting period|


### duration

Returns the duration of the vesting period

*This value is immutable and set during contract creation*


```solidity
function duration() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The duration in seconds of the vesting period|


### end

Returns the timestamp when vesting ends

*Calculated as start() + duration()*


```solidity
function end() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The end timestamp of the vesting period|


### released

Returns the amount of tokens already released

*Used in vesting calculations to determine how many more tokens can be released*


```solidity
function released() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens that have been released so far|


### releasable

Calculates the amount of tokens that can be released now

*Subtracts already released tokens from the total vested amount*


```solidity
function releasable() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens currently available to be released|


### _checkTimelock

Verifies the caller is the timelock controller

*Throws Unauthorized error if caller is not the timelock*


```solidity
function _checkTimelock() internal view virtual;
```

### vestedAmount

Calculates the amount of tokens that have vested by a given timestamp

*Internal function used by releasable()*


```solidity
function vestedAmount(uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint64`|The timestamp to calculate vested amount for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total amount of tokens vested at the specified timestamp|


### _vestingSchedule

Calculates vested tokens according to the linear vesting schedule

*Implements the core vesting calculation logic*


```solidity
function _vestingSchedule(uint256 totalAllocation, uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAllocation`|`uint256`|Total token allocation (current balance + already released)|
|`timestamp`|`uint64`|The timestamp to calculate vested amount for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens vested at the specified timestamp|


