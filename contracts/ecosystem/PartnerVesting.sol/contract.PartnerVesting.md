# PartnerVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/PartnerVesting.sol)

**Inherits:**
[IPARTNERVESTING](/contracts/interfaces/IPartnerVesting.sol/interface.IPARTNERVESTING.md), Context, Ownable2Step, ReentrancyGuard

**Author:**
alexei@nebula-labs(dot)xyz

Manages linear token vesting for Lendefi partners

*Implements a time-based linear vesting schedule with partner control and DAO cancellation capability*

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


### _creator
*Address that created this contract (Ecosystem contract)*


```solidity
address public immutable _creator;
```


### _tokensReleased
*Running total of tokens that have been released*


```solidity
uint256 private _tokensReleased;
```


## Functions
### onlyAuthorized

Restricts function access to the contract creator only

*Used for cancellation functionality*


```solidity
modifier onlyAuthorized();
```

### constructor

Creates a new vesting contract for a partner

*Sets the beneficiary as the owner, initializes immutable vesting parameters*


```solidity
constructor(address token, address beneficiary, uint64 startTimestamp, uint64 durationSeconds) Ownable(beneficiary);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ERC20 token to be vested|
|`beneficiary`|`address`|Address that will receive the vested tokens|
|`startTimestamp`|`uint64`|UNIX timestamp when vesting begins|
|`durationSeconds`|`uint64`|Duration of vesting period in seconds|


### cancelContract

Cancels the vesting contract, releasing vested tokens and returning unvested tokens

*First releases any vested tokens to the beneficiary, then returns remaining tokens to creator*


```solidity
function cancelContract() external nonReentrant onlyAuthorized returns (uint256 remainder);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remainder`|`uint256`|The amount of tokens returned to the creator|


### release

Releases vested tokens to the beneficiary

*Can be called by anyone but tokens are always sent to the owner (beneficiary)*


```solidity
function release() public virtual nonReentrant onlyOwner;
```

### start

Returns the timestamp when vesting starts


```solidity
function start() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The start timestamp of the vesting period|


### duration

Returns the duration of the vesting period


```solidity
function duration() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The duration in seconds of the vesting period|


### end

Returns the timestamp when vesting ends


```solidity
function end() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The end timestamp of the vesting period|


### released

Returns the amount of tokens already released


```solidity
function released() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens that have been released so far|


### releasable

Calculates the amount of tokens that can be released now


```solidity
function releasable() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens currently available to be released|


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


### _checkAuthorized

Verifies the caller is authorized to perform creator-only actions

*Throws Unauthorized error if caller is not the creator*


```solidity
function _checkAuthorized() internal view virtual;
```

