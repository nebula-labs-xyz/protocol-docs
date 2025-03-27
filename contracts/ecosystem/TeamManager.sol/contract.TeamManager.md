# TeamManager
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/TeamManager.sol)

**Inherits:**
[ITEAMMANAGER](/contracts/interfaces/ITeamManager.sol/interface.ITEAMMANAGER.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Creates and deploys team vesting contracts

*Implements a secure and upgradeable team manager with upgrade timelock*

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
*Minimum cliff period (3 months)*


```solidity
uint64 private constant MIN_CLIFF = 90 days;
```


### MAX_CLIFF
*Maximum cliff period (1 year)*


```solidity
uint64 private constant MAX_CLIFF = 365 days;
```


### MIN_DURATION
*Minimum vesting duration (1 year)*


```solidity
uint64 private constant MIN_DURATION = 365 days;
```


### MAX_DURATION
*Maximum vesting duration (4 years)*


```solidity
uint64 private constant MAX_DURATION = 1460 days;
```


### UPGRADE_TIMELOCK_DURATION
*Upgrade timelock duration (4 days)*


```solidity
uint256 private constant UPGRADE_TIMELOCK_DURATION = 3 days;
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


### pendingUpgrade
*Pending upgrade information*


```solidity
UpgradeRequest public pendingUpgrade;
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
*gap for future storage variables (50 - 8 existing variables = 42)*


```solidity
uint256[22] private __gap;
```


## Functions
### nonZeroAddress

*Modifier to check for non-zero address*


```solidity
modifier nonZeroAddress(address addr);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|The address to check|


### nonZeroAmount

*Modifier to check for non-zero amount*


```solidity
modifier nonZeroAmount(uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount to check|


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

*Sets up the initial state of the contract with core functionality*


```solidity
function initialize(address token, address timelock_, address multisig) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the ecosystem token contract|
|`timelock_`|`address`|The address of the timelock controller|
|`multisig`|`address`|The address receiving UPGRADER_ROLE|


### pause

Pauses all contract operations

*Prevents execution of state-modifying functions*


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Resumes all contract operations

*Re-enables execution of state-modifying functions*


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### addTeamMember

*Create and fund a vesting contract for a new team member*


```solidity
function addTeamMember(address beneficiary, uint256 amount, uint256 cliff, uint256 duration)
    external
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE)
    nonZeroAddress(beneficiary)
    nonZeroAmount(amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`beneficiary`|`address`|The address of the team member|
|`amount`|`uint256`|The amount of tokens to vest|
|`cliff`|`uint256`|The cliff period in seconds|
|`duration`|`uint256`|The vesting duration in seconds after cliff|


### scheduleUpgrade

*Schedules an upgrade to a new implementation*


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
|`<none>`|`uint256`|timeRemaining The time remaining in seconds, or 0 if no upgrade is scheduled or timelock has passed|


### _authorizeUpgrade

Authorizes and processes contract upgrades with timelock enforcement

*Internal override for UUPS upgrade authorization*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


