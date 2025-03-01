# ITREASURY
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/ITreasury.sol)

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### version

*UUPS version incremented every upgrade*


```solidity
function version() external returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|version number|


### pause

*Pause contract*


```solidity
function pause() external;
```

### unpause

*Unpause contract.*


```solidity
function unpause() external;
```

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
|`<none>`|`uint256`|end timnestamp|


### released

*Getter for the amount of eth already released*


```solidity
function released() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of ETH released so far|


### released

*Getter for the amount of token already released*


```solidity
function released(address token) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of tokens released so far|


### releasable

*Getter for the amount of releasable eth.*


```solidity
function releasable() external returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested ETH|


### releasable

*Getter for the amount of vested `ERC20` tokens.*


```solidity
function releasable(address token) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested tokens|


### release

*Release the native token (ether) that have already vested.*


```solidity
function release(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|amount of ETH to transfer Emits a [EtherReleased](/contracts/interfaces/ITreasury.sol/interface.ITREASURY.md#etherreleased) event.|


### release

*Release the tokens that have already vested.*


```solidity
function release(address token, address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|token address|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|amount of tokens to transfer Emits a [ERC20Released](/contracts/interfaces/ITreasury.sol/interface.ITREASURY.md#erc20released) event.|


## Events
### Initialized
*Initialized Event.*


```solidity
event Initialized(address indexed src);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|

### Received
*Received Event*


```solidity
event Received(address indexed src, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|
|`amount`|`uint256`|amount|

### Upgrade
*Upgrade Event.*


```solidity
event Upgrade(address indexed src, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|
|`implementation`|`address`|address|

### EtherReleased
*EtherReleased Event*


```solidity
event EtherReleased(address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|amount of ETH to transfer|

### ERC20Released
*ERC20Released Event*


```solidity
event ERC20Released(address indexed token, address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|
|`to`|`address`|address|
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

