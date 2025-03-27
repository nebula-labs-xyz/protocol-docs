# Ecosystem
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/Ecosystem.sol)

**Inherits:**
[IECOSYSTEM](/contracts/interfaces/IEcosystem.sol/interface.IECOSYSTEM.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Ecosystem contract handles airdrops, rewards, burning, and partnerships

*Implements a secure and upgradeable DAO ecosystem*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## State Variables
### BURNER_ROLE
*AccessControl Burner Role*


```solidity
bytes32 internal constant BURNER_ROLE = keccak256("BURNER_ROLE");
```


### PAUSER_ROLE
*AccessControl Pauser Role*


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### UPGRADER_ROLE
*AccessControl Upgrader Role*


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### REWARDER_ROLE
*AccessControl Rewarder Role*


```solidity
bytes32 internal constant REWARDER_ROLE = keccak256("REWARDER_ROLE");
```


### MANAGER_ROLE
*AccessControl Manager Role*


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### MIN_VESTING_DURATION
*Minimum required vesting duration (30 days)*


```solidity
uint256 internal constant MIN_VESTING_DURATION = 30 days;
```


### MAX_VESTING_DURATION
*Maximum allowed vesting duration (10 years)*


```solidity
uint256 internal constant MAX_VESTING_DURATION = 3650 days;
```


### UPGRADE_TIMELOCK_DURATION
*Upgrade timelock duration (3 days)*


```solidity
uint256 private constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### tokenInstance
*governance token instance*


```solidity
ILENDEFI internal tokenInstance;
```


### rewardSupply
*starting reward supply*


```solidity
uint256 public rewardSupply;
```


### maxReward
*maximal one time reward amount*


```solidity
uint256 public maxReward;
```


### issuedReward
*issued reward*


```solidity
uint256 public issuedReward;
```


### burnedAmount
*burned reward amount (tracked separately for transparency)*


```solidity
uint256 public burnedAmount;
```


### maxBurn
*maximum one time burn amount*


```solidity
uint256 public maxBurn;
```


### airdropSupply
*starting airdrop supply*


```solidity
uint256 public airdropSupply;
```


### issuedAirDrop
*total amount airdropped so far*


```solidity
uint256 public issuedAirDrop;
```


### partnershipSupply
*starting partnership supply*


```solidity
uint256 public partnershipSupply;
```


### issuedPartnership
*partnership tokens issued so far*


```solidity
uint256 public issuedPartnership;
```


### version
*number of UUPS upgrades - packed with other variables to save gas*


```solidity
uint32 public version;
```


### timelock
*timelock address for partner vesting cancellations*


```solidity
address public timelock;
```


### vestingContracts
*Addresses of vesting contracts issued to partners*


```solidity
mapping(address partner => address vesting) public vestingContracts;
```


### pendingUpgrade
*Pending upgrade information*


```solidity
UpgradeRequest public pendingUpgrade;
```


### __gap

```solidity
uint256[16] private __gap;
```


## Functions
### nonZeroAmount

*Modifier to check for non-zero amounts*


```solidity
modifier nonZeroAmount(uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount to validate|


### nonZeroAddress

*Modifier to check for non-zero addresses*


```solidity
modifier nonZeroAddress(address addr);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|The address to validate|


### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### receive

*Prevents receiving Ether. This contract doesn't handle ETH.*


```solidity
receive() external payable;
```

### initialize

Initializes the Ecosystem contract with core dependencies and configurations

*Sets up the contract with initial token allocations, roles, and limits*

**Notes:**
- security: Uses initializer modifier to prevent multiple initializations

- security-roles: Grants the following roles:
- DEFAULT_ADMIN_ROLE to timelock
- MANAGER_ROLE to timelock
- PAUSER_ROLE to timelock
- UPGRADER_ROLE to both timelock and multisig

- allocation: Configures token allocations as follows:
- 26% for rewards
- 10% for airdrops
- 8% for partnerships

- limits: Sets initial limits:
- maxReward: 0.1% of reward supply
- maxBurn: 2% of reward supply

- events-emits: {Initialized} when initialization is complete

- throws: ZeroAddressDetected if any input address is zero


```solidity
function initialize(address token, address timelockAddr, address multisig) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the LENDEFI token contract|
|`timelockAddr`|`address`|Address of the timelock contract that will have admin control|
|`multisig`|`address`|Address of the multisig wallet that will have upgrade rights|


### pause

Pauses all contract operations.

*Pause contract.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Paused} event from PausableUpgradeable


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Resumes all contract operations.

*Unpause contract.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Unpaused} event from PausableUpgradeable


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### scheduleUpgrade

Only callable by an address with UPGRADER_ROLE

*Schedules an upgrade to a new implementation*


```solidity
function scheduleUpgrade(address newImplementation)
    external
    onlyRole(UPGRADER_ROLE)
    nonZeroAddress(newImplementation);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


### emergencyWithdrawToken

Only callable by addresses with MANAGER_ROLE

Always sends all available tokens to the timelock controller

*Emergency function to withdraw all tokens to the timelock*

**Notes:**
- throws: ZeroAddress if token address is zero

- throws: ZeroBalance if contract has no token balance


```solidity
function emergencyWithdrawToken(address token) external nonReentrant onlyRole(MANAGER_ROLE) nonZeroAddress(token);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to withdraw|


### airdrop

Distributes a specified amount of tokens to each address in the recipients array.

*Performs an airdrop to a list of recipients.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: Amount must be at least 1 ether

- requires: Total airdropped amount must not exceed the airdrop supply

- requires: Number of recipients must not exceed 4000 to avoid gas limit issues

- events-emits: {AirDrop} event

- throws: InvalidAmount if the amount is less than 1 ether

- throws: AirdropSupplyLimit if the total airdropped amount exceeds the airdrop supply

- throws: GasLimit if the number of recipients exceeds 4000


```solidity
function airdrop(address[] calldata recipients, uint256 amount)
    external
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|An array of addresses to receive the airdrop.|
|`amount`|`uint256`|The amount of tokens to be airdropped to each address.|


### reward

Distributes a specified amount of tokens to a beneficiary address.

*Reward functionality for the Nebula Protocol.*

**Notes:**
- requires-role: REWARDER_ROLE

- requires: Contract must not be paused

- requires: Amount must be greater than 0

- requires: Amount must not exceed the maximum reward limit

- requires: Total rewarded amount must not exceed the reward supply

- events-emits: {Reward} event

- throws: InvalidAmount if the amount is 0

- throws: InvalidAddress if the recipient address is 0

- throws: RewardLimit if the amount exceeds the maximum reward limit

- throws: RewardSupplyLimit if the total rewarded amount exceeds the reward supply


```solidity
function reward(address to, uint256 amount)
    external
    nonReentrant
    whenNotPaused
    onlyRole(REWARDER_ROLE)
    nonZeroAmount(amount)
    nonZeroAddress(to);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address that will receive the reward.|
|`amount`|`uint256`|The amount of tokens to be rewarded.|


### burn

Burns a specified amount of tokens from the reward supply.

*Enables burn functionality for the DAO.*

**Notes:**
- requires-role: BURNER_ROLE

- requires: Contract must not be paused

- requires: Amount must be greater than 0

- requires: Amount must not exceed the maximum burn limit

- requires: Total burned amount must not exceed the reward supply

- events-emits: {Burn} event

- throws: InvalidAmount if the amount is 0

- throws: MaxBurnLimit if the amount exceeds the maximum burn limit

- throws: BurnSupplyLimit if the amount exceeds available supply


```solidity
function burn(uint256 amount) external nonReentrant whenNotPaused nonZeroAmount(amount) onlyRole(BURNER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of tokens to be burned.|


### addPartner

Adds a new partner by creating a cancellable vesting contract and transferring the specified amount of tokens.

*Creates and funds a new vesting contract for a new partner.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: Partner address must not be zero and must be a valid contract or EOA

- requires: Amount must be between 100 ether and half of the partnership supply

- requires: Duration must be within reasonable bounds

- requires: Total issued partnership tokens must not exceed the partnership supply

- events-emits: {AddPartner} event

- throws: InvalidAddress if the partner address is zero

- throws: PartnerExists if the partner already has a vesting contract

- throws: InvalidAmount if the amount is not within the valid range

- throws: InvalidVestingSchedule if duration is not within reasonable bounds

- throws: AmountExceedsSupply if the total issued partnership tokens exceed the partnership supply


```solidity
function addPartner(address partner, uint256 amount, uint256 cliff, uint256 duration)
    external
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE)
    nonZeroAddress(partner);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner to receive the vesting contract.|
|`amount`|`uint256`|The amount of tokens to be vested.|
|`cliff`|`uint256`|The duration in seconds of the cliff period.|
|`duration`|`uint256`|The duration in seconds of the vesting period.|


### cancelPartnership

This can only be called by the timelock (governance)

*Cancels a partner vesting contract*

**Notes:**
- requires: The caller must be the timelock

- requires: The partner must have a valid vesting contract

- emits: CancelPartnership event on successful cancellation

- throws: CallerNotAllowed if the caller is not the timelock

- throws: InvalidAddress if the partner has no vesting contract


```solidity
function cancelPartnership(address partner) external nonZeroAddress(partner) onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner whose vesting should be cancelled|


### cancelUpgrade

Cancels a previously scheduled upgrade

*Only callable by addresses with UPGRADER_ROLE*


```solidity
function cancelUpgrade() external onlyRole(UPGRADER_ROLE);
```

### updateMaxReward

Allows updating the maximum reward that can be issued in a single transaction.

*Updates the maximum one-time reward amount.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: New max reward must be greater than 0

- requires: New max reward must not exceed 5% of remaining reward supply

- events-emits: {MaxRewardUpdated} event

- throws: InvalidAmount if the amount is 0

- throws: ExcessiveMaxValue if the amount exceeds 5% of remaining reward supply


```solidity
function updateMaxReward(uint256 newMaxReward)
    external
    whenNotPaused
    onlyRole(MANAGER_ROLE)
    nonZeroAmount(newMaxReward);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxReward`|`uint256`|The new maximum reward amount.|


### updateMaxBurn

Allows updating the maximum amount that can be burned in a single transaction.

*Updates the maximum one-time burn amount.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: New max burn must be greater than 0

- requires: New max burn must not exceed 10% of remaining reward supply

- events-emits: {MaxBurnUpdated} event

- throws: InvalidAmount if the amount is 0

- throws: ExcessiveMaxValue if the amount exceeds 10% of remaining reward supply


```solidity
function updateMaxBurn(uint256 newMaxBurn) external whenNotPaused onlyRole(MANAGER_ROLE) nonZeroAmount(newMaxBurn);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxBurn`|`uint256`|The new maximum burn amount.|


### upgradeTimelockRemaining

*Returns the remaining time before a scheduled upgrade can be executed*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The time remaining in seconds, or 0 if no upgrade is scheduled or timelock has passed|


### availableRewardSupply

*Returns the effective available reward supply considering burns.*


```solidity
function availableRewardSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The current available reward supply.|


### availableAirdropSupply

*Returns the effective available airdrop supply.*


```solidity
function availableAirdropSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The current available airdrop supply.|


### availablePartnershipSupply

*Returns the effective available partnership supply.*


```solidity
function availablePartnershipSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The current available partnership supply.|


### _authorizeUpgrade

*Authorizes an upgrade to a new implementation with timelock enforcement*


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|The address of the new implementation contract|


