# GovernanceToken
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/GovernanceToken.sol)

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


### DEFAULT_MAX_BRIDGE_AMOUNT

```solidity
uint256 private constant DEFAULT_MAX_BRIDGE_AMOUNT = 5_000 ether;
```


### TREASURY_SHARE

```solidity
uint256 private constant TREASURY_SHARE = 27_400_000 ether;
```


### ECOSYSTEM_SHARE

```solidity
uint256 private constant ECOSYSTEM_SHARE = 22_000_000 ether;
```


### DEPLOYER_SHARE

```solidity
uint256 private constant DEPLOYER_SHARE = 600_000 ether;
```


### UPGRADE_TIMELOCK_DURATION
Upgrade timelock duration (in seconds)


```solidity
uint256 private constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### TGE_ROLE
*AccessControl Role Constants*


```solidity
bytes32 internal constant TGE_ROLE = keccak256("TGE_ROLE");
```


### PAUSER_ROLE

```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### BRIDGE_ROLE

```solidity
bytes32 internal constant BRIDGE_ROLE = keccak256("BRIDGE_ROLE");
```


### UPGRADER_ROLE

```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### MANAGER_ROLE

```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
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


### pendingUpgrade

```solidity
UpgradeRequest public pendingUpgrade;
```


### __gap
*Storage gap for future upgrades*


```solidity
uint256[48] private __gap;
```


## Functions
### nonZeroAmount

*Modifier to check for non-zero amounts*


```solidity
modifier nonZeroAmount(uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount to validate|


### nonZeroAddress

*Modifier to check for non-zero addresses*


```solidity
modifier nonZeroAddress(address addr);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|The address to validate|


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
- requires: The addresses must not be zero.

- events-emits: [Initialized](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#initialized) event.

- throws: ZeroAddress if any address is zero.


```solidity
function initializeUUPS(address guardian, address timelock) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|The address of the guardian (admin).|
|`timelock`|`address`|The address of the timelock controller.|


### setBridgeAddress

*Sets the bridge address with BRIDGE_ROLE*

**Notes:**
- requires-role: MANAGER_ROLE

- throws: ZeroAddress if bridgeAddress is zero


```solidity
function setBridgeAddress(address bridgeAddress) external onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`bridgeAddress`|`address`|The address of the bridge contract|


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
function initializeTGE(address ecosystem, address treasury) external onlyRole(TGE_ROLE);
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

- requires: amount must not be zero or exceed maxBridge limit

- events-emits: [BridgeMint](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#bridgemint) event

- throws: ZeroAddress if recipient address is zero

- throws: ZeroAmount if amount is zero

- throws: BridgeAmountExceeded if amount exceeds maxBridge

- throws: MaxSupplyExceeded if the mint would exceed initialSupply


```solidity
function bridgeMint(address to, uint256 amount)
    external
    nonZeroAddress(to)
    nonZeroAmount(amount)
    whenNotPaused
    onlyRole(BRIDGE_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Address receiving the tokens|
|`amount`|`uint256`|Amount to mint|


### updateMaxBridgeAmount

Only callable by manager role

*Updates the maximum allowed bridge amount per transaction*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: New amount must be greater than zero and less than 1% of total supply

- events-emits: [MaxBridgeUpdated](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#maxbridgeupdated) event

- throws: ZeroAmount if newMaxBridge is zero

- throws: ValidationFailed if bridge amount is too high


```solidity
function updateMaxBridgeAmount(uint256 newMaxBridge) external nonZeroAmount(newMaxBridge) onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxBridge`|`uint256`|New maximum bridge amount|


### scheduleUpgrade

Only callable by an address with UPGRADER_ROLE

*Schedules an upgrade to a new implementation*

**Notes:**
- requires-role: UPGRADER_ROLE

- events-emits: [UpgradeScheduled](/contracts/ecosystem/GovernanceToken.sol/contract.GovernanceToken.md#upgradescheduled) event

- throws: ZeroAddress if newImplementation is zero


```solidity
function scheduleUpgrade(address newImplementation)
    external
    nonZeroAddress(newImplementation)
    onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


### cancelUpgrade

Cancels a previously scheduled upgrade

*Only callable by addresses with UPGRADER_ROLE*


```solidity
function cancelUpgrade() external onlyRole(UPGRADER_ROLE);
```

### upgradeTimelockRemaining

*Returns the remaining time before a scheduled upgrade can be executed*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The time remaining in seconds, or 0 if no upgrade is scheduled or timelock has passed|


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

*Internal authorization for contract upgrades with timelock enforcement*

**Notes:**
- requires-role: UPGRADER_ROLE (enforced by the function modifier)

- requires: Upgrade must be scheduled and timelock must be expired

- throws: UpgradeNotScheduled if no upgrade was scheduled

- throws: UpgradeTimelockActive if timelock period hasn't passed


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


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

### BridgeRoleAssigned
*Emitted when a bridge role is assigned*


```solidity
event BridgeRoleAssigned(address indexed admin, address indexed bridgeAddress);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|The admin who set the role|
|`bridgeAddress`|`address`|The bridge address receiving the role|

### UpgradeScheduled
*Emitted when an upgrade is scheduled*


```solidity
event UpgradeScheduled(
    address indexed sender, address indexed implementation, uint64 scheduledTime, uint64 effectiveTime
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sender`|`address`|The address that scheduled the upgrade|
|`implementation`|`address`|The new implementation address|
|`scheduledTime`|`uint64`|The time when the upgrade was scheduled|
|`effectiveTime`|`uint64`|The time when the upgrade can be executed|

### UpgradeCancelled
Emitted when a scheduled upgrade is cancelled


```solidity
event UpgradeCancelled(address indexed canceller, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`canceller`|`address`|The address that cancelled the upgrade|
|`implementation`|`address`|The implementation address that was cancelled|

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

### UpgradeTimelockActive
*Error thrown when trying to execute an upgrade too soon*


```solidity
error UpgradeTimelockActive(uint256 remainingTime);
```

### UpgradeNotScheduled
*Error thrown when trying to execute an upgrade that wasn't scheduled*


```solidity
error UpgradeNotScheduled();
```

### ImplementationMismatch
*Error thrown when trying to execute an upgrade with wrong implementation*


```solidity
error ImplementationMismatch(address expected, address provided);
```

### ValidationFailed
*Error thrown for general validation failures*


```solidity
error ValidationFailed(string reason);
```

## Structs
### UpgradeRequest
*Upgrade timelock storage*


```solidity
struct UpgradeRequest {
    address implementation;
    uint64 scheduledTime;
    bool exists;
}
```

