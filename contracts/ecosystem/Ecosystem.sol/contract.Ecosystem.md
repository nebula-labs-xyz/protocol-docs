# Ecosystem
[Git Source](https://github.com/nebula-labs-xyz/lendefi-dao/blob/05bc99b130950ac71cca48b96856e5ce17a94027/contracts/ecosystem/Ecosystem.sol)


# Comprehensive Review of the Lendefi DAO Ecosystem Contract

## Overview

The Lendefi DAO Ecosystem contract is a sophisticated governance contract responsible for managing token distribution, rewards, burning, and partner vesting within the Lendefi ecosystem. This contract is implemented using the UUPS upgradeable pattern, providing flexibility for future improvements while maintaining a strong security posture through a comprehensive role-based access control system.

## Architecture & Design

### Contract Structure
- **Inheritance**: The contract inherits from multiple OpenZeppelin upgradeable contracts (Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable)
- **Implementation**: The contract implements the IECOSYSTEM interface
- **Token Integration**: It interacts with a governance token implementing the ILENDEFI interface

### Storage Layout
The contract maintains well-organized state variables:
- Token instance and timelock address
- Supply tracking (reward, airdrop, partnership)
- Issuance tracking (rewards, airdrops, partnerships, burns)
- Maximum limits (max reward, max burn)
- Version control for upgrades
- Partner vesting contract mapping

### Role-Based Access Control
The contract implements a robust role system:
- BURNER_ROLE: Controls token burning
- PAUSER_ROLE: Controls pause/unpause functionality
- UPGRADER_ROLE: Controls contract upgradability
- REWARDER_ROLE: Controls reward distribution
- MANAGER_ROLE: Controls partner management and configuration updates
- DEFAULT_ADMIN_ROLE: Grants/revokes roles (assigned to guardian)

## Functionality Analysis

### Token Distribution
- **Airdrop Function**: Distributes equal amounts to multiple recipients
  - Appropriate input validation (min amount, recipient count)
  - Gas-aware implementation (4000 recipient limit)
  - Proper accounting (issuedAirDrop tracking)
  - Zero address handling in recipients array

- **Reward Function**: Distributes tokens to individual addresses
  - Checks maximum reward limits
  - Ensures sufficient reward supply
  - Updates accounting

### Token Management
- **Burn Function**: Permanently removes tokens from circulation
  - Enforces maximum burn limits and supply constraints
  - Updates both rewardSupply and burnedAmount for accurate accounting
  - Calls the token's burn function directly

### Partner Management
- **Add Partner**: Creates vesting contracts for partners
  - Prevents duplicate partners
  - Enforces minimum/maximum allocation limits (100 ether to 50% of supply)
  - Creates dedicated vesting contract with cliff and duration parameters

- **Cancel Partnership**: Terminates vesting contracts
  - Restricted to timelock (governance)
  - Improved accounting through return value tracking
  - Tokens flow from vesting→ecosystem→timelock

### Configuration Management
- **Update Max Reward**: Configures reward limits
  - Prevents excessive values (maximum 5% of remaining supply)
  - Maintains proper event logs of changes

- **Update Max Burn**: Configures burn limits
  - Prevents excessive values (maximum 10% of remaining supply)
  - Maintains proper event logs of changes

## Security Analysis

### Strengths
1. **Comprehensive Access Control**: Well-defined roles with proper separation of concerns
2. **Reentrancy Protection**: All state-changing functions use ReentrancyGuard
3. **Input Validation**: Thorough validation with meaningful error messages
4. **Event Emissions**: Events for all critical state changes
5. **Safe Token Handling**: Uses SafeERC20 for all token operations
6. **Upgradability**: UUPS pattern with proper authorization checks and version tracking
7. **Storage Gap**: Includes `__gap` for future storage additions
8. **Partnership Supply Protections**: Both individual and total limits on partnership allocations



**Inherits:**
[IECOSYSTEM](/contracts/interfaces/IEcosystem.sol/interface.IECOSYSTEM.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Ecosystem contract handles airdrops, rewards, burning, and partnerships

*Implements a secure and upgradeable DAO ecosystem with improved accounting*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

- oz-upgrades: 


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
*burned reward amount*


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
*number of UUPS upgrades*


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


### __gap

```solidity
uint256[49] private __gap;
```


## Functions
### constructor

*Custom error types*

*Events*

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

Sets up the initial state of the contract, including roles and token supplies.

*Initializes the ecosystem contract.*

**Notes:**
- requires: All input addresses must not be zero.

- requires-role: DEFAULT_ADMIN_ROLE for the guardian.

- requires-role: PAUSER_ROLE for the pauser.

- events-emits: {Initialized} event.

- throws: ZeroAddressDetected if any of the input addresses are zero.


```solidity
function initialize(address token, address timelockAddr, address guardian, address pauser) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the governance token.|
|`timelockAddr`|`address`|The address of the timelock controller for partner vesting cancellation.|
|`guardian`|`address`|The address of the guardian (admin).|
|`pauser`|`address`|The address of the pauser.|


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

- throws: RewardLimit if the amount exceeds the maximum reward limit

- throws: RewardSupplyLimit if the total rewarded amount exceeds the reward supply


```solidity
function reward(address to, uint256 amount) external nonReentrant whenNotPaused onlyRole(REWARDER_ROLE);
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
function burn(uint256 amount) external nonReentrant whenNotPaused onlyRole(BURNER_ROLE);
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

- requires: Total issued partnership tokens must not exceed the partnership supply

- events-emits: {AddPartner} event

- throws: InvalidAddress if the partner address is zero

- throws: PartnerExists if the partner already has a vesting contract

- throws: InvalidAmount if the amount is not within the valid range

- throws: AmountExceedsSupply if the total issued partnership tokens exceed the partnership supply


```solidity
function addPartner(address partner, uint256 amount, uint256 cliff, uint256 duration)
    external
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE);
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

This can only be called by the timelock (governance)

*Cancels a partner vesting contract*

*Cancels a partner vesting contract*

**Notes:**
- requires: The caller must be the timelock

- emits: CancelPartnership event on successful cancellation


```solidity
function cancelPartnership(address partner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner whose vesting should be cancelled|


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
function updateMaxReward(uint256 newMaxReward) external whenNotPaused onlyRole(MANAGER_ROLE);
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
function updateMaxBurn(uint256 newMaxBurn) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxBurn`|`uint256`|The new maximum burn amount.|


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

This function is called during the upgrade process to authorize the new implementation.

*Authorizes an upgrade to a new implementation.*

**Notes:**
- requires-role: UPGRADER_ROLE

- events-emits: {Upgrade} event with upgrader, implementation and version


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|The address of the new implementation contract.|


