# Treasury
[Git Source](https://github.com/nebula-labs-xyz/lendefi-dao/blob/07f5cb7369219dbffd648091ffbddb6d70a0157c/contracts/ecosystem/Treasury.sol)

# Security Review: Lendefi DAO Treasury Contract

## 1. Overview

The Treasury contract implements a secure, upgradeable vesting mechanism for managing and distributing both ETH and ERC20 tokens to beneficiaries according to a linear vesting schedule. The contract serves as the financial backbone for the Lendefi DAO, controlling the release of funds over time with appropriate governance controls.

## 2. Contract Architecture

The contract inherits from multiple OpenZeppelin upgradeable contracts to provide a robust foundation:
- **AccessControlUpgradeable**: Role-based permission system
- **PausableUpgradeable**: Emergency pause functionality
- **UUPSUpgradeable**: Secure upgrade pattern 
- **ReentrancyGuardUpgradeable**: Protection against reentrancy attacks

Key components include:
1. **Vesting System**: Linear vesting over 3 years (1095 days)
2. **Role-Based Access Control**: Different roles for different responsibilities
3. **Pausable Functionality**: Emergency pause/unpause capabilities
4. **Upgrade Mechanism**: Controlled contract upgrades with version tracking

## 3. Role-Based Access Control

The contract implements four main roles:
- **DEFAULT_ADMIN_ROLE**: Assigned to "guardian", manages other roles
- **MANAGER_ROLE**: Assigned to "timelock", controls fund releases
- **PAUSER_ROLE**: Can pause/unpause contract operations
- **UPGRADER_ROLE**: Can authorize contract upgrades

This separation of concerns is a security best practice, limiting the power of any single entity.

## 4. Detailed Feature Analysis

### 4.1 Vesting Schedule

The vesting schedule is initialized with:
- **Start time**: 180 days before contract initialization
- **Duration**: 1095 days (3 years)
- **Vesting formula**: Linear vesting based on elapsed time

This creates an immediate partial vesting at contract initialization (180 days worth), followed by continuous vesting over the remaining period.

### 4.2 Fund Release Mechanism

Two separate methods handle fund releases:
- **ETH release**: Protected with `nonReentrant` modifier
- **ERC20 release**: Uses SafeERC20 for secure transfers

Both functions:
1. Verify the caller has the MANAGER_ROLE
2. Check the contract is not paused
3. Validate the recipient address is not zero
4. Ensure the requested amount is within vested limits
5. Update release accounting
6. Transfer funds and emit events

### 4.3 Upgrade Mechanism

The contract uses the UUPS (Universal Upgradeable Proxy Standard) pattern:
- Restricted to UPGRADER_ROLE
- Version tracking with automatic incrementation
- Storage gap for future upgrades
- Events emitted on upgrades

## 5. Security Assessment

### 5.1 Strengths

1. **Comprehensive Access Control**: Well-implemented role-based permissions
2. **Reentrancy Protection**: For ETH transfers
3. **Pausability**: Emergency controls for unforeseen issues
4. **SafeERC20**: Protection against malicious tokens
5. **Storage Gap**: Future-proofing for upgrades
6. **Input Validation**: Checks for zero addresses and vested amounts
7. **Accurate Vesting Calculations**: Correctly implements linear vesting

### 5.2 Concerns and Recommendations

#### High Severity

No high severity issues identified.



**Inherits:**
[ITREASURY](/contracts/interfaces/ITreasury.sol/interface.ITREASURY.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Vesting contract: initialRelease + (36 month duration)

Offers flexible withdrawal schedule (gas efficient)

*Implements secure and upgradeable DAO treasury with linear vesting*

**Notes:**
- security-contact: security@nebula-labs.xysz

- oz-upgrades: 


## State Variables
### PAUSER_ROLE
*AccessControl Pauser Role*


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE
*AccessControl Manager Role*


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
*AccessControl Upgrader Role*


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### _released
*ETH amount released so far*


```solidity
uint256 private _released;
```


### _start
*start timestamp*


```solidity
uint64 private _start;
```


### _duration
*duration seconds*


```solidity
uint64 private _duration;
```


### version
*UUPS version*


```solidity
uint32 public version;
```


### _erc20Released
*token amounts released so far*


```solidity
mapping(address token => uint256) private _erc20Released;
```


### __gap
*upgrade gap*


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

solidity receive function


```solidity
receive() external payable virtual;
```

### initialize

*Initializes the UUPS contract*


```solidity
function initialize(address guardian, address timelock) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|admin address|
|`timelock`|`address`|address of timelock contract|


### pause

Emergency function to pause contract operations

*Pauses all token transfers and releases.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Paused} from PausableUpgradeable


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Resumes normal contract operations after pause

*Unpauses token transfers and releases.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Unpaused} from PausableUpgradeable


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### release

Allows the manager to release vested ETH to a beneficiary

*Release the native token (ether) that have already vested.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: Amount must not exceed vested amount

- requires: Beneficiary address must not be zero

- security: non-reentrant

- access: restricted to MANAGER_ROLE

- events-emits: {EtherReleased}


```solidity
function release(address to, uint256 amount) external nonReentrant whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address that will receive the vested ETH|
|`amount`|`uint256`|The amount of ETH to release|


### release

Allows the manager to release vested tokens to a beneficiary

*Release the ERC20 tokens that have already vested.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: Amount must not exceed vested amount

- requires: Token address must not be zero

- requires: Beneficiary address must not be zero

- access: restricted to MANAGER_ROLE

- events-emits: {ERC20Released}


```solidity
function release(address token, address to, uint256 amount) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the ERC20 token to release|
|`to`|`address`|The address that will receive the vested tokens|
|`amount`|`uint256`|The amount of tokens to release|


### start

*Getter for the start timestamp.*


```solidity
function start() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|start timestamp|


### duration

*Getter for the vesting duration.*


```solidity
function duration() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|duration seconds|


### end

*Getter for the end timestamp.*


```solidity
function end() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|end timnestamp|


### released

*Getter for the amount of eth already released*


```solidity
function released() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of ETH released so far|


### released

*Getter for the amount of token already released*


```solidity
function released(address token) public view virtual returns (uint256);
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
function releasable() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested ETH|


### releasable

*Getter for the amount of vested `ERC20` tokens.*


```solidity
function releasable(address token) public view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested tokens|


### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

### vestedAmount

*Calculates the amount of ETH that has already vested. Default implementation is a linear vesting curve.*


```solidity
function vestedAmount(uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount ETH vested|


### vestedAmount

*Calculates the amount of tokens that has already vested. Default implementation is a linear vesting curve.*


```solidity
function vestedAmount(address token, uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address of token|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount vested|


### _vestingSchedule

*Virtual implementation of the vesting formula. This returns the amount vested, as a function of time, for
an asset given its total historical allocation.*


```solidity
function _vestingSchedule(uint256 totalAllocation, uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAllocation`|`uint256`|initial amount|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount vested|


