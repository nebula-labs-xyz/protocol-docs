# LendefiYieldToken
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/7882024792b94909a5d6c51ec494855406aaf294/contracts/lender/LendefiYieldToken.sol)

**Inherits:**
Initializable, ERC20PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

**Author:**
alexei@nebula-labs(dot)xyz

LP token representing shares in the Lendefi lending protocol's liquidity pool, using 6 decimals to match USDC

*This contract implements an ERC20 token that represents a user's share of the Lendefi protocol's
lending pool. It's designed to be controlled exclusively by the main Lendefi protocol.
The token uses 6 decimals to maintain consistency with USDC.*

**Notes:**
- security-contact: security@nebula-labs.xyz

- oz-upgrades: 


## State Variables
### PAUSER_ROLE
Role for pausing and unpausing token transfers in emergency situations


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### PROTOCOL_ROLE
Role for the main Lendefi protocol to control token minting and burning


```solidity
bytes32 internal constant PROTOCOL_ROLE = keccak256("PROTOCOL_ROLE");
```


### UPGRADER_ROLE
Role for authorizing contract upgrades


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### version
Current version of the contract, incremented on each upgrade

*Used to track implementation versions and verify successful upgrades*


```solidity
uint8 public version;
```


### __gap
*Reserved storage slots for future upgrades to maintain storage layout compatibility*


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

### initialize

Initializes the token with name, symbol, and key roles

*Sets up token details and access control roles*

**Notes:**
- oz-upgrades-unsafe: initializer is used instead of constructor for proxy pattern

- access: Only callable once during initialization

- validation-rules: 
- All addresses must be non-zero
- Sets initial version to 1


```solidity
function initialize(address protocol, address guardian) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`protocol`|`address`|Address of the Lendefi protocol contract (receives PROTOCOL_ROLE)|
|`guardian`|`address`|Address with pausing capability (receives PAUSER_ROLE)|


### mint

Mints new tokens to a recipient

*Creates new token supply and assigns it to the recipient*

**Notes:**
- access: Restricted to PROTOCOL_ROLE (Lendefi protocol only)

- security: Non-reentrant pattern prevents potential reentrancy attacks

- state-changes: 
- Increases recipient's token balance
- Increases total token supply


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

*Destroys token supply from the specified account*

**Notes:**
- access: Restricted to PROTOCOL_ROLE (Lendefi protocol only)

- security: Non-reentrant pattern prevents potential reentrancy attacks

- state-changes: 
- Decreases account's token balance
- Decreases total token supply


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

*Prevents all token movements in case of emergency*

**Notes:**
- access: Restricted to PAUSER_ROLE

- state-changes: Sets the paused state to true

- events: Emits a Paused event from PausableUpgradeable


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses token transfers and minting

*Restores normal token operation after emergency pause*

**Notes:**
- access: Restricted to PAUSER_ROLE

- state-changes: Sets the paused state to false

- events: Emits an Unpaused event from PausableUpgradeable


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### decimals

Returns the number of decimals used for token amounts

*Overrides the default ERC20 implementation which uses 18 decimals*

**Note:**
state-changes: None, view-only function


```solidity
function decimals() public pure override returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The number of decimals (6 to match USDC)|


### _update

*Override to enforce pause state during transfers*

**Notes:**
- state-changes: None, modifies underlying token transfer behavior

- validation-rules: Reverts if contract is paused


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

*Authorizes an upgrade to a new implementation*

**Notes:**
- access: Restricted to UPGRADER_ROLE

- state-changes: 
- Increments the contract version
- Upgrades implementation contract (via UUPSUpgradeable)

- events: Emits Upgrade event with upgrader and new implementation addresses


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

