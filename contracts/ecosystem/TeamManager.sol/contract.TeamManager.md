# TeamManager
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/TeamManager.sol)

**Inherits:**
[ITEAMMANAGER](/contracts/interfaces/ITeamManager.sol/interface.ITEAMMANAGER.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Creates and deploys team vesting contracts

*Implements a secure and upgradeable team manager for the DAO*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

- oz-upgrades: 


## State Variables
### TEAM_ALLOCATION_PERCENT
*Team allocation percentage of total supply (18%)*


```solidity
uint256 private constant TEAM_ALLOCATION_PERCENT = 18;
```


### MIN_CLIFF
*Minimum cliff period (6 months)*


```solidity
uint64 public constant MIN_CLIFF = 90 days;
```


### MAX_CLIFF
*Maximum cliff period (2 years)*


```solidity
uint64 public constant MAX_CLIFF = 365 days;
```


### MIN_DURATION
*Minimum vesting duration (1 year)*


```solidity
uint64 public constant MIN_DURATION = 365 days;
```


### MAX_DURATION
*Maximum vesting duration (4 years)*


```solidity
uint64 public constant MAX_DURATION = 1460 days;
```


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


### ecosystemToken
*governance token instance*


```solidity
ILENDEFI internal ecosystemToken;
```


### supply
*amount of ecosystem tokens in the contract*


```solidity
uint256 public supply;
```


### totalAllocation
*amount of tokens allocated so far*


```solidity
uint256 public totalAllocation;
```


### timelock
*timelock address*


```solidity
address public timelock;
```


### version
*number of UUPS upgrades*


```solidity
uint32 public version;
```


### allocations
*token allocations to team members*


```solidity
mapping(address src => uint256 amount) public allocations;
```


### vestingContracts
*vesting contract addresses for team members*


```solidity
mapping(address src => address vesting) public vestingContracts;
```


### __gap
*gap for future storage variables*


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

*Prevents receiving Ether*


```solidity
receive() external payable;
```

### initialize

Initializes the team manager contract

*Sets up the initial state of the contract with core functionality:
1. Initializes upgradeable base contracts
2. Sets up access control roles
3. Configures token and supply parameters*

**Notes:**
- requires-role: None - can only be called once during initialization

- security: Implements initializer modifier to prevent re-initialization

- security: Validates all input addresses are non-zero

- events-emits: Initialized(msg.sender)

- throws: CustomError("ZERO_ADDRESS_DETECTED") if any input address is zero


```solidity
function initialize(address token, address timelock_, address guardian) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the ecosystem token contract|
|`timelock_`|`address`|The address of the timelock controller|
|`guardian`|`address`|The address of the admin who will receive DEFAULT_ADMIN_ROLE|


### pause

Pauses all contract operations

*Prevents execution of state-modifying functions*

**Notes:**
- requires-role: PAUSER_ROLE

- security: Inherits OpenZeppelin's PausableUpgradeable

- events-emits: {Paused} event from PausableUpgradeable

- throws: Unauthorized if caller lacks PAUSER_ROLE


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Resumes all contract operations

*Re-enables execution of state-modifying functions*

**Notes:**
- requires-role: PAUSER_ROLE

- security: Inherits OpenZeppelin's PausableUpgradeable

- events-emits: {Unpaused} event from PausableUpgradeable

- throws: Unauthorized if caller lacks PAUSER_ROLE


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### addTeamMember

*Create and fund a vesting contract for a new team member*

**Notes:**
- requires: beneficiary must not be zero address

- requires: cliff must be between MIN_CLIFF and MAX_CLIFF

- requires: duration must be between MIN_DURATION and MAX_DURATION

- requires: amount must not exceed remaining supply

- throws: CustomError("SUPPLY_LIMIT") if allocation exceeds supply

- throws: CustomError("INVALID_BENEFICIARY") if beneficiary is zero address

- throws: CustomError("INVALID_CLIFF") if cliff period is invalid

- throws: CustomError("INVALID_DURATION") if duration is invalid

- throws: CustomError("ALREADY_ADDED") if beneficiary already has allocation


```solidity
function addTeamMember(address beneficiary, uint256 amount, uint256 cliff, uint256 duration)
    external
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`beneficiary`|`address`|The address of the team member|
|`amount`|`uint256`|The amount of tokens to vest|
|`cliff`|`uint256`|The cliff period in seconds|
|`duration`|`uint256`|The vesting duration in seconds after cliff|


### _authorizeUpgrade

Authorizes and processes contract upgrades

*Internal override for UUPS upgrade authorization*

*Performs:
1. Validates caller has UPGRADER_ROLE
2. Increments contract version
3. Emits upgrade event with details*

**Notes:**
- throws: Unauthorized if caller lacks UPGRADER_ROLE

- emits: Upgrade event with upgrader address and new implementation

- security: Role-based access control via UPGRADER_ROLE

- security: Version tracking for upgrade management

- security: Inherits OpenZeppelin's UUPSUpgradeable pattern


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


