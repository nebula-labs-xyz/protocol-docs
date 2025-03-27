# LendefiYieldToken
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/lender/LendefiYieldToken.sol)

**Inherits:**
Initializable, ERC20PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

**Author:**
alexei@nebula-labs.xyz

LP token representing shares in the Lendefi lending protocol's liquidity pool

*This ERC20 token uses 6 decimals to match USDC and represents the lender's share in the
Lendefi protocol's lending pool. Only the main protocol can mint and burn tokens.*

**Note:**
security-contact: security@nebula-labs.xyz


## State Variables
### PAUSER_ROLE
Role for pausing and unpausing token transfers in emergency situations


```solidity
bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### PROTOCOL_ROLE
Role for the main Lendefi protocol to control token minting and burning


```solidity
bytes32 public constant PROTOCOL_ROLE = keccak256("PROTOCOL_ROLE");
```


### UPGRADER_ROLE
Role for authorizing contract upgrades


```solidity
bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### UPGRADE_TIMELOCK_DURATION
Duration of the timelock for upgrade operations (3 days)

*Provides time for users to review and react to scheduled upgrades*


```solidity
uint256 public constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### version
Current version of the contract, incremented on each upgrade


```solidity
uint8 public version;
```


### pendingUpgrade
Information about the currently pending upgrade


```solidity
UpgradeRequest public pendingUpgrade;
```


### __gap
*Reserved storage slots for future upgrades*


```solidity
uint256[25] private __gap;
```


## Functions
### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

Initializes the token with name, symbol, and key roles

*Sets up token details and access control roles*


```solidity
function initialize(address protocol, address timelock, address multisig) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`protocol`|`address`|Address of the Lendefi protocol contract (receives PROTOCOL_ROLE)|
|`timelock`|`address`|Address of the timelock contract (receives DEFAULT_ADMIN_ROLE)|
|`multisig`|`address`|Address with upgrade capability (receives UPGRADER_ROLE)|


### mint

Mints new tokens to a recipient

*Only callable by the protocol contract when not paused*


```solidity
function mint(address to, uint256 amount) external onlyRole(PROTOCOL_ROLE) whenNotPaused nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Address receiving the minted tokens|
|`amount`|`uint256`|Amount of tokens to mint|


### burn

Burns tokens from a holder

*Only callable by the protocol contract when not paused*


```solidity
function burn(address from, uint256 amount) external onlyRole(PROTOCOL_ROLE) whenNotPaused nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address whose tokens are being burned|
|`amount`|`uint256`|Amount of tokens to burn|


### pause

Pauses all token transfers and minting

*Only callable by addresses with PAUSER_ROLE*


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses token transfers and minting

*Only callable by addresses with PAUSER_ROLE*


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### scheduleUpgrade

Schedules an upgrade to a new implementation with timelock

*Only callable by addresses with UPGRADER_ROLE*


```solidity
function scheduleUpgrade(address newImplementation) external onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


### cancelUpgrade

Cancels a previously scheduled upgrade

*Only callable by addresses with UPGRADER_ROLE*


```solidity
function cancelUpgrade() external onlyRole(UPGRADER_ROLE);
```

### upgradeTimelockRemaining

Returns the remaining time before a scheduled upgrade can be executed

*Returns 0 if no upgrade is scheduled or if the timelock has expired*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|timeRemaining The time remaining in seconds|


### decimals

Returns the number of decimals used for token amounts

*Overrides the default ERC20 implementation to use 6 decimals to match USDC*


```solidity
function decimals() public pure override returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The number of decimals (6)|


### _update

Override to enforce pause state during transfers

*Reverts if the contract is paused*


```solidity
function _update(address from, address to, uint256 value) internal override whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address sending tokens|
|`to`|`address`|Address receiving tokens|
|`value`|`uint256`|Token amount|


### _authorizeUpgrade

Authorizes an upgrade to a new implementation

*Implements the upgrade verification and authorization logic*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of new implementation contract|


## Events
### Initialized
Emitted when the contract is initialized


```solidity
event Initialized(address indexed admin);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|Address of the initial admin|

### Upgrade
Emitted when the contract is upgraded


```solidity
event Upgrade(address indexed upgrader, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`upgrader`|`address`|Address that triggered the upgrade|
|`implementation`|`address`|Address of the new implementation contract|

### UpgradeScheduled
Emitted when an upgrade is scheduled


```solidity
event UpgradeScheduled(
    address indexed scheduler, address indexed implementation, uint64 scheduledTime, uint64 effectiveTime
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`scheduler`|`address`|The address scheduling the upgrade|
|`implementation`|`address`|The new implementation contract address|
|`scheduledTime`|`uint64`|The timestamp when the upgrade was scheduled|
|`effectiveTime`|`uint64`|The timestamp when the upgrade can be executed|

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

## Errors
### ZeroAddressNotAllowed
Thrown when attempting to set a critical address to the zero address


```solidity
error ZeroAddressNotAllowed();
```

### UpgradeTimelockActive
Thrown when attempting to execute an upgrade before timelock expires


```solidity
error UpgradeTimelockActive(uint256 timeRemaining);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timeRemaining`|`uint256`|The time remaining until the upgrade can be executed|

### UpgradeNotScheduled
Thrown when attempting to execute an upgrade that wasn't scheduled


```solidity
error UpgradeNotScheduled();
```

### ImplementationMismatch
Thrown when implementation address doesn't match scheduled upgrade


```solidity
error ImplementationMismatch(address scheduledImpl, address attemptedImpl);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`scheduledImpl`|`address`|The address that was scheduled for upgrade|
|`attemptedImpl`|`address`|The address that was attempted to be used|

## Structs
### UpgradeRequest
Structure to store pending upgrade details


```solidity
struct UpgradeRequest {
    address implementation;
    uint64 scheduledTime;
    bool exists;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`implementation`|`address`|Address of the new implementation contract|
|`scheduledTime`|`uint64`|Timestamp when the upgrade was scheduled|
|`exists`|`bool`|Boolean flag indicating if an upgrade is currently scheduled|

