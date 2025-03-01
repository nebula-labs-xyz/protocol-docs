# InvestorVesting
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/InvestorVesting.sol)

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


