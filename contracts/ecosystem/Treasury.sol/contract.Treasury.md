# Treasury
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/ecosystem/Treasury.sol)

**Inherits:**
[ITREASURY](/contracts/interfaces/ITreasury.sol/interface.ITREASURY.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Vesting contract: initialRelease + (36 month duration)

Offers flexible withdrawal schedule (gas efficient)

*Implements secure and upgradeable DAO treasury with linear vesting*

**Notes:**
- security-contact: security@nebula-labs.xysz

- oz-upgrades: 


## State Variables
### PAUSER_ROLE
*AccessControl Pauser Role*


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE
*AccessControl Manager Role*


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
*AccessControl Upgrader Role*


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### _released
*ETH amount released so far*


```solidity
uint256 private _released;
```


### _start
*start timestamp*


```solidity
uint64 private _start;
```


### _duration
*duration seconds*


```solidity
uint64 private _duration;
```


### version
*UUPS version*


```solidity
uint32 public version;
```


### _erc20Released
*token amounts released so far*


```solidity
mapping(address token => uint256) private _erc20Released;
```


### __gap
*upgrade gap*


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

### receive

solidity receive function


```solidity
receive() external payable virtual;
```

### initialize

*Initializes the UUPS contract*


```solidity
function initialize(address guardian, address timelock) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|admin address|
|`timelock`|`address`|address of timelock contract|


### pause

Emergency function to pause contract operations

*Pauses all token transfers and releases.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Paused} from PausableUpgradeable


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

Resumes normal contract operations after pause

*Unpauses token transfers and releases.*

**Notes:**
- requires-role: PAUSER_ROLE

- events-emits: {Unpaused} from PausableUpgradeable


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### release

Allows the manager to release vested ETH to a beneficiary

*Release the native token (ether) that have already vested.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: Amount must not exceed vested amount

- requires: Beneficiary address must not be zero

- security: non-reentrant

- access: restricted to MANAGER_ROLE

- events-emits: {EtherReleased}


```solidity
function release(address to, uint256 amount) external nonReentrant whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address that will receive the vested ETH|
|`amount`|`uint256`|The amount of ETH to release|


### release

Allows the manager to release vested tokens to a beneficiary

*Release the ERC20 tokens that have already vested.*

**Notes:**
- requires-role: MANAGER_ROLE

- requires: Contract must not be paused

- requires: Amount must not exceed vested amount

- requires: Token address must not be zero

- requires: Beneficiary address must not be zero

- access: restricted to MANAGER_ROLE

- events-emits: {ERC20Released}


```solidity
function release(address token, address to, uint256 amount) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The address of the ERC20 token to release|
|`to`|`address`|The address that will receive the vested tokens|
|`amount`|`uint256`|The amount of tokens to release|


### start

*Getter for the start timestamp.*


```solidity
function start() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|start timestamp|


### duration

*Getter for the vesting duration.*


```solidity
function duration() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|duration seconds|


### end

*Getter for the end timestamp.*


```solidity
function end() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|end timnestamp|


### released

*Getter for the amount of eth already released*


```solidity
function released() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of ETH released so far|


### released

*Getter for the amount of token already released*


```solidity
function released(address token) public view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of tokens released so far|


### releasable

*Getter for the amount of releasable eth.*


```solidity
function releasable() public view virtual returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested ETH|


### releasable

*Getter for the amount of vested `ERC20` tokens.*


```solidity
function releasable(address token) public view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount of vested tokens|


### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

### vestedAmount

*Calculates the amount of ETH that has already vested. Default implementation is a linear vesting curve.*


```solidity
function vestedAmount(uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount ETH vested|


### vestedAmount

*Calculates the amount of tokens that has already vested. Default implementation is a linear vesting curve.*


```solidity
function vestedAmount(address token, uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address of token|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount vested|


### _vestingSchedule

*Virtual implementation of the vesting formula. This returns the amount vested, as a function of time, for
an asset given its total historical allocation.*


```solidity
function _vestingSchedule(uint256 totalAllocation, uint64 timestamp) internal view virtual returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAllocation`|`uint256`|initial amount|
|`timestamp`|`uint64`|current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount vested|


