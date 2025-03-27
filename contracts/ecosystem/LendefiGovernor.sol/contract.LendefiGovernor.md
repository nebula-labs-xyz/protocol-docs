# LendefiGovernor
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/LendefiGovernor.sol)

**Inherits:**
GovernorUpgradeable, GovernorSettingsUpgradeable, GovernorCountingSimpleUpgradeable, GovernorVotesUpgradeable, GovernorVotesQuorumFractionUpgradeable, GovernorTimelockControlUpgradeable, AccessControlUpgradeable, UUPSUpgradeable

Standard OZUpgradeable governor with UUPS and AccessControl

*Implements a secure and upgradeable DAO governor with consistent role patterns*

**Notes:**
- security-contact: security@nebula-labs.xyz

- oz-upgrades: 


## State Variables
### UPGRADER_ROLE
*Role identifier for addresses that can upgrade the contract*

**Note:**
security: Should be granted carefully as this is a critical permission


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### DEFAULT_VOTING_DELAY
Default voting delay in blocks (approximately 1 day)

*The period after a proposal is created during which voting cannot start*


```solidity
uint48 public constant DEFAULT_VOTING_DELAY = 7200;
```


### DEFAULT_VOTING_PERIOD
Default voting period in blocks (approximately 1 week)

*The period during which voting can occur*


```solidity
uint32 public constant DEFAULT_VOTING_PERIOD = 50400;
```


### DEFAULT_PROPOSAL_THRESHOLD
Default proposal threshold (20,000 tokens)

*The minimum number of votes needed to submit a proposal*


```solidity
uint256 public constant DEFAULT_PROPOSAL_THRESHOLD = 20_000 ether;
```


### UPGRADE_TIMELOCK_DURATION
Duration of the timelock for upgrade operations (3 days)

*Time that must elapse between scheduling and executing an upgrade*

**Note:**
security: Provides time for users to respond to potentially malicious upgrades


```solidity
uint256 public constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### uupsVersion
UUPS upgrade version tracker

*Incremented with each upgrade to track contract versions*


```solidity
uint32 public uupsVersion;
```


### pendingUpgrade
Information about the currently pending upgrade

*Will have exists=false if no upgrade is pending*


```solidity
UpgradeRequest public pendingUpgrade;
```


### __gap
*Reserved storage space for future upgrades*

**Note:**
oz-upgrades-unsafe-allow: state-variable-immutable


```solidity
uint256[21] private __gap;
```


## Functions
### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### initialize

*Initializes the UUPS contract*


```solidity
function initialize(IVotes _token, TimelockControllerUpgradeable _timelock, address _gnosisSafe) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`IVotes`|IVotes token instance|
|`_timelock`|`TimelockControllerUpgradeable`|timelock instance|
|`_gnosisSafe`|`address`|multisig address for emergency functions and upgrades|


### scheduleUpgrade

Schedules an upgrade to a new implementation with timelock

*Can only be called by addresses with UPGRADER_ROLE*


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


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|timeRemaining The time remaining in seconds, or 0 if no upgrade is scheduled or timelock has passed|


### supportsInterface


```solidity
function supportsInterface(bytes4 interfaceId)
    public
    view
    override(GovernorUpgradeable, AccessControlUpgradeable)
    returns (bool);
```

### votingDelay


```solidity
function votingDelay() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256);
```

### votingPeriod


```solidity
function votingPeriod() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256);
```

### quorum


```solidity
function quorum(uint256 blockNumber)
    public
    view
    override(GovernorUpgradeable, GovernorVotesQuorumFractionUpgradeable)
    returns (uint256);
```

### state


```solidity
function state(uint256 proposalId)
    public
    view
    override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)
    returns (ProposalState);
```

### proposalNeedsQueuing


```solidity
function proposalNeedsQueuing(uint256 proposalId)
    public
    view
    override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)
    returns (bool);
```

### proposalThreshold


```solidity
function proposalThreshold() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256);
```

### _queueOperations


```solidity
function _queueOperations(
    uint256 proposalId,
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    bytes32 descriptionHash
) internal override(GovernorUpgradeable, GovernorTimelockControlUpgradeable) returns (uint48);
```

### _executeOperations


```solidity
function _executeOperations(
    uint256 proposalId,
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    bytes32 descriptionHash
) internal override(GovernorUpgradeable, GovernorTimelockControlUpgradeable);
```

### _cancel


```solidity
function _cancel(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash)
    internal
    override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)
    returns (uint256);
```

### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

### _executor


```solidity
function _executor()
    internal
    view
    override(GovernorUpgradeable, GovernorTimelockControlUpgradeable)
    returns (address);
```

## Events
### Initialized
Emitted when the contract is initialized


```solidity
event Initialized(address indexed src);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|The address that initialized the contract|

### Upgrade
Emitted when the contract is upgraded


```solidity
event Upgrade(address indexed src, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|The address that executed the upgrade|
|`implementation`|`address`|The address of the new implementation|

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
### ZeroAddress
Error thrown when a zero address is provided


```solidity
error ZeroAddress();
```

### UpgradeTimelockActive
Error thrown when attempting to execute an upgrade before timelock expires


```solidity
error UpgradeTimelockActive(uint256 timeRemaining);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timeRemaining`|`uint256`|The time remaining until the upgrade can be executed|

### UpgradeNotScheduled
Error thrown when attempting to execute an upgrade that wasn't scheduled


```solidity
error UpgradeNotScheduled();
```

### ImplementationMismatch
Error thrown when implementation address doesn't match scheduled upgrade


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

