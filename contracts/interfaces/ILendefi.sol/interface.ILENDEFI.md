# ILENDEFI
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/ILendefi.sol)

**Inherits:**
IERC20, IERC20Metadata

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### initializeUUPS

*UUPS deploy proxy initializer.*


```solidity
function initializeUUPS(address admin) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|address|


### initializeTGE

*Performs TGE.*


```solidity
function initializeTGE(address ecosystem, address treasury) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ecosystem`|`address`|contract address|
|`treasury`|`address`|contract address Emits a [TGE](/contracts/interfaces/ILendefi.sol/interface.ILENDEFI.md#tge) event.|


### pause

*ERC20 pause contract.*


```solidity
function pause() external;
```

### unpause

*ERC20 unpause contract.*


```solidity
function unpause() external;
```

### burn

*ERC20 Burn.*


```solidity
function burn(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|of tokens to burn Emits a {Burn} event.|


### burnFrom

*ERC20 burn from.*


```solidity
function burnFrom(address account, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|address|
|`amount`|`uint256`|of tokens to burn from Emits a {Burn} event.|


### bridgeMint

*Facilitates Bridge BnM functionality.*


```solidity
function bridgeMint(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|to bridge|


### initialSupply

*Getter for the Initial supply.*


```solidity
function initialSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|initial supply at TGE|


### maxBridge

*Getter for the maximum amount alowed to pass through bridge in a single transaction.*


```solidity
function maxBridge() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|maximum bridge transaction size|


### version

*Getter for the UUPS version, incremented with every upgrade.*


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|version number (1,2,3)|


## Events
### TGE
*TGE Event.*


```solidity
event TGE(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|of initial supply|

### BridgeMint
*BridgeMint Event.*


```solidity
event BridgeMint(address indexed src, address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|to bridge|

### Upgrade
*event emitted on UUPS upgrades*


```solidity
event Upgrade(address indexed src, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|
|`implementation`|`address`|new implementation address|

