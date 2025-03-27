# IECOSYSTEM
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/interfaces/IEcosystem.sol)

Interface for the Ecosystem contract that handles airdrops, rewards, burning, partnerships, and secure upgrades

*Defines all external functions and events for the Ecosystem contract*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Functions
### initialize

Initializes the ecosystem contract

*Sets up the initial state of the contract, including roles and token supplies*


```solidity
function initialize(address token, address timelockAddr, address multisig) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the governance token|
|`timelockAddr`|`address`|Address of the timelock controller|
|`multisig`|`address`|Address of the multisig wallet|


### pause

Pauses all contract operations

*Can only be called by accounts with the PAUSER_ROLE*


```solidity
function pause() external;
```

### unpause

Resumes all contract operations

*Can only be called by accounts with the PAUSER_ROLE*


```solidity
function unpause() external;
```

### scheduleUpgrade

Schedules an upgrade to a new implementation

*Can only be called by accounts with the UPGRADER_ROLE*


```solidity
function scheduleUpgrade(address newImplementation) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newImplementation`|`address`|Address of the new implementation|


### cancelUpgrade

Cancels a previously scheduled upgrade

*Can only be called by accounts with the UPGRADER_ROLE*


```solidity
function cancelUpgrade() external;
```

### upgradeTimelockRemaining

Returns the remaining time before a scheduled upgrade can be executed

*Returns 0 if no upgrade is scheduled or timelock has passed*


```solidity
function upgradeTimelockRemaining() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The time remaining in seconds|


### emergencyWithdrawToken

Emergency function to withdraw tokens to the timelock

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function emergencyWithdrawToken(address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token to withdraw|


### airdrop

Distributes tokens to multiple recipients

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function airdrop(address[] calldata recipients, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Array of addresses to receive the airdrop|
|`amount`|`uint256`|Amount of tokens each recipient will receive|


### reward

Rewards a single address with tokens

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function reward(address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Recipient address|
|`amount`|`uint256`|Amount of tokens to reward|


### burn

Burns tokens from the reward supply

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function burn(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of tokens to burn|


### addPartner

Creates a vesting contract for a new partner

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function addPartner(address partner, uint256 amount, uint256 cliff, uint256 duration) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|Address of the partner|
|`amount`|`uint256`|Amount of tokens to vest|
|`cliff`|`uint256`|Cliff period in seconds|
|`duration`|`uint256`|Vesting duration in seconds|


### cancelPartnership

Cancels a partner's vesting contract

*Can only be called by the timelock*


```solidity
function cancelPartnership(address partner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|Address of the partner|


### updateMaxReward

Updates the maximum one-time reward amount

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function updateMaxReward(uint256 newMaxReward) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxReward`|`uint256`|New maximum reward value|


### updateMaxBurn

Updates the maximum one-time burn amount

*Can only be called by accounts with the MANAGER_ROLE*


```solidity
function updateMaxBurn(uint256 newMaxBurn) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxBurn`|`uint256`|New maximum burn value|


### availableRewardSupply

Returns the available reward supply

*Calculates remaining reward tokens*


```solidity
function availableRewardSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Available tokens in the reward supply|


### availableAirdropSupply

Returns the available airdrop supply

*Calculates remaining airdrop tokens*


```solidity
function availableAirdropSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Available tokens in the airdrop supply|


### availablePartnershipSupply

Returns the available partnership supply

*Calculates remaining partnership tokens*


```solidity
function availablePartnershipSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Available tokens in the partnership supply|


### rewardSupply

Gets the total reward supply


```solidity
function rewardSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total reward supply|


### maxReward

Gets the maximum reward amount


```solidity
function maxReward() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum reward amount|


### issuedReward

Gets the total amount of tokens issued as rewards


```solidity
function issuedReward() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total issued reward amount|


### burnedAmount

Gets the total amount of tokens burned


```solidity
function burnedAmount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total burned amount|


### maxBurn

Gets the maximum burn amount


```solidity
function maxBurn() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum burn amount|


### airdropSupply

Gets the total airdrop supply


```solidity
function airdropSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total airdrop supply|


### issuedAirDrop

Gets the total amount of tokens issued via airdrops


```solidity
function issuedAirDrop() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total issued airdrop amount|


### partnershipSupply

Gets the total partnership supply


```solidity
function partnershipSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total partnership supply|


### issuedPartnership

Gets the total amount of tokens issued to partners


```solidity
function issuedPartnership() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total issued partnership amount|


### version

Gets the contract version


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|The current version number|


### timelock

Gets the timelock address


```solidity
function timelock() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The timelock address|


### vestingContracts

Gets the vesting contract address for a partner


```solidity
function vestingContracts(address partner) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The vesting contract address|


## Events
### Initialized
*Emitted when the contract is initialized*


```solidity
event Initialized(address indexed initializer);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initializer`|`address`|The address that initialized the contract|

### AirDrop
*Emitted when an airdrop is executed*


```solidity
event AirDrop(address[] indexed winners, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`winners`|`address[]`|Array of addresses that received the airdrop|
|`amount`|`uint256`|Amount of tokens each address received|

### Reward
*Emitted when a reward is distributed*


```solidity
event Reward(address indexed sender, address indexed recipient, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sender`|`address`|The address that initiated the reward|
|`recipient`|`address`|The address that received the reward|
|`amount`|`uint256`|The amount of tokens awarded|

### Burn
*Emitted when tokens are burned*


```solidity
event Burn(address indexed burner, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`burner`|`address`|The address that initiated the burn|
|`amount`|`uint256`|The amount of tokens burned|

### AddPartner
*Emitted when a new partner is added*


```solidity
event AddPartner(address indexed partner, address indexed vestingContract, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner|
|`vestingContract`|`address`|The address of the partner's vesting contract|
|`amount`|`uint256`|The amount of tokens allocated to the partner|

### CancelPartnership
*Emitted when a partnership is cancelled*


```solidity
event CancelPartnership(address indexed partner, uint256 remainingAmount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner whose contract was cancelled|
|`remainingAmount`|`uint256`|The amount of tokens returned to the timelock|

### MaxRewardUpdated
*Emitted when the maximum reward amount is updated*


```solidity
event MaxRewardUpdated(address indexed updater, uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|The address that updated the maximum reward|
|`oldValue`|`uint256`|The previous maximum reward value|
|`newValue`|`uint256`|The new maximum reward value|

### MaxBurnUpdated
*Emitted when the maximum burn amount is updated*


```solidity
event MaxBurnUpdated(address indexed updater, uint256 oldValue, uint256 newValue);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|The address that updated the maximum burn|
|`oldValue`|`uint256`|The previous maximum burn value|
|`newValue`|`uint256`|The new maximum burn value|

### Upgrade
*Emitted when the contract is upgraded*


```solidity
event Upgrade(address indexed upgrader, address indexed newImplementation, uint32 version);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`upgrader`|`address`|The address that performed the upgrade|
|`newImplementation`|`address`|The address of the new implementation|
|`version`|`uint32`|The new version number|

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
|`sender`|`address`|The address that scheduled the upgrade|
|`implementation`|`address`|The new implementation address|
|`scheduledTime`|`uint64`|The time when the upgrade was scheduled|
|`effectiveTime`|`uint64`|The time when the upgrade can be executed|

### UpgradeCancelled
*Emitted when a scheduled upgrade is cancelled*


```solidity
event UpgradeCancelled(address indexed canceller, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`canceller`|`address`|The address that cancelled the upgrade|
|`implementation`|`address`|The implementation address that was cancelled|

### EmergencyWithdrawal
*Emitted when an emergency withdrawal is executed*


```solidity
event EmergencyWithdrawal(address indexed token, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token withdrawn|
|`amount`|`uint256`|Amount withdrawn|

## Errors
### ValidationFailed
*Error thrown for general validation failures*


```solidity
error ValidationFailed(string reason);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`reason`|`string`|Description of the validation failure|

### UpgradeTimelockActive
*Error thrown if you try to upgrade while the timelock is active*


```solidity
error UpgradeTimelockActive(uint256 remainingTime);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`remainingTime`|`uint256`|Time remaining in the timelock period|

### UpgradeNotScheduled
*Thrown when trying to execute an upgrade that wasn't scheduled*


```solidity
error UpgradeNotScheduled();
```

### ImplementationMismatch
*Thrown when trying to execute an upgrade with wrong implementation*


```solidity
error ImplementationMismatch(address expected, address provided);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`expected`|`address`|The implementation address that was scheduled|
|`provided`|`address`|The implementation address that was attempted|

### ZeroAddressDetected
*Error thrown when a zero address is provided where a non-zero address is required*


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
|`amount`|`uint256`|The invalid amount that was provided|

### AirdropSupplyLimit
*Error thrown when an airdrop exceeds the available supply*


```solidity
error AirdropSupplyLimit(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The amount of tokens requested for the airdrop|
|`available`|`uint256`|The amount of tokens actually available|

### GasLimit
*Error thrown when too many recipients are provided for an airdrop*


```solidity
error GasLimit(uint256 recipients);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`uint256`|The number of recipients that would exceed gas limits|

### RewardLimit
*Error thrown when a reward exceeds the maximum allowed amount*


```solidity
error RewardLimit(uint256 amount, uint256 maxAllowed);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The requested reward amount|
|`maxAllowed`|`uint256`|The maximum allowed reward amount|

### RewardSupplyLimit
*Error thrown when a reward exceeds the available supply*


```solidity
error RewardSupplyLimit(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested reward amount|
|`available`|`uint256`|The amount of tokens actually available|

### BurnSupplyLimit
*Error thrown when a burn exceeds the available supply*


```solidity
error BurnSupplyLimit(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested burn amount|
|`available`|`uint256`|The amount of tokens actually available|

### MaxBurnLimit
*Error thrown when a burn exceeds the maximum allowed amount*


```solidity
error MaxBurnLimit(uint256 amount, uint256 maxAllowed);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The requested burn amount|
|`maxAllowed`|`uint256`|The maximum allowed burn amount|

### InvalidAddress
*Error thrown when an invalid address is provided*


```solidity
error InvalidAddress();
```

### PartnerExists
*Error thrown when attempting to create a vesting contract for an existing partner*


```solidity
error PartnerExists(address partner);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|The address of the partner that already exists|

### AmountExceedsSupply
*Error thrown when an amount exceeds the available supply*


```solidity
error AmountExceedsSupply(uint256 requested, uint256 available);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|The requested amount|
|`available`|`uint256`|The amount actually available|

### ExcessiveMaxValue
*Error thrown when a maximum value update exceeds allowed limits*


```solidity
error ExcessiveMaxValue(uint256 amount, uint256 maxAllowed);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The requested new maximum value|
|`maxAllowed`|`uint256`|The maximum allowed value|

### InvalidVestingSchedule
*Thrown when attempting to set an invalid vesting duration*


```solidity
error InvalidVestingSchedule();
```

### ZeroBalance
*Error thrown when attempting operations with zero balance*


```solidity
error ZeroBalance();
```

## Structs
### UpgradeRequest
*Structure to track pending upgrades with timelock*


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
|`implementation`|`address`|The address of the new implementation contract|
|`scheduledTime`|`uint64`|The timestamp when the upgrade was scheduled|
|`exists`|`bool`|Boolean flag indicating if an upgrade is currently scheduled|

