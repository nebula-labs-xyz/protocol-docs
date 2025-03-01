# Treasury
[Git Source](https://github.com/nebula-labs-xyz/lendefi-dao/blob/07f5cb7369219dbffd648091ffbddb6d70a0157c/contracts/ecosystem/Treasury.sol)

## Overview

The Treasury contract serves as the financial backbone of the Lendefi DAO, managing token vesting over a 36-month period with a linear release schedule. It offers a secure, upgradeable system for controlled fund distribution with role-based access controls and comprehensive security measures.

## Architecture Analysis

### Design Philosophy

The contract demonstrates a thoughtful "separation of concerns" architecture with:
- **Access-controlled operations**: Different roles for different functionalities
- **Linear vesting mechanism**: Predictable, time-based token release
- **Secure upgradeability**: UUPS pattern with version tracking
- **Emergency controls**: Pausability for risk mitigation

### Key Components

1. **Vesting Engine**
   - Linear schedule over 3 years (1095 days)
   - Retroactive start (180 days before initialization)
   - Support for both ETH and ERC20 tokens
   - Time-proportional calculation formula

2. **Role Management**
   - MANAGER_ROLE: Controls fund releases (assigned to timelock)
   - PAUSER_ROLE: Emergency controls
   - UPGRADER_ROLE: Contract upgrade authorization
   - DEFAULT_ADMIN_ROLE: Role management (assigned to guardian)

3. **Fund Management**
   - Separate tracking for ETH and each ERC20 token
   - Granular release amounts (vs. releasing all vested funds)
   - Proper accounting of historical releases

## Technical Assessment

### Strengths

1. **Security Measures**
   - ReentrancyGuard on ETH transfers
   - SafeERC20 for token operations
   - Address.sendValue for safe ETH transfers
   - Input validation with custom errors
   - State updates before external calls
   - Pause functionality for emergency response

2. **Upgradeability Pattern**
   - Well-implemented UUPS pattern
   - Version tracking with increment on upgrades
   - Storage gap for future extensions
   - Event emission for upgrade transparency

3. **Gas Efficiency**
   - Optimal storage usage
   - Efficient vesting calculation
   - SafeCast for type conversions
   - Specific amount releases vs. releasing all vested funds

4. **Governance Integration**
   - Timelock assignment for fund management
   - Guardian role for administrative oversight
   - Clear separation between immediate and governed actions

### Potential Concerns

1. **Retroactive Start Time**
   - Setting start time to 180 days before deployment means ~5% is immediately vested
   - This creates an unusual vesting pattern with immediate availability
   - May cause confusion about actual vesting duration

2. **Limited Recovery Mechanisms**
   - No functionality to recover accidentally sent tokens
   - No way to handle token rebasing or fee-on-transfer tokens
   - No mechanism to adjust parameters post-initialization

3. **Release Validation**
   - Release functions don't verify that token contracts exist
   - No batch release functionality for gas optimization
   - No specific handling for tokens with transfer restrictions

4. **Technical Oddities**
   - Typo in function documentation ("timnestamp")
   - Unnecessary "virtual" keyword on some view functions
   - Security contact email has typo ("xysz" instead of "xyz")

## Implementation Details

### Vesting Calculation

The contract implements a standard linear vesting formula:
```
vestedAmount = totalAllocation * (currentTime - startTime) / duration
```

With boundary conditions:
- Before start: 0% vested
- After duration: 100% vested
- During vesting: Linear proportion based on elapsed time

### Release Mechanism

The release functions implement a "partial withdrawal" pattern that:
1. Validates recipient address is not zero
2. Verifies requested amount doesn't exceed vested amount
3. Updates release accounting before transfer
4. Emits event with details
5. Transfers funds to recipient

This approach offers flexibility in fund management but requires more explicit tracking.



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


