# IINVMANAGER
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/interfaces/IInvestmentManager.sol)

Interface for the contract that manages investment rounds and token vesting

*Defines external functions and events for the InvestmentManager contract*


## Functions
### initialize

*Initializes the contract*


```solidity
function initialize(address token, address timelock_, address treasury_) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ecosystem token|
|`timelock_`|`address`|Address of the timelock controller|
|`treasury_`|`address`|Address of the treasury|


### pause

*Pauses the contract*


```solidity
function pause() external;
```

### unpause

*Unpauses the contract*


```solidity
function unpause() external;
```

### scheduleUpgrade

*Schedules an upgrade to a new implementation*


```solidity
function scheduleUpgrade(address newImplementation) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


### emergencyWithdrawToken

*Executes an emergency withdrawal*


```solidity
function emergencyWithdrawToken(address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token to withdraw (0xEeee... for ETH)|


### emergencyWithdrawEther

*Executes an emergency withdrawal*


```solidity
function emergencyWithdrawEther() external;
```

### createRound

*Creates a new investment round*


```solidity
function createRound(
    uint64 start,
    uint64 duration,
    uint256 ethTarget,
    uint256 tokenAlloc,
    uint64 vestingCliff,
    uint64 vestingDuration
) external returns (uint32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`start`|`uint64`|Start timestamp of the round|
|`duration`|`uint64`|Duration in seconds|
|`ethTarget`|`uint256`|Target ETH amount|
|`tokenAlloc`|`uint256`|Total token allocation|
|`vestingCliff`|`uint64`|Cliff period for vesting in seconds|
|`vestingDuration`|`uint64`|Duration for vesting in seconds|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|ID of the created round|


### activateRound

*Activates a pending round*


```solidity
function activateRound(uint32 roundId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round to activate|


### addInvestorAllocation

*Adds an investor allocation to a round*


```solidity
function addInvestorAllocation(uint32 roundId, address investor, uint256 ethAmount, uint256 tokenAmount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`ethAmount`|`uint256`|ETH allocation amount|
|`tokenAmount`|`uint256`|Token allocation amount|


### removeInvestorAllocation

*Removes an investor allocation from a round*


```solidity
function removeInvestorAllocation(uint32 roundId, address investor) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|


### cancelInvestment

*Cancels an investment in a round*


```solidity
function cancelInvestment(uint32 roundId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|


### finalizeRound

*Finalizes a completed round*


```solidity
function finalizeRound(uint32 roundId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|


### cancelRound

*Cancels a round*


```solidity
function cancelRound(uint32 roundId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|


### claimRefund

*Claims a refund for a cancelled round*


```solidity
function claimRefund(uint32 roundId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|


### investEther

*Invests ETH in a round*


```solidity
function investEther(uint32 roundId) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|


### upgradeTimelockRemaining

*Returns the remaining time before a scheduled upgrade can be executed*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Time remaining in seconds|


### getRefundAmount

*Gets the refund amount for an investor in a round*


```solidity
function getRefundAmount(uint32 roundId, address investor) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Refund amount|


### getInvestorDetails

*Gets investor details for a round*


```solidity
function getInvestorDetails(uint32 roundId, address investor)
    external
    view
    returns (uint256 etherAmount, uint256 tokenAmount, uint256 invested, address vestingContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`etherAmount`|`uint256`|ETH allocation amount|
|`tokenAmount`|`uint256`|Token allocation amount|
|`invested`|`uint256`|Amount invested|
|`vestingContract`|`address`|Address of the vesting contract|


### getEcosystemToken

*Gets the address of the ecosystem token*


```solidity
function getEcosystemToken() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the ecosystem token|


### getRoundInfo

*Gets information about a round*


```solidity
function getRoundInfo(uint32 roundId) external view returns (Round memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Round`|Round information|


### getRoundInvestors

*Gets list of investors in a round*


```solidity
function getRoundInvestors(uint32 roundId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of investor addresses|


### getCurrentRound

*Gets the current active round*


```solidity
function getCurrentRound() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|ID of the current active round, or type(uint32).max if none|


### timelock

*Gets the timelock controller address*


```solidity
function timelock() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the timelock controller|


### treasury

*Gets the treasury address*


```solidity
function treasury() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the treasury|


### supply

*Gets the total token supply managed by this contract*


```solidity
function supply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total supply|


### version

*Gets the contract version*


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|Contract version|


## Events
### Initialized
*Emitted when the contract is initialized*


```solidity
event Initialized(address indexed initializer);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initializer`|`address`|Address that called initialize|

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
|`sender`|`address`|Address that scheduled the upgrade|
|`implementation`|`address`|New implementation address|
|`scheduledTime`|`uint64`|Time when upgrade was scheduled|
|`effectiveTime`|`uint64`|Time when upgrade can be executed|

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

### Upgraded
*Emitted when contract implementation is upgraded*


```solidity
event Upgraded(address indexed upgrader, address indexed implementation, uint32 version);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`upgrader`|`address`|Address that performed the upgrade|
|`implementation`|`address`|New implementation address|
|`version`|`uint32`|New version number|

### EmergencyWithdrawal
*Emitted when an emergency withdrawal is executed*


```solidity
event EmergencyWithdrawal(address indexed token, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token withdrawn (ethereum constant for ETH)|
|`amount`|`uint256`|Amount withdrawn|

### CreateRound
*Emitted when a new investment round is created*


```solidity
event CreateRound(
    uint32 indexed roundId, uint64 startTime, uint64 duration, uint256 etherTarget, uint256 tokenAllocation
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the created round|
|`startTime`|`uint64`|Start timestamp of the round|
|`duration`|`uint64`|Duration in seconds|
|`etherTarget`|`uint256`|Target ETH amount|
|`tokenAllocation`|`uint256`|Total token allocation|

### RoundStatusUpdated
*Emitted when a round's status is updated*


```solidity
event RoundStatusUpdated(uint32 indexed roundId, RoundStatus status);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`status`|`RoundStatus`|New status|

### InvestorAllocated
*Emitted when an investor is allocated in a round*


```solidity
event InvestorAllocated(uint32 indexed roundId, address indexed investor, uint256 etherAmount, uint256 tokenAmount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`etherAmount`|`uint256`|ETH allocation amount|
|`tokenAmount`|`uint256`|Token allocation amount|

### InvestorAllocationRemoved
*Emitted when an investor's allocation is removed*


```solidity
event InvestorAllocationRemoved(
    uint32 indexed roundId, address indexed investor, uint256 etherAmount, uint256 tokenAmount
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`etherAmount`|`uint256`|ETH allocation amount removed|
|`tokenAmount`|`uint256`|Token allocation amount removed|

### CancelInvestment
*Emitted when an investment is cancelled*


```solidity
event CancelInvestment(uint32 indexed roundId, address indexed investor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`amount`|`uint256`|ETH amount returned|

### RoundFinalized
*Emitted when a round is finalized*


```solidity
event RoundFinalized(address indexed finalizer, uint32 indexed roundId, uint256 ethAmount, uint256 tokenAmount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`finalizer`|`address`|Address that finalized the round|
|`roundId`|`uint32`|ID of the round|
|`ethAmount`|`uint256`|Total ETH raised|
|`tokenAmount`|`uint256`|Total tokens distributed|

### RoundCancelled
*Emitted when a round is cancelled*


```solidity
event RoundCancelled(uint32 indexed roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the cancelled round|

### RefundClaimed
*Emitted when a refund is claimed*


```solidity
event RefundClaimed(uint32 indexed roundId, address indexed investor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`amount`|`uint256`|ETH amount refunded|

### Invest
*Emitted when an investment is made*


```solidity
event Invest(uint32 indexed roundId, address indexed investor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`amount`|`uint256`|ETH amount invested|

### RoundComplete
*Emitted when a round is completed*


```solidity
event RoundComplete(uint32 indexed roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the completed round|

### DeployVesting
*Emitted when a vesting contract is deployed*


```solidity
event DeployVesting(
    uint32 indexed roundId, address indexed investor, address indexed vestingContract, uint256 allocation
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|
|`investor`|`address`|Address of the investor|
|`vestingContract`|`address`|Address of the vesting contract|
|`allocation`|`uint256`|Token allocation amount|

## Errors
### UpgradeTimelockActive
*Error thrown when trying to execute an upgrade before timelock expires*


```solidity
error UpgradeTimelockActive(uint256 remainingTime);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`remainingTime`|`uint256`|Time remaining until upgrade can be executed|

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

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`expected`|`address`|Expected implementation address|
|`provided`|`address`|Provided implementation address|

### InvalidRound
*Error thrown when an invalid round ID is provided*


```solidity
error InvalidRound(uint32 roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The invalid round ID|

### RoundNotActive
*Error thrown when a round is not in active status*


```solidity
error RoundNotActive(uint32 roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The round ID|

### InvalidRoundStatus
*Error thrown when a round is in the wrong status*


```solidity
error InvalidRoundStatus(uint32 roundId, RoundStatus requiredStatus, RoundStatus currentStatus);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The round ID|
|`requiredStatus`|`RoundStatus`|Required status|
|`currentStatus`|`RoundStatus`|Current status|

### ZeroAddressDetected
*Error thrown when a zero address is provided*


```solidity
error ZeroAddressDetected();
```

### InvalidAmount
*Error thrown when an invalid amount is provided*


```solidity
error InvalidAmount(uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The invalid amount|

### InvalidDuration
*Error thrown when a duration is outside allowed range*


```solidity
error InvalidDuration(uint256 provided, uint256 minimum, uint256 maximum);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint256`|Provided duration|
|`minimum`|`uint256`|Minimum allowed duration|
|`maximum`|`uint256`|Maximum allowed duration|

### InvalidEthTarget
*Error thrown when ETH target is invalid*


```solidity
error InvalidEthTarget();
```

### InvalidTokenAllocation
*Error thrown when token allocation is invalid*


```solidity
error InvalidTokenAllocation();
```

### InvalidStartTime
*Error thrown when start time is in the past*


```solidity
error InvalidStartTime(uint64 provided, uint256 current);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint64`|Provided start time|
|`current`|`uint256`|Current time|

### InsufficientSupply
*Error thrown when token supply is insufficient*


```solidity
error InsufficientSupply(uint256 required, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`required`|`uint256`|Required amount|
|`available`|`uint256`|Available amount|

### RoundStartTimeNotReached
*Error thrown when round start time hasn't been reached*


```solidity
error RoundStartTimeNotReached(uint256 currentTime, uint256 startTime);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`currentTime`|`uint256`|Current time|
|`startTime`|`uint256`|Start time of the round|

### RoundEndTimeReached
*Error thrown when round end time has been reached*


```solidity
error RoundEndTimeReached(uint256 currentTime, uint256 endTime);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`currentTime`|`uint256`|Current time|
|`endTime`|`uint256`|End time of the round|

### InvalidInvestor
*Error thrown when an invalid investor address is provided*


```solidity
error InvalidInvestor();
```

### InvalidEthAmount
*Error thrown when ETH amount is invalid*


```solidity
error InvalidEthAmount();
```

### InvalidTokenAmount
*Error thrown when token amount is invalid*


```solidity
error InvalidTokenAmount();
```

### AllocationExists
*Error thrown when allocation already exists for an investor*


```solidity
error AllocationExists(address investor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

### ExceedsRoundAllocation
*Error thrown when allocation exceeds round allocation*


```solidity
error ExceedsRoundAllocation(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|Requested allocation|
|`available`|`uint256`|Available allocation|

### NoAllocationExists
*Error thrown when no allocation exists for an investor*


```solidity
error NoAllocationExists(address investor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

### InvestorHasActivePosition
*Error thrown when investor already has an active position*


```solidity
error InvestorHasActivePosition(address investor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

### NoInvestment
*Error thrown when investor has no investment*


```solidity
error NoInvestment(address investor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

### RoundNotCancelled
*Error thrown when a round is not cancelled*


```solidity
error RoundNotCancelled(uint32 roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|

### NoRefundAvailable
*Error thrown when no refund is available*


```solidity
error NoRefundAvailable(address investor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

### InvalidStatusTransition
*Error thrown when status transition is invalid*


```solidity
error InvalidStatusTransition(RoundStatus current, RoundStatus new_);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`current`|`RoundStatus`|Current status|
|`new_`|`RoundStatus`|New status|

### NoActiveRound
*Error thrown when no active round exists*


```solidity
error NoActiveRound();
```

### RoundEnded
*Error thrown when a round has ended*


```solidity
error RoundEnded(uint32 roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|

### RoundOversubscribed
*Error thrown when a round is oversubscribed*


```solidity
error RoundOversubscribed(uint32 roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|ID of the round|

### NoAllocation
*Error thrown when investor has no allocation*


```solidity
error NoAllocation(address investor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

### AmountAllocationMismatch
*Error thrown when amount doesn't match allocation*


```solidity
error AmountAllocationMismatch(uint256 provided, uint256 required);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provided`|`uint256`|Provided amount|
|`required`|`uint256`|Required amount|

### ZeroBalance
*Error thrown when attempting operations with zero balance*


```solidity
error ZeroBalance();
```

## Structs
### Allocation
Investor allocation details

*Tracks individual investor allocations within a round*


```solidity
struct Allocation {
    uint256 etherAmount;
    uint256 tokenAmount;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`etherAmount`|`uint256`|Maximum ETH that can be invested|
|`tokenAmount`|`uint256`|Tokens to be received for full allocation|

### Round
Investment round details

*Contains all parameters and current state of an investment round*


```solidity
struct Round {
    uint256 etherTarget;
    uint256 etherInvested;
    uint256 tokenAllocation;
    uint256 tokenDistributed;
    uint64 startTime;
    uint64 endTime;
    uint64 vestingCliff;
    uint64 vestingDuration;
    uint32 participants;
    RoundStatus status;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`etherTarget`|`uint256`|Total ETH to be raised|
|`etherInvested`|`uint256`|Current amount of ETH invested|
|`tokenAllocation`|`uint256`|Total tokens allocated to the round|
|`tokenDistributed`|`uint256`|Tokens that have been distributed to vesting contracts|
|`startTime`|`uint64`|Round opening timestamp|
|`endTime`|`uint64`|Round closing timestamp|
|`vestingCliff`|`uint64`|Time before vesting begins|
|`vestingDuration`|`uint64`|Total vesting period length|
|`participants`|`uint32`|Number of investors in the round|
|`status`|`RoundStatus`|Current state of the round|

### UpgradeRequest
Upgrade request details

*Tracks pending contract upgrades with timelock*


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
|`implementation`|`address`|New implementation contract address|
|`scheduledTime`|`uint64`|When the upgrade was requested|
|`exists`|`bool`|Whether this upgrade request is active|

## Enums
### RoundStatus
*Enum representing the status of an investment round*


```solidity
enum RoundStatus {
    PENDING,
    ACTIVE,
    COMPLETED,
    CANCELLED,
    FINALIZED
}
```

