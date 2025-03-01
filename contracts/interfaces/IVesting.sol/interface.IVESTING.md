# IVESTING
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IVesting.sol)

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### start

*Getter for the start timestamp*


```solidity
function start() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|start timestamp|


### duration

*Getter for the duration period*


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

*Amount of token already released*


```solidity
function released() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount released|


### releasable

*Getter for the amount of releasable ERC20 tokens.*


```solidity
function releasable() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|available amount|


### release

*Release the tokens that have already vested.
Emits a [ERC20Released](/contracts/interfaces/IVesting.sol/interface.IVESTING.md#erc20released) event.*


```solidity
function release() external;
```

## Events
### Cancelled
event emmited when contract cancelled


```solidity
event Cancelled(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|tokens returned|

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

