# ILendefiYieldToken
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/interfaces/ILendefiYieldToken.sol)

**Author:**
Lendefi Protocol Team

Interface for the LendefiYieldToken, an ERC20 token representing shares in Lendefi's lending protocol

*This interface defines the external functions of the LP token contract that uses 6 decimals to match USDC*

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### mint

Mints new tokens to a recipient

*Creates new token supply and assigns it to the recipient*

**Notes:**
- access: Restricted to PROTOCOL_ROLE (Lendefi protocol only)

- state-changes: 
- Increases recipient's token balance
- Increases total token supply


```solidity
function mint(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Address receiving the minted tokens|
|`amount`|`uint256`|Amount of tokens to mint|


### burn

Burns tokens from a holder

*Destroys token supply from the specified account*

**Notes:**
- access: Restricted to PROTOCOL_ROLE (Lendefi protocol only)

- state-changes: 
- Decreases account's token balance
- Decreases total token supply


```solidity
function burn(address from, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address whose tokens are being burned|
|`amount`|`uint256`|Amount of tokens to burn|


### balanceOf

Gets the token balance of an account

*Standard ERC20 balanceOf function*

**Note:**
state-changes: None, view-only function


```solidity
function balanceOf(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to query the balance of|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens owned by the account|


### totalSupply

Gets the total supply of tokens in circulation

*Standard ERC20 totalSupply function*

**Note:**
state-changes: None, view-only function


```solidity
function totalSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total amount of tokens currently in circulation|


### pause

Pauses all token transfers and minting

*Prevents all token movements in case of emergency*

**Notes:**
- access: Restricted to PAUSER_ROLE

- state-changes: Sets the paused state to true

- events: Emits a Paused event from PausableUpgradeable


```solidity
function pause() external;
```

### unpause

Unpauses token transfers and minting

*Restores normal token operation after emergency pause*

**Notes:**
- access: Restricted to PAUSER_ROLE

- state-changes: Sets the paused state to false

- events: Emits an Unpaused event from PausableUpgradeable


```solidity
function unpause() external;
```

### hasRole

Checks if an account has a specific role

*From AccessControlUpgradeable*

**Note:**
state-changes: None, view-only function


```solidity
function hasRole(bytes32 role, address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role identifier to check|
|`account`|`address`|The address to check for role assignment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account has the role, false otherwise|


### grantRole

Grants a role to an account

*From AccessControlUpgradeable*

**Notes:**
- access: Restricted to accounts with DEFAULT_ADMIN_ROLE

- state-changes: Updates role assignments for the account

- events: Emits a RoleGranted event from AccessControlUpgradeable


```solidity
function grantRole(bytes32 role, address account) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role identifier to grant|
|`account`|`address`|The address receiving the role|


### revokeRole

Revokes a role from an account

*From AccessControlUpgradeable*

**Notes:**
- access: Restricted to accounts with DEFAULT_ADMIN_ROLE

- state-changes: Updates role assignments for the account

- events: Emits a RoleRevoked event from AccessControlUpgradeable


```solidity
function revokeRole(bytes32 role, address account) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role identifier to revoke|
|`account`|`address`|The address losing the role|


### paused

Checks if the contract is currently paused

*From PausableUpgradeable*

**Note:**
state-changes: None, view-only function


```solidity
function paused() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the contract is paused, false otherwise|


### decimals

Returns the number of decimals used for token amounts

*Overrides the default ERC20 implementation which uses 18 decimals*

**Note:**
state-changes: None, view-only function


```solidity
function decimals() external pure returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The number of decimals (6 to match USDC)|


### version

Gets the current version of the contract

*Used to track implementation versions and verify successful upgrades*

**Note:**
state-changes: None, view-only function


```solidity
function version() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The contract version number|


### name

Returns the name of the token

*Standard ERC20 name function*

**Note:**
state-changes: None, view-only function


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The name of the token|


### symbol

Returns the symbol of the token

*Standard ERC20 symbol function*

**Note:**
state-changes: None, view-only function


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The symbol of the token|


### PAUSER_ROLE

Returns the role hash for PAUSER_ROLE

*Constant value defined in the contract*

**Note:**
state-changes: None, view-only function


```solidity
function PAUSER_ROLE() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The bytes32 role identifier for PAUSER_ROLE|


### PROTOCOL_ROLE

Returns the role hash for PROTOCOL_ROLE

*Constant value defined in the contract*

**Note:**
state-changes: None, view-only function


```solidity
function PROTOCOL_ROLE() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The bytes32 role identifier for PROTOCOL_ROLE|


### UPGRADER_ROLE

Returns the role hash for UPGRADER_ROLE

*Constant value defined in the contract*

**Note:**
state-changes: None, view-only function


```solidity
function UPGRADER_ROLE() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The bytes32 role identifier for UPGRADER_ROLE|


## Events
### Initialized
Emitted when the contract is initialized

**Note:**
access-control: This event is emitted once during initialization


```solidity
event Initialized(address indexed admin);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|Address of the initial admin|

### Upgrade
Emitted when the contract is upgraded

**Note:**
access-control: This event is emitted during authorized upgrades


```solidity
event Upgrade(address indexed upgrader, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`upgrader`|`address`|Address that triggered the upgrade|
|`implementation`|`address`|Address of the new implementation contract|

