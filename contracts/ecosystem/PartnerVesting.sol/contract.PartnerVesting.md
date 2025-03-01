# PartnerVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/PartnerVesting.sol)

**Inherits:**
[IPARTNERVESTING](/contracts/interfaces/IPartnerVesting.sol/interface.IPARTNERVESTING.md), Context, Ownable2Step, ReentrancyGuard


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


### _creator
*creator address (Ecosystem)*


```solidity
address public immutable _creator;
```


### _tokensReleased
*amount of tokens released*


```solidity
uint256 private _tokensReleased;
```


## Functions
### onlyAuthorized

*Throws if called by any account other than the timelock or creator.*


```solidity
modifier onlyAuthorized();
```

### constructor

*Sets the owner to partner address (beneficiary), the start timestamp and the
vesting duration of the vesting contract.*


```solidity
constructor(address token, address timelock, address beneficiary, uint64 startTimestamp, uint64 durationSeconds)
    Ownable(beneficiary);
```

### cancelContract

*Allows the DAO to cancel the contract in case the partnership ends.
First releases any vested tokens to the beneficiary, then returns
the remaining unvested tokens to the creator (Ecosystem contract).*


```solidity
function cancelContract() external nonReentrant onlyAuthorized returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|remainder The amount of tokens returned to the creator Can only be called by the creator (Ecosystem contract).|


### _checkAuthorized

*Throws if the sender is not authorized to cancel.*


```solidity
function _checkAuthorized() internal view virtual;
```

### release


```solidity
function release() public virtual;
```

### start


```solidity
function start() public view virtual returns (uint256);
```

### duration


```solidity
function duration() public view virtual returns (uint256);
```

### end


```solidity
function end() public view virtual returns (uint256);
```

### released


```solidity
function released() public view virtual returns (uint256);
```

### releasable


```solidity
function releasable() public view virtual returns (uint256);
```

### vestedAmount


```solidity
function vestedAmount(uint64 timestamp) internal view virtual returns (uint256);
```

### _vestingSchedule


```solidity
function _vestingSchedule(uint256 totalAllocation, uint64 timestamp) internal view virtual returns (uint256);
```

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

