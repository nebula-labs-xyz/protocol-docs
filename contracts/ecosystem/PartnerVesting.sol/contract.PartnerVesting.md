# PartnerVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-dao/blob/7c7b1b2bfafff849091a54ac0cdd4fabebe1032e/contracts/ecosystem/PartnerVesting.sol)


## Overview

The PartnerVesting contract implements a linear token vesting mechanism designed for partner allocations within the Lendefi DAO ecosystem. It provides controlled token distribution with cancellation capabilities, allowing the parent Ecosystem contract to reclaim unvested tokens if a partnership ends.

## Architecture & Design

### Design Pattern
The contract employs a hub-and-spoke architecture where:
- The Ecosystem contract acts as a central hub, creating and managing individual vesting instances
- Each PartnerVesting contract serves as a spoke, handling vesting for a single partner

### Inheritance
- **Ownable2Step**: Provides two-step ownership transfer for enhanced security
- **ReentrancyGuard**: Protects against reentrancy attacks
- **Context**: Provides execution context information

### Storage Design
The contract optimizes gas usage with:
- **Immutable Variables**: All core parameters are immutable, saving gas and preventing modifications
- **Minimal State**: Only tracks released tokens as mutable state
- **Type-Appropriate Storage**: Uses uint64 for timestamps to optimize storage

## Token Vesting Mechanism

### Implementation Approach
The vesting follows a simple linear model:
- Tokens vest continuously from start time to end time
- No cliff period (vesting begins immediately at start)
- Vested amount calculation: `totalAllocation * (currentTime - startTime) / duration`

### Vesting Calculation
The implementation correctly handles edge cases:
- Before start: 0 tokens vested
- After end: 100% tokens vested
- During vesting: Linear proportion based on elapsed time
- Already released tokens are properly accounted for




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

