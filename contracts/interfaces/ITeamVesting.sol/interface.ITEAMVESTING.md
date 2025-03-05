# ITEAMVESTING
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/ITeamVesting.sol)

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### start

*Getter for the start timestamp.*


```solidity
function start() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|start timestamp|


### duration

*Getter for the vesting duration.*


```solidity
function duration() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|duration seconds|


### end

*Getter for the end timestamp.*


```solidity
function end() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|end timestamp|


### released

*Getter for the amount of token already released*


```solidity
function released() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of tokens released so far|


### releasable

*Getter for the amount of releasable `token` ERC20 tokens.*


```solidity
function releasable() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested tokens|


### release

*Release the tokens that have already vested.
Emits a [ERC20Released](/contracts/interfaces/ITeamVesting.sol/interface.ITEAMVESTING.md#erc20released) event.*


```solidity
function release() external;
```

### cancelContract

*Release the tokens that have already vested.
Emits a [ERC20Released](/contracts/interfaces/ITeamVesting.sol/interface.ITEAMVESTING.md#erc20released) event.
Refund the remainder to the timelock*


```solidity
function cancelContract() external;
```

## Events
### Cancelled
*Cancelled Event*


```solidity
event Cancelled(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|that was refunded to the treasury|

### ERC20Released
*ERC20Released Event*


```solidity
event ERC20Released(address indexed token, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|
|`amount`|`uint256`|released|

## Errors
### CustomError
*Custom Error.*


```solidity
error CustomError(string msg);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`msg`|`string`|error desription|

