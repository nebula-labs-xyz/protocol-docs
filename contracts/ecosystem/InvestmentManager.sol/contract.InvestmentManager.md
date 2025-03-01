# InvestmentManager
[Git Source](https://github.com/nebula-labs-xyz/lendefi-dao/blob/07f5cb7369219dbffd648091ffbddb6d70a0157c/contracts/ecosystem/InvestmentManager.sol)

## Overview

The InvestmentManager contract is a sophisticated investment round management system for the Lendefi DAO. It facilitates token distribution through structured investment rounds with custom vesting parameters. The contract exhibits a well-designed state machine for round lifecycle management and implements comprehensive security measures.

## Architecture Design

### Core Components

1. **Round State Machine**
   - Well-defined round lifecycle progression (PENDING → ACTIVE → COMPLETED → FINALIZED)
   - Alternative CANCELLED state with refund capabilities
   - Clear status transition rules with validation

2. **Access Control System**
   - Four distinct roles with tailored permissions:
     - `MANAGER_ROLE`: Administrative operations
     - `PAUSER_ROLE`: Emergency controls
     - `UPGRADER_ROLE`: Smart contract upgrades
     - `DAO_ROLE`: Governance operations (assigned to timelock)

3. **Investor Management**
   - Allocation tracking with two-step participation model
   - Individual investment position tracking
   - Vesting contract deployment for finalized investments
   - Refund capabilities for cancelled rounds

4. **Vesting Integration**
   - Custom `InvestorVesting` contract deployment per investor
   - Round-specific vesting parameters (cliff and duration)
   - Direct token transfers to vesting contracts

## Technical Implementation

### Security Features

1. **Transaction Protection**
   - Reentrancy guards on all fund-moving functions
   - State updates before external calls (ETH transfers)
   - Robust modifier usage to validate conditions

2. **Fund Management**
   - Safe ETH transfers using `Address.sendValue`
   - SafeERC20 for token transfers
   - Clear accounting of ETH and token movements

3. **Input Validation**
   - Comprehensive parameter validation
   - Round boundary checking (min/max duration)
   - Allocation limits enforcement
   - Zero-address checks

4. **Emergency Controls**
   - Pausability for global emergency stop
   - Round cancellation with refund mechanism
   - Individual investment cancellation options

### Gas Optimization

1. **Storage Access**
   - Strategic use of storage vs memory variables
   - Caching of repeatedly accessed values
   - Efficient array operations

2. **Loop Optimization**
   - Unchecked increment for loop counters
   - Length caching before loops
   - Early exit patterns

3. **Array Management**
   - Efficient investor removal algorithm to minimize gas
   - Bounded arrays (MAX_INVESTORS_PER_ROUND = 50)

## Strengths

1. **Well-Structured Design**
   - Clean separation of concerns
   - Logical function organization
   - Clear state transitions

2. **Comprehensive Documentation**
   - Excellent NatSpec comments
   - Detailed function descriptions
   - Security considerations noted
   - Error explanations

3. **Robust Safety Measures**
   - Multiple validation layers
   - Clear error messages
   - Fail-fast design philosophy

4. **Upgradeability**
   - UUPS pattern implementation
   - Version tracking
   - Storage gap for future extensions

## Potential Concerns

1. **Centralization Risks**
   - Guardian role has significant power (multiple critical roles)
   - No time-locks on sensitive operations
   - Single-signature administration

2. **Gas Limitations**
   - Finalizing rounds with many investors might approach block gas limits
   - Linear search for investor removal is O(n)

3. **Edge Cases**
   - Round timing edge cases may exist around block timestamps
   - No mechanism to extend rounds if target not met

4. **Economic Design**
   - Fixed investor limit per round (50) may be restrictive for larger rounds
   - No dynamic pricing or auction mechanisms


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

```solidity
uint256 private constant MAX_INVESTORS_PER_ROUND = 50;
```


### MIN_ROUND_DURATION

```solidity
uint256 private constant MIN_ROUND_DURATION = 5 days;
```


### MAX_ROUND_DURATION

```solidity
uint256 private constant MAX_ROUND_DURATION = 90 days;
```


### PAUSER_ROLE

```solidity
bytes32 private constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE

```solidity
bytes32 private constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE

```solidity
bytes32 private constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### DAO_ROLE

```solidity
bytes32 private constant DAO_ROLE = keccak256("DAO_ROLE");
```


### ecosystemToken

```solidity
ILENDEFI public ecosystemToken;
```


### timelock

```solidity
address public timelock;
```


### treasury

```solidity
address public treasury;
```


### supply

```solidity
uint256 public supply;
```


### version

```solidity
uint32 public version;
```


### rounds

```solidity
Round[] public rounds;
```


### investors

```solidity
mapping(uint32 => address[]) private investors;
```


### investorPositions

```solidity
mapping(uint32 => mapping(address => uint256)) private investorPositions;
```


### vestingContracts

```solidity
mapping(uint32 => mapping(address => address)) private vestingContracts;
```


### investorAllocations

```solidity
mapping(uint32 => mapping(address => Allocation)) private investorAllocations;
```


### totalRoundAllocations

```solidity
mapping(uint32 => uint256) private totalRoundAllocations;
```


### __gap

```solidity
uint256[45] private __gap;
```


## Functions
### validRound


```solidity
modifier validRound(uint32 roundId);
```

### activeRound


```solidity
modifier activeRound(uint32 roundId);
```

### correctStatus


```solidity
modifier correctStatus(uint32 roundId, RoundStatus requiredStatus);
```

### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### receive

Fallback function to handle direct ETH transfers

*Automatically invests in the current active round*

*Process:
1. Gets current active round number
2. Validates active round exists
3. Forwards ETH to investEther function*

*Requirements:
- At least one round must be active
- Sent ETH must match remaining allocation
- Sender must have valid allocation*

**Notes:**
- throws: NO_ACTIVE_ROUND if no round is currently active

- throws: AMOUNT_ALLOCATION_MISMATCH if sent amount doesn't match allocation

- throws: NO_ALLOCATION if sender has no allocation

- emits: Invest when investment is processed

- security: Forwards to investEther which has reentrancy protection

- security: Validates round status before processing


```solidity
receive() external payable;
```

### initialize

Initializes the Investment Manager contract with core dependencies

*Sets up initial roles and contract references*

*Initialization sequence:
1. Initializes security modules:
- Pausable functionality
- Access control system
- UUPS upgrade mechanism
- Reentrancy protection
2. Validates addresses
3. Sets up roles and permissions
4. Initializes contract references
5. Sets initial version*

**Notes:**
- throws: ZERO_ADDRESS_DETECTED if any parameter is zero address

- security: Can only be called once due to initializer modifier

- security: Sets up critical security features first

- security: Validates all addresses before use

- emits: Initialized when setup is complete


```solidity
function initialize(address token, address timelock_, address treasury_, address guardian) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ecosystem token contract|
|`timelock_`|`address`|Address of the timelock contract for governance|
|`treasury_`|`address`|Address of the treasury contract|
|`guardian`|`address`|Address of the initial guardian who receives admin roles|


### pause

Pauses all contract operations

*Only callable by accounts with PAUSER_ROLE*

*When paused:
- No new rounds can be created
- No investments can be made
- No rounds can be activated
- No rounds can be finalized
- Cancellations and refunds remain active for security*

**Notes:**
- throws: Unauthorized if caller lacks PAUSER_ROLE

- emits: Paused event with caller's address

- security: Inherits OpenZeppelin's Pausable implementation

- security: Role-based access control via PAUSER_ROLE

- security: Emergency stop mechanism for contract operations


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses all contract operations

*Only callable by accounts with PAUSER_ROLE*

*After unpausing:
- Round creation becomes available
- Investments can be processed
- Round activation allowed
- Round finalization enabled
- Normal contract operations resume*

**Notes:**
- throws: Unauthorized if caller lacks PAUSER_ROLE

- emits: Unpaused event with caller's address

- security: Inherits OpenZeppelin's Pausable implementation

- security: Role-based access control via PAUSER_ROLE

- security: Restores normal contract functionality


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### createRound

Creates a new investment round with custom vesting parameters

*Only callable by accounts with DAO_ROLE when contract is not paused*

**Notes:**
- throws: INVALID_DURATION if round duration is outside allowed range

- throws: INVALID_ETH_TARGET if ethTarget is 0

- throws: INVALID_TOKEN_ALLOCATION if tokenAlloc is 0

- throws: INVALID_START_TIME if start is in the past

- throws: INVALID_VESTING_PARAMETERS if vesting parameters are outside allowed range

- throws: INSUFFICIENT_SUPPLY if contract doesn't have enough tokens

- emits: CreateRound when round is created

- emits: RoundStatusUpdated when round status is set to PENDING


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
|`start`|`uint64`|The timestamp when the round starts|
|`duration`|`uint64`|The duration of the round in seconds|
|`ethTarget`|`uint256`|The target amount of ETH to raise|
|`tokenAlloc`|`uint256`|The amount of tokens allocated for the round|
|`vestingCliff`|`uint64`|The cliff period in seconds before vesting begins|
|`vestingDuration`|`uint64`|The total duration of the vesting period in seconds|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|roundId The identifier of the newly created round|


### activateRound

Activates a pending investment round

*Only callable by accounts with MANAGER_ROLE*

*Requires:
- Round exists (validRound modifier)
- Round is in PENDING status (correctStatus modifier)
- Contract is not paused (whenNotPaused modifier)
- Current time is within round's time window*

**Notes:**
- throws: ROUND_START_TIME_NOT_REACHED if current time is before round start

- throws: ROUND_END_TIME_REACHED if current time is after round end

- throws: INVALID_ROUND if roundId is invalid

- throws: INVALID_ROUND_STATUS if round is not in PENDING status

- emits: RoundStatusUpdated when round status changes to ACTIVE


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
|`roundId`|`uint32`|The identifier of the round to activate|


### addInvestorAllocation

Adds or updates token allocation for an investor in a specific round

*Only callable by accounts with MANAGER_ROLE when contract is not paused*

*Requires:
- Round exists (validRound modifier)
- Round is not completed
- Valid investor address
- Non-zero amounts
- Total allocation within round limits*

**Notes:**
- throws: INVALID_INVESTOR if investor address is zero

- throws: INVALID_ETH_AMOUNT if ethAmount is zero

- throws: INVALID_TOKEN_AMOUNT if tokenAmount is zero

- throws: INVALID_ROUND_STATUS if round is completed or cancelled

- throws: EXCEEDS_ROUND_ALLOCATION if new total exceeds round allocation

- emits: InvestorAllocated when allocation is successfully added/updated


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
|`roundId`|`uint32`|The identifier of the investment round|
|`investor`|`address`|The address of the investor receiving the allocation|
|`ethAmount`|`uint256`|The amount of ETH being allocated|
|`tokenAmount`|`uint256`|The amount of tokens being allocated|


### removeInvestorAllocation

Removes an investor's allocation from a specific investment round

*Only callable by accounts with MANAGER_ROLE when contract is not paused*

*Requirements:
- Round exists (validRound modifier)
- Round is not completed
- Valid investor address
- Allocation exists for investor
- Investor has not made any investments yet*

*State Changes:
- Zeros out investor's allocation
- Decrements total round allocation*

**Notes:**
- throws: INVALID_INVESTOR if investor address is zero

- throws: INVALID_ROUND_STATUS if round is completed or cancelled

- throws: NO_ALLOCATION_EXISTS if investor has no allocation

- throws: INVESTOR_HAS_ACTIVE_POSITION if investor has already invested

- emits: InvestorAllocationRemoved when allocation is successfully removed

- security: Uses validRound modifier to prevent invalid round access

- security: Updates state before event emission

- security: Maintains accurate total allocation tracking


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
|`roundId`|`uint32`|The identifier of the investment round|
|`investor`|`address`|The address of the investor whose allocation to remove|


### cancelInvestment

Allows investors to cancel their investment and receive a refund

*Processes investment cancellation and ETH refund*

*Requirements:
- Round exists (validRound modifier)
- Round is active (activeRound modifier)
- Protected against reentrancy (nonReentrant modifier)
- Caller must have an active investment*

*State Changes:
- Sets investor's position to 0
- Decrements round's total ETH invested
- Decrements round's participant count
- Removes investor from round's investor list*

**Notes:**
- throws: NO_INVESTMENT if caller has no active investment

- emits: CancelInvestment when investment is successfully cancelled

- security: Uses nonReentrant modifier to prevent reentrancy attacks

- security: Uses Address.sendValue for safe ETH transfer

- security: Updates state before external calls


```solidity
function cancelInvestment(uint32 roundId) external validRound(roundId) activeRound(roundId) nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the round to cancel investment from|


### finalizeRound

Finalizes an investment round and deploys vesting contracts for investors

*Processes token distribution and transfers ETH to treasury*

*Requirements:
- Round exists (validRound modifier)
- Protected against reentrancy (nonReentrant modifier)
- Contract not paused (whenNotPaused modifier)
- Round must be in COMPLETED status*

*Process:
1. Validates round status
2. Iterates through all round investors
3. For each valid investment:
- Calculates token allocation
- Deploys vesting contract
- Transfers tokens to vesting contract
- Updates round token distribution
4. Updates round status to FINALIZED
5. Transfers accumulated ETH to treasury*

**Notes:**
- throws: INVALID_ROUND_STATUS if round is not in COMPLETED status

- throws: Address: insufficient balance if ETH transfer to treasury fails

- emits: RoundFinalized with final round statistics

- security: Uses nonReentrant modifier to prevent reentrancy

- security: Uses SafeERC20 for token transfers

- security: Uses unchecked block for gas optimization in loop counter


```solidity
function finalizeRound(uint32 roundId) external validRound(roundId) nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the round to finalize|


### cancelRound

Cancels an investment round and returns tokens to treasury

*Only callable by accounts with MANAGER_ROLE when contract is not paused*

*Requirements:
- Round exists (validRound modifier)
- Round is in PENDING or ACTIVE status
- Caller has MANAGER_ROLE
- Contract is not paused*

*State Changes:
- Updates round status to CANCELLED
- Decrements total token supply
- Transfers round's token allocation back to treasury*

**Notes:**
- throws: INVALID_STATUS_TRANSITION if round is not in PENDING or ACTIVE status

- throws: INVALID_ROUND if roundId is invalid

- emits: RoundCancelled when round is successfully cancelled

- security: Uses SafeERC20 for token transfers


```solidity
function cancelRound(uint32 roundId) external validRound(roundId) onlyRole(MANAGER_ROLE) whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the round to cancel|


### claimRefund

Allows investors to claim their refund after round cancellation

*Processes refund claims and updates round state*

*Requirements:
- Round exists (validRound modifier)
- Protected against reentrancy (nonReentrant modifier)
- Round must be in CANCELLED status
- Caller must have refund available*

*State Changes:
- Sets investor's position to 0
- Decrements round's total ETH invested
- Decrements round's participant count if > 0
- Removes investor from round's investor list*

**Notes:**
- throws: ROUND_NOT_CANCELLED if round is not in CANCELLED status

- throws: NO_REFUND_AVAILABLE if caller has no refund to claim

- throws: INVALID_ROUND if roundId is invalid

- emits: RefundClaimed when refund is successfully processed

- security: Uses nonReentrant modifier to prevent reentrancy attacks

- security: Uses Address.sendValue for safe ETH transfer

- security: Updates state before external calls


```solidity
function claimRefund(uint32 roundId) external validRound(roundId) nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the round to claim refund from|


### getRefundAmount

Gets the refund amount available for an investor in a cancelled round

*Returns 0 if round is not cancelled, otherwise returns investor's position*

**Notes:**
- security: No state modifications

- security: Safe to call by anyone

- security: Returns 0 for non-cancelled rounds or non-existent positions


```solidity
function getRefundAmount(uint32 roundId, address investor) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the investment round|
|`investor`|`address`|The address of the investor to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount The amount of ETH available for refund|


### getInvestorDetails

Gets the investment details for an investor in a specific round


```solidity
function getInvestorDetails(uint32 roundId, address investor)
    external
    view
    returns (uint256 etherAmount, uint256 tokenAmount, uint256 invested, address vestingContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The round identifier|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`etherAmount`|`uint256`|The amount of ETH allocated|
|`tokenAmount`|`uint256`|The amount of tokens allocated|
|`invested`|`uint256`|The amount already invested|
|`vestingContract`|`address`|The address of the vesting contract (if deployed)|


### getRoundInfo

Retrieves detailed information about a specific investment round

*Returns complete Round struct with all round parameters and current state*

*Round struct contains:
- etherTarget: Target ETH amount for the round
- etherInvested: Current ETH amount invested
- tokenAllocation: Total tokens allocated for round
- tokenDistributed: Amount of tokens distributed
- startTime: Round start timestamp
- endTime: Round end timestamp
- vestingCliff: Vesting cliff period
- vestingDuration: Total vesting duration
- participants: Current number of investors
- status: Current round status*

**Notes:**
- security: View function - no state modifications

- security: Safe to call by anyone

- security: Returns full struct copy - higher gas cost for large data


```solidity
function getRoundInfo(uint32 roundId) external view returns (Round memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the round to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Round`|Round struct containing all round details|


### getRoundInvestors

Gets the complete list of investors for a specific investment round

*Returns array of all investor addresses that participated in the round*

*Important considerations:
- Returns full array copy - gas cost scales with number of investors
- Array includes all historical investors, even those who cancelled
- Maximum size limited by MAX_INVESTORS_PER_ROUND (50)
- Order of addresses matches investment chronology*

**Notes:**
- security: View function - no state modifications

- security: Safe to call by anyone

- security: Returns empty array for invalid roundId

- security: Memory array bounded by MAX_INVESTORS_PER_ROUND


```solidity
function getRoundInvestors(uint32 roundId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the investment round to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of investor addresses for the specified round|


### investEther

Allows investors to participate in the current round using ETH

*Processes ETH investments for allocated participants*

*Requirements:
- Round exists (validRound modifier)
- Round is active (activeRound modifier)
- Contract not paused (whenNotPaused modifier)
- Protected against reentrancy (nonReentrant modifier)
- Round not ended
- Round not oversubscribed
- Investor has allocation
- Exact remaining allocation amount sent*

**Notes:**
- throws: ROUND_ENDED if round end time has passed

- throws: ROUND_OVERSUBSCRIBED if participant limit reached

- throws: NO_ALLOCATION if sender has no allocation

- throws: AMOUNT_ALLOCATION_MISMATCH if sent amount doesn't match remaining allocation

- emits: Invest when investment is processed successfully

- emits: RoundComplete when round reaches target after investment


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
|`roundId`|`uint32`|The identifier of the round to invest in|


### getCurrentRound

Gets the first active round number

*Iterates through rounds array to find first ACTIVE round*

*Returns type(uint32).max if no active round exists*

*Gas usage increases linearly with number of rounds*

*No state modifications - pure view function*

**Notes:**
- security: Safe to call by anyone

- security: No state modifications

- security: Returns max uint32 instead of reverting when no active round


```solidity
function getCurrentRound() public view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|roundId The first active round number, or type(uint32).max if none found|


### _updateRoundStatus

Updates the status of an investment round

*Internal function to manage round status transitions*

*Requirements:
- New status must be higher than current status
- Status transitions are one-way only
- Valid status progression:
PENDING -> ACTIVE -> COMPLETED -> FINALIZED
PENDING/ACTIVE -> CANCELLED*

**Notes:**
- throws: INVALID_STATUS_TRANSITION if attempting invalid status change

- emits: RoundStatusUpdated when status is successfully changed

- security: Enforces unidirectional status transitions

- security: Uses uint8 casting for safe status comparisons


```solidity
function _updateRoundStatus(uint32 roundId, RoundStatus newStatus) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the round to update|
|`newStatus`|`RoundStatus`|The new status to set for the round|


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


### _processInvestment

Processes an investment from either ETH or WETH

*Internal function to handle investment processing and state updates*

*State Changes:
- Adds investor to round's investor list if first investment
- Increments round's participant count for new investors
- Updates investor's position with investment amount
- Updates round's total ETH invested
- Updates round status if target reached*

**Notes:**
- emits: Invest when investment is processed

- emits: RoundComplete if investment reaches round target

- security: Updates all state before emitting events

- security: Handles first-time investor tracking

- security: Safe arithmetic operations via Solidity 0.8+


```solidity
function _processInvestment(uint32 roundId, address investor, uint256 amount) private;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the investment round|
|`investor`|`address`|The address of the investor making the investment|
|`amount`|`uint256`|The amount of ETH/WETH being invested|


### _removeInvestor

Removes an investor from a round's investor list

*Internal function using gas-optimized array manipulation*

*Algorithm:
1. Locates investor in round's investor array
2. If found and not last element:
- Moves last element to found position
- Pops last element
3. If found and last element:
- Simply pops last element*

*Gas Optimizations:
- Uses unchecked increment for loop counter
- Minimizes storage reads with length caching
- Uses efficient array pop over delete*

**Notes:**
- security: No return value - silently completes if investor not found

- security: Storage array modification only - no external calls

- security: Safe array operations via Solidity 0.8+ bounds checking


```solidity
function _removeInvestor(uint32 roundId, address investor) private;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the investment round|
|`investor`|`address`|The address of the investor to remove|


### _setupRoles

Sets up initial roles and permissions for contract operation

*Internal function called during initialization*

*Role assignments:
- Guardian receives:
- DEFAULT_ADMIN_ROLE
- MANAGER_ROLE
- PAUSER_ROLE
- UPGRADER_ROLE
- Timelock receives:
- DAO_ROLE*

**Notes:**
- security: Uses OpenZeppelin AccessControl

- security: Critical for establishing permission hierarchy

- security: Only called once during initialization


```solidity
function _setupRoles(address guardian, address timelock_) private;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|Address receiving admin and operational roles|
|`timelock_`|`address`|Address receiving governance role|


### _initializeContracts

Initializes core contract references and dependencies

*Internal function called during contract initialization*

*Sets up:
- Ecosystem token interface
- Timelock contract reference
- Treasury contract reference*

**Notes:**
- security: Called only once during initialization

- security: Addresses already validated before call

- security: Critical for contract functionality


```solidity
function _initializeContracts(address token, address timelock_, address treasury_) private;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the ecosystem token contract|
|`timelock_`|`address`|Address of the timelock contract for governance|
|`treasury_`|`address`|Address of the treasury contract|


### _deployVestingContract

Deploys vesting contract for an investor with round-specific parameters

*Internal function to create and configure vesting contracts*

*Process:
1. Retrieves round parameters from storage
2. Creates new InvestorVesting contract instance
3. Configures with:
- Ecosystem token address
- Investor address as beneficiary
- Cliff start time (current time + round cliff)
- Round-specific vesting duration
4. Emits deployment event*

**Notes:**
- security: Uses safe type casting for timestamps

- security: Emits event before returning for complete audit trail

- emits: DeployVesting with contract details and allocation


```solidity
function _deployVestingContract(address investor, uint256 allocation, uint32 roundId) private returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The address of the beneficiary for the vesting contract|
|`allocation`|`uint256`|The amount of tokens to be vested|
|`roundId`|`uint32`|The identifier of the investment round|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|address The address of the newly deployed vesting contract|


