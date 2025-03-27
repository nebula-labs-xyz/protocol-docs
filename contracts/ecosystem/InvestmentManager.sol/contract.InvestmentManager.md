# InvestmentManager
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/InvestmentManager.sol)

**Inherits:**
[IINVMANAGER](/contracts/interfaces/IInvestmentManager.sol/interface.IINVMANAGER.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Manages investment rounds and token vesting for the ecosystem

*Implements a secure and upgradeable investment management system*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

- oz-upgrades: 


## State Variables
### MAX_INVESTORS_PER_ROUND
Protects against gas limits when processing all investors during finalization

*Maximum number of investors allowed per investment round*


```solidity
uint256 private constant MAX_INVESTORS_PER_ROUND = 50;
```


### MIN_ROUND_DURATION
Ensures rounds are long enough for investor participation (5 days)

*Minimum duration for an investment round in seconds*


```solidity
uint256 private constant MIN_ROUND_DURATION = 5 days;
```


### MAX_ROUND_DURATION
Prevents overly long rounds that might block capital (90 days)

*Maximum duration for an investment round in seconds*


```solidity
uint256 private constant MAX_ROUND_DURATION = 90 days;
```


### UPGRADE_TIMELOCK_DURATION
Provides a security delay before implementation changes can be executed (3 days)

*Duration of the timelock period for contract upgrades in seconds*


```solidity
uint256 private constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### PAUSER_ROLE
Typically granted to guardian addresses for emergency security actions

*Role identifier for addresses authorized to pause contract functions*


```solidity
bytes32 private constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE
Can perform administrative functions like emergency withdrawals

*Role identifier for addresses authorized to manage investment operations*


```solidity
bytes32 private constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
Controls the ability to schedule and execute contract upgrades

*Role identifier for addresses authorized to upgrade the contract*


```solidity
bytes32 private constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### DAO_ROLE
Has control over creating investment rounds and major decisions

*Role identifier for addresses representing the DAO's governance*


```solidity
bytes32 private constant DAO_ROLE = keccak256("DAO_ROLE");
```


### ecosystemToken
Used for token transfers and vesting distributions

*Reference to the ecosystem's ERC20 token*


```solidity
IERC20 internal ecosystemToken;
```


### timelock
Destination for emergency withdrawals and holds elevated permissions

*Address of the timelock controller*


```solidity
address public timelock;
```


### treasury
Receives ETH from finalized investment rounds

*Address of the treasury contract*


```solidity
address public treasury;
```


### supply
Tracks the supply commitment for all created rounds

*Total amount of tokens allocated for investment rounds*


```solidity
uint256 public supply;
```


### version
Incremented on each upgrade to track contract versions

*Implementation version of this contract*


```solidity
uint32 public version;
```


### rounds
Core data structure that stores round configurations and status

*Array of all investment rounds*


```solidity
Round[] public rounds;
```


### investors
Used to track all participants in each round

*Maps round IDs to arrays of investor addresses*


```solidity
mapping(uint32 => address[]) private investors;
```


### investorPositions
Tracks how much each investor has contributed to a round

*Maps round IDs and investors to their invested ETH amount*


```solidity
mapping(uint32 => mapping(address => uint256)) private investorPositions;
```


### vestingContracts
Used to track deployed vesting contracts for each investor

*Maps round IDs and investors to their vesting contract addresses*


```solidity
mapping(uint32 => mapping(address => address)) private vestingContracts;
```


### investorAllocations
Stores maximum ETH and token amounts for each investor in a round

*Maps round IDs and investors to their token allocations*


```solidity
mapping(uint32 => mapping(address => Allocation)) private investorAllocations;
```


### totalRoundAllocations
Used to ensure round allocations don't exceed limits

*Maps round IDs to the total token allocation for that round*


```solidity
mapping(uint32 => uint256) private totalRoundAllocations;
```


### pendingUpgrade
Implements the timelock upgrade security pattern

*Holds information about a pending contract upgrade*


```solidity
UpgradeRequest public pendingUpgrade;
```


### __gap
Prevents storage collision when adding new variables in upgrades

*Reserved storage slots for future upgrades*


```solidity
uint256[18] private __gap;
```


## Functions
### validRound

*Ensures the provided round ID exists*

**Note:**
throws: InvalidRound if roundId is out of bounds


```solidity
modifier validRound(uint32 roundId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to check|


### activeRound

*Ensures the round is in active status*

**Note:**
throws: RoundNotActive if the round is not active


```solidity
modifier activeRound(uint32 roundId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to check|


### correctStatus

*Ensures the round has a specific status*

**Note:**
throws: InvalidRoundStatus if the round doesn't have the required status


```solidity
modifier correctStatus(uint32 roundId, RoundStatus requiredStatus);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to check|
|`requiredStatus`|`RoundStatus`|The status that the round should have|


### nonZeroAddress

*Ensures the provided address is not zero*

**Note:**
throws: ZeroAddressDetected if the address is zero


```solidity
modifier nonZeroAddress(address addr);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|The address to check|


### constructor

*Ensures the provided amount is not zero*

**Notes:**
- throws: InvalidAmount if the amount is zero

- oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### receive

Automatically invests in the current active round

*Fallback function that handles direct ETH transfers*

**Note:**
throws: NoActiveRound if no rounds are currently active


```solidity
receive() external payable;
```

### initialize

Sets up roles, connects to ecosystem contracts, and initializes version

*Initializes the contract with essential parameters*

**Note:**
throws: ZeroAddressDetected if any parameter is the zero address


```solidity
function initialize(address token, address timelock_, address treasury_) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ecosystem token|
|`timelock_`|`address`|Address of the timelock controller|
|`treasury_`|`address`|Address of the treasury contract|


### pause

Pauses the contract's core functionality

When paused, most state-changing functions will revert

Emergency functions like withdrawals remain active while paused

*Only callable by addresses with PAUSER_ROLE*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Paused} event from PausableUpgradeable


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses the contract, resuming normal operations

Re-enables all functionality that was blocked during pause

*Only callable by addresses with PAUSER_ROLE*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Unpaused} event from PausableUpgradeable


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### scheduleUpgrade

Schedules an upgrade to a new implementation contract

The upgrade cannot be executed until the timelock period has passed

*Initiates the timelock period before an upgrade can be executed*

**Notes:**
- requires-role: UPGRADER_ROLE

- security: Implements a timelock delay for added security

- events-emits: {UpgradeScheduled} when upgrade is scheduled

- throws: ZeroAddressDetected if newImplementation is zero address

- throws: If called by address without UPGRADER_ROLE


```solidity
function scheduleUpgrade(address newImplementation)
    external
    onlyRole(UPGRADER_ROLE)
    nonZeroAddress(newImplementation);
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

### emergencyWithdrawToken

Only callable by addresses with MANAGER_ROLE

*Emergency function to withdraw all tokens to the timelock*

**Notes:**
- throws: ZeroAddressDetected if token address is zero

- throws: ZeroBalance if contract has no token balance


```solidity
function emergencyWithdrawToken(address token) external nonReentrant onlyRole(MANAGER_ROLE) nonZeroAddress(token);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to withdraw|


### emergencyWithdrawEther

Only callable by addresses with MANAGER_ROLE

*Emergency function to withdraw all ETH to the timelock*

**Note:**
throws: ZeroBalance if contract has no ETH balance


```solidity
function emergencyWithdrawEther() external nonReentrant onlyRole(MANAGER_ROLE);
```

### createRound

Creates a new investment round with specified parameters

*Only callable by DAO_ROLE when contract is not paused*

**Notes:**
- requires-role: DAO_ROLE

- security: Validates all parameters and checks token supply

- events-emits: {CreateRound} when round is created

- events-emits: {RoundStatusUpdated} when round status is set to PENDING

- throws: InvalidDuration if duration is outside allowed range

- throws: InvalidEthTarget if ethTarget is zero

- throws: InvalidTokenAllocation if tokenAlloc is zero

- throws: InvalidStartTime if start is in the past

- throws: InsufficientSupply if contract lacks required tokens


```solidity
function createRound(
    uint64 start,
    uint64 duration,
    uint256 ethTarget,
    uint256 tokenAlloc,
    uint64 vestingCliff,
    uint64 vestingDuration
) external onlyRole(DAO_ROLE) whenNotPaused returns (uint32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`start`|`uint64`|Timestamp when the round should start|
|`duration`|`uint64`|Length of the round in seconds|
|`ethTarget`|`uint256`|Target amount of ETH to raise in the round|
|`tokenAlloc`|`uint256`|Total number of tokens allocated for this round|
|`vestingCliff`|`uint64`|Duration in seconds before vesting begins|
|`vestingDuration`|`uint64`|Total duration of the vesting period in seconds|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|roundId The ID of the newly created round|


### activateRound

Activates a pending investment round

*Only callable by MANAGER_ROLE when contract is not paused*

**Notes:**
- requires-role: MANAGER_ROLE

- security: Ensures round exists and is in PENDING status

- security: Validates round timing constraints

- events-emits: {RoundStatusUpdated} when round status is set to ACTIVE

- modifiers: validRound, correctStatus(PENDING), whenNotPaused

- throws: RoundStartTimeNotReached if current time is before start time

- throws: RoundEndTimeReached if current time is after end time

- throws: InvalidRound if roundId doesn't exist

- throws: InvalidRoundStatus if round is not in PENDING status


```solidity
function activateRound(uint32 roundId)
    external
    onlyRole(MANAGER_ROLE)
    validRound(roundId)
    correctStatus(roundId, RoundStatus.PENDING)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the round to activate|


### addInvestorAllocation

Adds a new investor allocation to a specific investment round

*Only callable by MANAGER_ROLE when contract is not paused*

**Notes:**
- requires-role: MANAGER_ROLE

- security: Validates round status and allocation limits

- events-emits: {InvestorAllocated} when allocation is added

- modifiers: validRound, whenNotPaused

- throws: InvalidInvestor if investor address is zero

- throws: InvalidEthAmount if ethAmount is zero

- throws: InvalidTokenAmount if tokenAmount is zero

- throws: InvalidRoundStatus if round is completed or finalized

- throws: AllocationExists if investor already has an allocation

- throws: ExceedsRoundAllocation if allocation exceeds round limit


```solidity
function addInvestorAllocation(uint32 roundId, address investor, uint256 ethAmount, uint256 tokenAmount)
    external
    onlyRole(MANAGER_ROLE)
    validRound(roundId)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|
|`investor`|`address`|The address of the investor to allocate|
|`ethAmount`|`uint256`|Amount of ETH the investor is allowed to invest|
|`tokenAmount`|`uint256`|Amount of tokens the investor will receive|


### removeInvestorAllocation

Removes an investor's allocation from a specific investment round

*Only callable by MANAGER_ROLE when contract is not paused*

**Notes:**
- requires-role: MANAGER_ROLE

- security: Ensures investor has no active position before removal

- events-emits: {InvestorAllocationRemoved} when allocation is removed

- modifiers: validRound, whenNotPaused

- throws: InvalidInvestor if investor address is zero

- throws: InvalidRoundStatus if round is not in PENDING or ACTIVE status

- throws: NoAllocationExists if investor has no allocation

- throws: InvestorHasActivePosition if investor has already invested


```solidity
function removeInvestorAllocation(uint32 roundId, address investor)
    external
    onlyRole(MANAGER_ROLE)
    validRound(roundId)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|
|`investor`|`address`|The address of the investor whose allocation to remove|


### cancelInvestment

Allows an investor to cancel their investment in an active round

*Returns the invested ETH to the investor and updates round state*

**Notes:**
- security: Uses nonReentrant guard for ETH transfers

- security: Only allows cancellation during active round status

- events-emits: {CancelInvestment} when investment is cancelled

- modifiers: validRound, activeRound, nonReentrant

- throws: NoInvestment if caller has no active investment

- throws: InvalidRound if roundId doesn't exist

- throws: RoundNotActive if round is not in active status


```solidity
function cancelInvestment(uint32 roundId) external validRound(roundId) activeRound(roundId) nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|


### finalizeRound

Finalizes a completed investment round

*Deploys vesting contracts and distributes tokens to investors*

**Notes:**
- security: Uses nonReentrant guard for token transfers

- security: Only callable when contract is not paused

- security: Processes all investors in the round

- events-emits: {RoundFinalized} when round is finalized

- events-emits: {DeployVesting} for each vesting contract created

- events-emits: {RoundStatusUpdated} when status changes to FINALIZED

- modifiers: validRound, correctStatus(COMPLETED), nonReentrant, whenNotPaused

- throws: InvalidRound if roundId doesn't exist

- throws: InvalidRoundStatus if round is not in COMPLETED status


```solidity
function finalizeRound(uint32 roundId)
    external
    validRound(roundId)
    correctStatus(roundId, RoundStatus.COMPLETED)
    nonReentrant
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to finalize|


### cancelRound

Cancels an investment round and returns tokens to treasury

*Only callable by MANAGER_ROLE when contract is not paused*

**Notes:**
- requires-role: MANAGER_ROLE

- security: Ensures round can only be cancelled in PENDING or ACTIVE status

- security: Automatically returns allocated tokens to treasury

- events-emits: {RoundStatusUpdated} when status changes to CANCELLED

- events-emits: {RoundCancelled} when round is cancelled

- modifiers: validRound, whenNotPaused

- throws: InvalidRound if roundId doesn't exist

- throws: InvalidRoundStatus if round is not in PENDING or ACTIVE status


```solidity
function cancelRound(uint32 roundId) external validRound(roundId) onlyRole(MANAGER_ROLE) whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to cancel|


### claimRefund

Allows investors to claim ETH refunds from cancelled rounds

*Refunds the full invested amount and updates round state*

**Notes:**
- security: Uses nonReentrant guard for ETH transfers

- security: Ensures round is in CANCELLED status

- security: Updates investor position and round state atomically

- events-emits: {RefundClaimed} when refund is processed

- modifiers: validRound, nonReentrant

- throws: InvalidRound if roundId doesn't exist

- throws: RoundNotCancelled if round is not in CANCELLED status

- throws: NoRefundAvailable if caller has no refund to claim


```solidity
function claimRefund(uint32 roundId) external validRound(roundId) nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|


### upgradeTimelockRemaining

Calculates remaining time in the upgrade timelock period

*Returns 0 if no upgrade is pending or timelock has expired*

**Note:**
security: Helps track upgrade timelock status


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 Remaining time in seconds before upgrade can be executed|


### getRefundAmount

Gets the refundable amount for an investor in a cancelled round

*Returns 0 if round is not cancelled or investor has no position*

**Note:**
security: Only returns values for cancelled rounds


```solidity
function getRefundAmount(uint32 roundId, address investor) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|
|`investor`|`address`|The address of the investor to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 Amount of ETH available for refund|


### getInvestorDetails

Retrieves investment details for a specific investor in a round

*Returns allocation amounts, invested amount, and vesting contract address*


```solidity
function getInvestorDetails(uint32 roundId, address investor)
    external
    view
    returns (uint256 etherAmount, uint256 tokenAmount, uint256 invested, address vestingContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to query|
|`investor`|`address`|The address of the investor to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`etherAmount`|`uint256`|The maximum amount of ETH the investor can invest|
|`tokenAmount`|`uint256`|The amount of tokens allocated to the investor|
|`invested`|`uint256`|The amount of ETH already invested by the investor|
|`vestingContract`|`address`|The address of the investor's vesting contract|


### getEcosystemToken

Gets the address of the ecosystem token

*Provides access to the token contract address*


```solidity
function getEcosystemToken() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|address The address of the ecosystem token contract|


### getRoundInfo

Retrieves all information about a specific investment round

*Returns the full Round struct with all round parameters*


```solidity
function getRoundInfo(uint32 roundId) external view returns (Round memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Round`|Round Complete round information including status and allocations|


### getRoundInvestors

Gets the list of investors in a specific round

*Returns array of all investor addresses that participated*


```solidity
function getRoundInvestors(uint32 roundId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|address[] Array of investor addresses in the round|


### investEther

Processes ETH investment in a specific investment round

*Handles direct investment and fallback function investments*

**Notes:**
- security: Uses nonReentrant guard for ETH transfers

- security: Validates round status and investor allocation

- security: Enforces investment limits and round constraints

- events-emits: {Invest} when investment is processed

- events-emits: {RoundComplete} if round target is reached

- modifiers: validRound, activeRound, whenNotPaused, nonReentrant

- throws: RoundEnded if round end time has passed

- throws: RoundOversubscribed if maximum investor count reached

- throws: NoAllocation if sender has no allocation

- throws: AmountAllocationMismatch if sent ETH doesn't match remaining allocation


```solidity
function investEther(uint32 roundId)
    public
    payable
    validRound(roundId)
    activeRound(roundId)
    whenNotPaused
    nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|


### getCurrentRound

Gets the ID of the currently active investment round

*Iterates through all rounds to find one with ACTIVE status*

**Notes:**
- security: Returns max uint32 value as sentinel when no active round is found

- view-stability: Does not modify state


```solidity
function getCurrentRound() public view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|uint32 The ID of the active round, or type(uint32).max if no active round exists|


### _authorizeUpgrade

Authorizes and executes a contract upgrade after timelock period

*Internal function called by the UUPS upgrade mechanism*

**Notes:**
- requires-role: UPGRADER_ROLE

- security: Enforces timelock period before upgrade execution

- security: Verifies upgrade was properly scheduled

- security: Checks implementation address matches scheduled upgrade

- events-emits: {Upgraded} when upgrade is executed

- throws: UpgradeNotScheduled if no upgrade was scheduled

- throws: ImplementationMismatch if implementation doesn't match scheduled

- throws: UpgradeTimelockActive if timelock period hasn't elapsed


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation contract|


### _updateRoundStatus

Updates the status of an investment round

*Internal function to manage round state transitions*

**Notes:**
- security: Ensures status transitions are only forward-moving

- events-emits: {RoundStatusUpdated} when status is changed

- throws: InvalidStatusTransition if attempting to move to a previous status


```solidity
function _updateRoundStatus(uint32 roundId, RoundStatus newStatus) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round to update|
|`newStatus`|`RoundStatus`|The new status to set for the round|


### _processInvestment

Processes an investment into a round

*Handles investment accounting and status updates*

**Notes:**
- security: Updates investor tracking and round state atomically

- events-emits: {Invest} when investment is processed

- events-emits: {RoundComplete} if round target is reached


```solidity
function _processInvestment(uint32 roundId, address investor, uint256 amount) private;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|
|`investor`|`address`|The address of the investor|
|`amount`|`uint256`|The amount of ETH being invested|


### _removeInvestor

Removes an investor from a round's tracking array

*Uses optimized removal pattern to maintain array integrity*

**Note:**
security: Maintains array consistency with gas-efficient removal


```solidity
function _removeInvestor(uint32 roundId, address investor) private;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The ID of the investment round|
|`investor`|`address`|The address of the investor to remove|


### _deployVestingContract

Deploys a new vesting contract for an investor

*Creates and configures a vesting schedule for allocated tokens*

**Notes:**
- security: Sets up vesting parameters based on round configuration

- events-emits: {DeployVesting} when vesting contract is created


```solidity
function _deployVestingContract(address investor, uint256 allocation, uint32 roundId) private returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The address of the investor who will receive the tokens|
|`allocation`|`uint256`|The amount of tokens to be vested|
|`roundId`|`uint32`|The ID of the investment round|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|address The address of the newly deployed vesting contract|


