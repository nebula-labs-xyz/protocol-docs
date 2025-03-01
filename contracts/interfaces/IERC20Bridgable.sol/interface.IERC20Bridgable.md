# IERC20Bridgable
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IERC20Bridgable.sol)

**Inherits:**
IERC20

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### burn

*Burns tokens.*


```solidity
function burn(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|of tokens to burn Emits a {Burn} event. Inherited from ERC20Burnable|


### bridgeMint

*BnM BridgeMint.*


```solidity
function bridgeMint(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|address|
|`amount`|`uint256`|of tokens Emits a [BridgeMint](/contracts/interfaces/IERC20Bridgable.sol/interface.IERC20Bridgable.md#bridgemint) event.|


## Events
### BridgeMint
*BridgeMint Event.*


```solidity
event BridgeMint(address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|rewarded|

