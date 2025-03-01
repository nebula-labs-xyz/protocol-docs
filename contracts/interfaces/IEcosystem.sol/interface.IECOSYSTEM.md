# IECOSYSTEM
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IEcosystem.sol)

Interface for the Ecosystem contract that handles airdrops, rewards, burning, and partnerships

*Defines all external functions and events for the Ecosystem contract*


## Functions
### initialize

Initializes the ecosystem contract

*Sets up the initial state of the contract, including roles and token supplies*

**Note:**
throws: ZeroAddressDetected if any address is zero


```solidity
function initialize(address token, address timelockAddr, address guardian, address pauser) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the governance token|
|`timelockAddr`|`address`|Address of the timelock controller for partner vesting cancellation|
|`guardian`|`address`|Address of the guardian (admin)|
|`pauser`|`address`|Address of the pauser|


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

### airdrop

Distributes tokens to multiple recipients

*Performs an airdrop of a fixed amount of tokens to each address in the recipients array*

**Notes:**
- throws: InvalidAmount if amount is less than 1 ether

- throws: AirdropSupplyLimit if total exceeds available supply

- throws: GasLimit if recipients array is too large


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

*Transfers a specified amount of tokens to a recipient as a reward*

**Notes:**
- throws: InvalidAmount if amount is zero

- throws: RewardLimit if amount exceeds maximum reward

- throws: RewardSupplyLimit if amount exceeds available supply


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

*Permanently removes tokens from circulation, updating supply calculations*

**Notes:**
- throws: InvalidAmount if amount is zero

- throws: MaxBurnLimit if amount exceeds maximum burn

- throws: BurnSupplyLimit if amount exceeds available supply


```solidity
function burn(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of tokens to burn|


### addPartner

Creates a vesting contract for a new partner

*Deploys a new PartnerVesting contract and funds it with the specified amount*

**Notes:**
- throws: InvalidAddress if partner address is zero

- throws: PartnerExists if partner already has a vesting contract

- throws: InvalidAmount if amount is outside allowed range

- throws: AmountExceedsSupply if total exceeds partnership supply


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

*Returns unvested tokens to the timelock and updates accounting*

**Notes:**
- throws: CallerNotAllowed if caller is not the timelock

- throws: InvalidAddress if no vesting contract exists for partner


```solidity
function cancelPartnership(address partner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`partner`|`address`|Address of the partner|


### updateMaxReward

Updates the maximum one-time reward amount

*Sets a new limit on the maximum tokens that can be rewarded in one transaction*

**Notes:**
- throws: InvalidAmount if new value is zero

- throws: ExcessiveMaxValue if value exceeds allowed percentage of supply


```solidity
function updateMaxReward(uint256 newMaxReward) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxReward`|`uint256`|New maximum reward value|


### updateMaxBurn

Updates the maximum one-time burn amount

*Sets a new limit on the maximum tokens that can be burned in one transaction*

**Notes:**
- throws: InvalidAmount if new value is zero

- throws: ExcessiveMaxValue if value exceeds allowed percentage of supply


```solidity
function updateMaxBurn(uint256 newMaxBurn) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxBurn`|`uint256`|New maximum burn value|


### availableRewardSupply

Returns the available reward supply

*Calculates tokens available for rewards by subtracting issued rewards from total supply*


```solidity
function availableRewardSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Available tokens in the reward supply|


### availableAirdropSupply

Returns the available airdrop supply

*Calculates tokens available for airdrops by subtracting issued airdrops from total supply*


```solidity
function availableAirdropSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Available tokens in the airdrop supply|


### availablePartnershipSupply

Returns the available partnership supply

*Calculates tokens available for partnerships by subtracting issued partnerships from total supply*


```solidity
function availablePartnershipSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Available tokens in the partnership supply|


### rewardSupply

Gets the total reward supply

*Returns the total amount of tokens allocated for rewards*


```solidity
function rewardSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total reward supply|


### maxReward

Gets the maximum reward amount

*Returns the maximum tokens that can be rewarded in one transaction*


```solidity
function maxReward() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum reward amount|


### issuedReward

Gets the total amount of tokens issued as rewards

*Returns the cumulative amount of tokens that have been rewarded*


```solidity
function issuedReward() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total issued reward amount|


### burnedAmount

Gets the total amount of tokens burned

*Returns the cumulative amount of tokens that have been burned*


```solidity
function burnedAmount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total burned amount|


### maxBurn

Gets the maximum burn amount

*Returns the maximum tokens that can be burned in one transaction*


```solidity
function maxBurn() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum burn amount|


### airdropSupply

Gets the total airdrop supply

*Returns the total amount of tokens allocated for airdrops*


```solidity
function airdropSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total airdrop supply|


### issuedAirDrop

Gets the total amount of tokens issued via airdrops

*Returns the cumulative amount of tokens that have been airdropped*


```solidity
function issuedAirDrop() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total issued airdrop amount|


### partnershipSupply

Gets the total partnership supply

*Returns the total amount of tokens allocated for partnerships*


```solidity
function partnershipSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total partnership supply|


### issuedPartnership

Gets the total amount of tokens issued to partners

*Returns the cumulative amount of tokens that have been allocated to partners*


```solidity
function issuedPartnership() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total issued partnership amount|


### version

Gets the contract version

*Returns the version number, which is incremented with each upgrade*


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|The current version number|


### timelock

Gets the timelock address

*Returns the address of the timelock controller used for governance actions*


```solidity
function timelock() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The timelock address|


### vestingContracts

Gets the vesting contract address for a partner

*Returns the address of the vesting contract created for a specific partner*


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
|`<none>`|`address`|The vesting contract address, or zero address if none exists|


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

## Errors
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

### CallerNotAllowed
*Error thrown when a function is called by an unauthorized account*


```solidity
error CallerNotAllowed();
```

