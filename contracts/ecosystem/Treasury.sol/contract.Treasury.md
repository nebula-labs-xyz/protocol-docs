# Treasury
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/ecosystem/Treasury.sol)

**Inherits:**
[ITREASURY](/contracts/interfaces/ITreasury.sol/interface.ITREASURY.md), Initializable, UUPSUpgradeable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable

Treasury contract with 3-year linear vesting and external multisig support

*Implements a secure and upgradeable treasury with vesting for ETH and ERC20 tokens*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

- oz-upgrades: 


## State Variables
### MANAGER_ROLE
*Role identifier for accounts that can release funds*


```solidity
bytes32 public constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### PAUSER_ROLE
*Role identifier for accounts that can pause/unpause the contract*


```solidity
bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### UPGRADER_ROLE
*Role identifier for accounts that can upgrade the contract*


```solidity
bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### UPGRADE_TIMELOCK_DURATION
*Upgrade timelock duration in seconds (3 days)*


```solidity
uint256 private constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### _version
*Current version of the contract implementation
Incremented during upgrades*


```solidity
uint32 private _version;
```


### _start
*Start timestamp of the vesting schedule*


```solidity
uint256 private _start;
```


### _duration
*Duration of the vesting period in seconds*


```solidity
uint256 private _duration;
```


### _released
*Total amount of ETH already released*


```solidity
uint256 private _released;
```


### _timelockAddress
*Timelock controller address*


```solidity
address private _timelockAddress;
```


### _erc20Released
*Mapping of token address to amount released for each token*


```solidity
mapping(address => uint256) private _erc20Released;
```


### pendingUpgrade
*Pending upgrade information*


```solidity
UpgradeRequest public pendingUpgrade;
```


### __gap
*Reserved storage space for future upgrades
This allows adding new state variables in future versions while maintaining
the storage layout (30 - 7 = 23)*


```solidity
uint256[23] private __gap;
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

*Prevents initialization of the implementation contract*

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor();
```

### receive

*Allows the contract to receive ETH*

*Emits a {Received} event*


```solidity
receive() external payable virtual;
```

### initialize

Initializes the treasury contract

*Sets up the initial state of the contract*


```solidity
function initialize(address timelock, address multisig, uint256 startOffset, uint256 vestingDuration)
    external
    initializer
    nonZeroAddress(timelock)
    nonZeroAddress(multisig);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timelock`|`address`|The address that will have the DEFAULT_ADMIN_ROLE and MANAGER_ROLE|
|`multisig`|`address`|The address of the Gnosis Safe multisig that will be granted the UPGRADER_ROLE|
|`startOffset`|`uint256`|The number of seconds the start time is before the current block timestamp|
|`vestingDuration`|`uint256`|The duration of vesting in seconds (must be at least 730 days)|


### pause

Pauses all token transfers and releases

*Can only be called by accounts with the PAUSER_ROLE*


```solidity
function pause() external override onlyRole(PAUSER_ROLE);
```

### unpause

Unpauses token transfers and releases

*Can only be called by accounts with the PAUSER_ROLE*


```solidity
function unpause() external override onlyRole(PAUSER_ROLE);
```

### release

Releases a specific amount of vested ETH

*Can only be called by accounts with the MANAGER_ROLE*

*Reverts if the requested amount exceeds vested amount*


```solidity
function release(address to, uint256 amount)
    external
    override
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE)
    nonZeroAddress(to)
    nonZeroAmount(amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address that will receive the ETH|
|`amount`|`uint256`|The amount of ETH to release|


### release

Releases a specific amount of vested tokens

*Can only be called by accounts with the MANAGER_ROLE*

*Reverts if the requested amount exceeds vested amount*


```solidity
function release(address token, address to, uint256 amount)
    external
    override
    nonReentrant
    whenNotPaused
    onlyRole(MANAGER_ROLE)
    nonZeroAddress(token)
    nonZeroAddress(to)
    nonZeroAmount(amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to release|
|`to`|`address`|The address that will receive the tokens|
|`amount`|`uint256`|The amount of tokens to release|


### updateVestingSchedule

Updates the vesting schedule parameters

*Can only be called by accounts with the DEFAULT_ADMIN_ROLE*


```solidity
function updateVestingSchedule(uint256 newStart, uint256 newDuration)
    external
    override
    nonReentrant
    onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newStart`|`uint256`|The new start timestamp|
|`newDuration`|`uint256`|The new duration in seconds|


### emergencyWithdrawToken

Only callable by addresses with MANAGER_ROLE

*Emergency function to withdraw all tokens to the timelock*

**Notes:**
- throws: ZeroAddress if token address is zero

- throws: ZeroBalanceError if contract has no token balance


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
throws: ZeroBalanceError if contract has no ETH balance


```solidity
function emergencyWithdrawEther() external nonReentrant onlyRole(MANAGER_ROLE);
```

### scheduleUpgrade

*Schedules an upgrade to a new implementation*


```solidity
function scheduleUpgrade(address newImplementation) external onlyRole(UPGRADER_ROLE);
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


### timelockAddress

*Get the timelock controller address*


```solidity
function timelockAddress() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The timelock controller address|


### releasable

Returns the amount of ETH that can be released now


```solidity
function releasable() public view virtual override returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of releasable ETH|


### releasable

Returns the amount of a specific token that can be released now


```solidity
function releasable(address token) public view virtual override returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of releasable tokens|


### vestedAmount

Calculates the amount of ETH vested at a specific timestamp


```solidity
function vestedAmount(uint256 timestamp) public view virtual override returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint256`|The timestamp to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The vested amount of ETH|


### vestedAmount

Calculates the amount of tokens vested at a specific timestamp


```solidity
function vestedAmount(address token, uint256 timestamp) public view virtual override returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to check|
|`timestamp`|`uint256`|The timestamp to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The vested amount of tokens|


### version

Returns the current contract version


```solidity
function version() public view virtual override returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|Current version number|


### start

Returns the start timestamp of the vesting period


```solidity
function start() public view virtual override returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Start timestamp|


### duration

Returns the duration of the vesting period


```solidity
function duration() public view virtual override returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Duration in seconds|


### end

Returns the end timestamp of the vesting period


```solidity
function end() public view virtual override returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|End timestamp (start + duration)|


### released

Returns the amount of ETH already released


```solidity
function released() public view virtual override returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of ETH released so far|


### released

Returns the amount of a specific token already released


```solidity
function released(address token) public view virtual override returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The ERC20 token to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of tokens released so far|


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


### _vestingSchedule

*Internal function to calculate vested amounts for a given allocation and timestamp*

*Uses linear vesting between start and end time*


```solidity
function _vestingSchedule(uint256 totalAllocation, uint256 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAllocation`|`uint256`|The total amount to vest|
|`timestamp`|`uint256`|The timestamp to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount vested at the specified timestamp|


