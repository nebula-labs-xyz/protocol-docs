# LendefiGovernor
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/LendefiGovernor.sol)

**Inherits:**
GovernorUpgradeable, GovernorSettingsUpgradeable, GovernorCountingSimpleUpgradeable, GovernorVotesUpgradeable, GovernorVotesQuorumFractionUpgradeable, GovernorTimelockControlUpgradeable, Ownable2StepUpgradeable, UUPSUpgradeable

Standard OZUpgradeable governor, small modification with UUPS

*Implements a secure and upgradeable DAO governor*

**Notes:**
- security-contact: security@nebula-labs.xyz

- oz-upgrades: 


## State Variables
### uupsVersion
*UUPS version tracker*


```solidity
uint32 public uupsVersion;
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

### initialize

*Initializes the UUPS contract*


```solidity
function initialize(IVotes _token, TimelockControllerUpgradeable _timelock, address guardian) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`IVotes`|IVotes token instance|
|`_timelock`|`TimelockControllerUpgradeable`|timelock instance|
|`guardian`|`address`|owner address|


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

### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyOwner;
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
*Initialized Event.*


```solidity
event Initialized(address indexed src);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|sender address|

### Upgrade
*event emitted on UUPS upgrade*


```solidity
event Upgrade(address indexed src, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|upgrade sender address|
|`implementation`|`address`|new implementation address|

## Errors
### CustomError
*Custom Error.*


```solidity
error CustomError(string msg);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`msg`|`string`|error desription|

