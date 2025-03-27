# InvestorVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-dao/blob/07f5cb7369219dbffd648091ffbddb6d70a0157c/contracts/ecosystem/InvestorVesting.sol)

## Overview

The InvestorVesting contract implements a linear token vesting schedule for investors participating in the Lendefi DAO ecosystem. Derived from OpenZeppelin's VestingWallet, this contract provides a clean, gas-efficient mechanism for token distribution with time-based release controls.

## Architecture Analysis

### Contract Structure
- **Inheritance Model**: Implements IVESTING interface and inherits from Context and Ownable2Step
- **Storage Design**: Uses immutable variables for core parameters (token, start, duration)
- **Release Mechanism**: On-demand release pattern rather than automatic distribution
- **Ownership Model**: Two-step ownership transfer for improved security

### Vesting Design
- **Schedule Type**: Pure linear vesting with no cliff period
- **Calculation Method**: Uses time proportion formula: `totalAllocation * (currentTime - startTime) / duration`
- **Flexibility**: Relies on manual release calls rather than automatic distribution

## Technical Assessment

### Strengths

1. **Gas Efficiency**:
   - Immutable variables reduce storage operations
   - On-demand release pattern conserves gas
   - Efficient vesting calculation with minimal operations

2. **Security Features**:
   - Two-step ownership transfers prevent accidental transfers
   - SafeERC20 usage protects against non-standard token implementations
   - Input validation for zero addresses
   - Precise timestamp handling with SafeCast

3. **Clean Design**:
   - Single-responsibility principle followed throughout
   - Clear separation between calculation and execution
   - Minimal storage footprint

4. **Auditability**:
   - Simple, straightforward implementation
   - Well-documented functions with comments
   - Clear naming conventions

### Potential Concerns

1. **Limited Flexibility**:
   - No cliff period option for investors
   - No partial release functionality
   - No way to adjust vesting parameters after deployment

2. **Transfer Restrictions**:
   - Beneficiary (owner) cannot transfer partial rights to another address
   - No streaming of tokens (only discrete releases)

3. **Technical Considerations**:
   - Mapping for `_erc20Released` seems unnecessary since only one token is used
   - No recovery mechanism for accidentally sent tokens
   - No events for constructor initialization

4. **Edge Cases**:
   - No handling for token rebasing or fee-on-transfer tokens
   - Function may fail if token implements hooks that consume excessive gas

## Code Quality Assessment

- **Documentation**: Good NatSpec documentation for functions
- **Input Validation**: Basic zero address check in constructor
- **Event Emissions**: Appropriate event for token release
- **Variable Naming**: Clear and descriptive
- **Code Organization**: Logical grouping of functions
- **Inheritance**: Appropriate use of OpenZeppelin contracts


**Inherits:**
[IVESTING](/contracts/interfaces/IVesting.sol/interface.IVESTING.md), Context, Ownable2Step

Investor Vesting contract

Offers flexible withdrawal schedule (gas efficient)

*Implements linear vesting schedule for investors*

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


### _erc20Released
*amount of tokens released*


```solidity
mapping(address token => uint256 amount) private _erc20Released;
```


## Functions
### constructor

*Sets the owner to beneficiary address, the start timestamp and the
vesting duration of the vesting contract.*


```solidity
constructor(address token, address beneficiary, uint64 startTimestamp, uint64 durationSeconds) Ownable(beneficiary);
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
|`<none>`|`uint256`|end timnestamp|


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

*Getter for the amount of vested `ERC20` tokens.*


```solidity
function releasable() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested tokens|


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

*Virtual implementation of the vesting formula. This returns the amount vested, as a function of time, for
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


