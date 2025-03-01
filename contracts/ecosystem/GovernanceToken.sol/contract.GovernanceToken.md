# GovernanceToken
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/GovernanceToken.sol)

**Inherits:**
ERC20Upgradeable, ERC20BurnableUpgradeable, ERC20PausableUpgradeable, AccessControlUpgradeable, ERC20PermitUpgradeable, ERC20VotesUpgradeable, UUPSUpgradeable

Burnable contract that votes and has BnM-Bridge functionality

*Implements a secure and upgradeable DAO governance token*

**Notes:**
- security-contact: security@nebula-labs.xyz

- oz-upgrades: 


## State Variables
### INITIAL_SUPPLY
Token supply and distribution constants


```solidity
uint256 private constant INITIAL_SUPPLY = 50_000_000 ether;
```


### MAX_BRIDGE_AMOUNT

```solidity
uint256 private constant MAX_BRIDGE_AMOUNT = 20_000 ether;
```


### TREASURY_SHARE

```solidity
uint256 private constant TREASURY_SHARE = 56;
```


### ECOSYSTEM_SHARE

```solidity
uint256 private constant ECOSYSTEM_SHARE = 44;
```


### PAUSER_ROLE
*AccessControl Pauser Role*


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### BRIDGE_ROLE
*AccessControl Bridge Role*


```solidity
bytes32 internal constant BRIDGE_ROLE = keccak256("BRIDGE_ROLE");
```


### UPGRADER_ROLE
*AccessControl Upgrader Role*


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### initialSupply
*Initial token supply*


```solidity
uint256 public initialSupply;
```


### maxBridge
*max bridge passthrough amount*


```solidity
uint256 public maxBridge;
```


### version
*number of UUPS upgrades*


```solidity
uint32 public version;
```


### tge
*tge initialized variable*


```solidity
uint32 public tge;
```


### __gap

```solidity
uint256[50] private __gap;
```


## Functions
### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### receive


```solidity
receive() external payable;
```

### initializeUUPS

Sets up the initial state of the contract, including roles and token supplies.

*Initializes the UUPS contract.*

**Notes:**
- requires: The guardian address must not be zero.

- events-emits: [Initialized](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#initialized) event.

- throws: ZeroAddress if the guardian address is zero.


```solidity
function initializeUUPS(address guardian) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|The address of the guardian (admin).|


### initializeTGE

Sets up the initial token distribution between the ecosystem and treasury contracts.

*Initializes the Token Generation Event (TGE).*

**Notes:**
- requires: The ecosystem and treasury addresses must not be zero.

- requires: TGE must not be already initialized.

- events-emits: [TGE](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#tge) event.

- throws: ZeroAddress if any address is zero.

- throws: TGEAlreadyInitialized if TGE was already initialized.


```solidity
function initializeTGE(address ecosystem, address treasury) external onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ecosystem`|`address`|The address of the ecosystem contract.|
|`treasury`|`address`|The address of the treasury contract.|


### pause

This function can be called by an account with the PAUSER_ROLE to pause the contract.

*Pauses all token transfers and operations.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Paused} event from PausableUpgradeable


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

This function can be called by an account with the PAUSER_ROLE to unpause the contract.

*Unpauses all token transfers and operations.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Unpaused} event from PausableUpgradeable


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### bridgeMint

Can only be called by the official Bridge contract

*Mints tokens for cross-chain bridge transfers*

**Notes:**
- requires-role: BRIDGE_ROLE

- requires: Total supply must not exceed initialSupply

- requires: to address must not be zero

- requires: amount must not be zero

- requires: amount must not exceed maxBridge limit

- events-emits: [BridgeMint](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#bridgemint) event

- throws: ZeroAddress if recipient address is zero

- throws: ZeroAmount if amount is zero

- throws: BridgeAmountExceeded if amount exceeds maxBridge

- throws: MaxSupplyExceeded if the mint would exceed initialSupply


```solidity
function bridgeMint(address to, uint256 amount) external whenNotPaused onlyRole(BRIDGE_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Address receiving the tokens|
|`amount`|`uint256`|Amount to mint|


### updateMaxBridgeAmount

Only callable by admin role

*Updates the maximum allowed bridge amount per transaction*

**Notes:**
- requires-role: DEFAULT_ADMIN_ROLE

- requires: New amount must be greater than zero

- events-emits: [MaxBridgeUpdated](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#maxbridgeupdated) event

- throws: ZeroAmount if newMaxBridge is zero


```solidity
function updateMaxBridgeAmount(uint256 newMaxBridge) external onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxBridge`|`uint256`|New maximum bridge amount|


### nonces


```solidity
function nonces(address owner) public view override(ERC20PermitUpgradeable, NoncesUpgradeable) returns (uint256);
```

### _update


```solidity
function _update(address from, address to, uint256 value)
    internal
    override(ERC20Upgradeable, ERC20PausableUpgradeable, ERC20VotesUpgradeable);
```

### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

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

### TGE
*event emitted at TGE*


```solidity
event TGE(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|token amount|

### BridgeMint
*event emitted when bridge triggers a mint*


```solidity
event BridgeMint(address indexed src, address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender|
|`to`|`address`|beneficiary address|
|`amount`|`uint256`|token amount|

### MaxBridgeUpdated
*Emitted when the maximum bridge amount is updated*


```solidity
event MaxBridgeUpdated(address indexed admin, uint256 oldMaxBridge, uint256 newMaxBridge);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|The address that updated the value|
|`oldMaxBridge`|`uint256`|Previous maximum bridge amount|
|`newMaxBridge`|`uint256`|New maximum bridge amount|

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

## Errors
### ZeroAddress
*Error thrown when an address parameter is zero*


```solidity
error ZeroAddress();
```

### ZeroAmount
*Error thrown when an amount parameter is zero*


```solidity
error ZeroAmount();
```

### MaxSupplyExceeded
*Error thrown when a mint would exceed the max supply*


```solidity
error MaxSupplyExceeded(uint256 requested, uint256 maxAllowed);
```

### BridgeAmountExceeded
*Error thrown when bridge amount exceeds allowed limit*


```solidity
error BridgeAmountExceeded(uint256 requested, uint256 maxAllowed);
```

### TGEAlreadyInitialized
*Error thrown when TGE is already initialized*


```solidity
error TGEAlreadyInitialized();
```

### InvalidAddress
*Error thrown when addresses don't match expected values*


```solidity
error InvalidAddress(address provided, string reason);
```

### ValidationFailed
*Error thrown for general validation failures*


```solidity
error ValidationFailed(string reason);
```

