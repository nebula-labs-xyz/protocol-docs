# Bridge
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/bridge/Bridge.sol)

**Inherits:**
[IBRIDGE](/contracts/interfaces/IBridge.sol/interface.IBRIDGE.md), Initializable, PausableUpgradeable, AccessControlUpgradeable, ReentrancyGuardUpgradeable, UUPSUpgradeable

Secure cross-chain token bridge with advanced security features

*Implements a secure, upgradeable bridge with transaction management*

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.

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


### RELAYER_ROLE
*AccessControl Relayer Role - for trusted cross-chain relayers*


```solidity
bytes32 internal constant RELAYER_ROLE = keccak256("RELAYER_ROLE");
```


### BASIS_POINTS
*Fee basis points (10000 = 100%)*


```solidity
uint16 internal constant BASIS_POINTS = 10000;
```


### DEFAULT_TIMEOUT
*Default transaction timeout (7 days)*


```solidity
uint256 internal constant DEFAULT_TIMEOUT = 7 days;
```


### DEFAULT_FEE_BP
*Default fee in basis points (0.1%)*


```solidity
uint16 internal constant DEFAULT_FEE_BP = 10;
```


### DEFAULT_HOURLY_LIMIT
*Default hourly transaction limit*


```solidity
uint256 internal constant DEFAULT_HOURLY_LIMIT = 1000;
```


### DEFAULT_REQUIRED_CONFIRMATIONS
*Default required confirmations*


```solidity
uint8 internal constant DEFAULT_REQUIRED_CONFIRMATIONS = 3;
```


### DEFAULT_CHALLENGE_THRESHOLD
*Default challenge threshold (1000 tokens)*


```solidity
uint256 internal constant DEFAULT_CHALLENGE_THRESHOLD = 1000 ether;
```


### tokenSet
*EnumerableSet of supported tokens*


```solidity
EnumerableSet.AddressSet internal tokenSet;
```


### chainSet
*EnumerableSet of supported chains*


```solidity
EnumerableSet.UintSet internal chainSet;
```


### transactionId
*stores last transaction ID*


```solidity
uint256 public transactionId;
```


### chainCount
*chain transaction count, by chainId*


```solidity
mapping(uint256 chainId => uint256 count) public chainCount;
```


### chains
*Chain object by chainId mapping*


```solidity
mapping(uint256 chainId => Chain) public chains;
```


### tokens
*supported tokens mapping*


```solidity
mapping(address asset => Token) public tokens;
```


### transactions
*Transaction by ID mapping*


```solidity
mapping(uint256 id => ExtendedTransaction) private transactions;
```


### version
*Number of UUPS upgrades*


```solidity
uint32 public version;
```


### transactionTimeout
*Transaction timeout in seconds (default 7 days)*


```solidity
uint256 public transactionTimeout;
```


### feeBasisPoints
*Fee in basis points (default 0.1% = 10 basis points)*


```solidity
uint16 public feeBasisPoints;
```


### hourlyTransactionLimit
*Rate limiting: max transactions per hour*


```solidity
uint256 public hourlyTransactionLimit;
```


### currentHourlyTransactions
*Rate limiting: transactions in the current hour*


```solidity
uint256 public currentHourlyTransactions;
```


### currentHourTimestamp
*Rate limiting: current hour timestamp*


```solidity
uint256 public currentHourTimestamp;
```


### feeCollector
*Fee collector address*


```solidity
address public feeCollector;
```


### accumulatedFees
*Accumulated fees by token*


```solidity
mapping(address token => uint256 amount) public accumulatedFees;
```


### requiredConfirmations
*Required confirmations for large transactions*


```solidity
uint8 public requiredConfirmations;
```


### challengeThreshold
*Challenge amount threshold - amounts above this require confirmations*


```solidity
uint256 public challengeThreshold;
```


### confirmations
*Challenge confirmations mapping*


```solidity
mapping(uint256 txId => uint8 confirmations) public confirmations;
```


### hasConfirmed
*Challenge confirmation status mapping*


```solidity
mapping(uint256 txId => mapping(address relayer => bool hasConfirmed)) public hasConfirmed;
```


### __gap
*Reserved space for future upgrades*


```solidity
uint256[45] private __gap;
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
function initialize(address guardian, address timelock, address feeRecipient) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|Admin address|
|`timelock`|`address`|Governance timelock address|
|`feeRecipient`|`address`|Address to collect fees|


### listToken

*Adds token to listed tokens.*


```solidity
function listToken(string calldata name, string calldata symbol, address token)
    external
    whenNotPaused
    onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Token name|
|`symbol`|`string`|Token symbol|
|`token`|`address`|Token address|


### removeToken

*Removes token from listed tokens.*


```solidity
function removeToken(address token) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|


### bridgeTokens

*Bridge function for outbound token transfers.*


```solidity
function bridgeTokens(address token, address to, uint256 amount, uint256 destChainId)
    external
    nonReentrant
    whenNotPaused
    returns (uint256 id);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|
|`to`|`address`|Recipient address|
|`amount`|`uint256`|Amount to bridge|
|`destChainId`|`uint256`|Destination chain ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|


### processBridgeInbound

*Processes an inbound bridge request (called by relayers).*


```solidity
function processBridgeInbound(
    uint256 sourceChainId,
    uint256 sourceTxId,
    address sender,
    address recipient,
    address token,
    uint256 amount
) external nonReentrant whenNotPaused onlyRole(RELAYER_ROLE) returns (bool success);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sourceChainId`|`uint256`|Source chain ID|
|`sourceTxId`|`uint256`|Original transaction ID on source chain|
|`sender`|`address`|Sender on source chain|
|`recipient`|`address`|Recipient on this chain|
|`token`|`address`|Token address|
|`amount`|`uint256`|Amount to mint|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`success`|`bool`|Whether the operation succeeded|


### _processLargeInboundTransaction

*Internal function to handle large inbound transaction confirmations*


```solidity
function _processLargeInboundTransaction(
    uint256 inboundTxId,
    address sender,
    address recipient,
    address token,
    uint256 amount,
    uint256 sourceChainId
) internal;
```

### collectFees

*Collects accumulated fees for a token.*


```solidity
function collectFees(address token) external nonReentrant onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|


### confirmTransaction

*Confirms a pending transaction (for large amounts).*


```solidity
function confirmTransaction(uint256 txId) external nonReentrant onlyRole(RELAYER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|


### expireTransaction

*Marks a transaction as expired if its timeout has passed.*


```solidity
function expireTransaction(uint256 txId) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|


### pause

*Pause contract.*


```solidity
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause

*Unpause contract.*


```solidity
function unpause() external onlyRole(PAUSER_ROLE);
```

### updateBridgeParameter

*Updates bridge parameters.*


```solidity
function updateBridgeParameter(string calldata paramName, uint256 value)
    external
    whenNotPaused
    onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paramName`|`string`|Name of the parameter to update|
|`value`|`uint256`|New value|


### updateFeeCollector

*Updates the fee collector address.*


```solidity
function updateFeeCollector(address newCollector) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCollector`|`address`|New fee collector address|


### addChain

*Add supported chain.*


```solidity
function addChain(string calldata name, uint256 chainId) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Chain name|
|`chainId`|`uint256`|Chain ID|


### removeChain

*Remove supported chain.*


```solidity
function removeChain(uint256 chainId) external whenNotPaused onlyRole(MANAGER_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chainId`|`uint256`|Chain ID|


### getTransaction

*Get transaction details by ID*


```solidity
function getTransaction(uint256 txId) external view returns (TransactionDetails memory details);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`details`|`TransactionDetails`|Complete transaction details including confirmation count|


### getExtendedTransaction

*Get extended transaction details by ID*


```solidity
function getExtendedTransaction(uint256 txId) external view returns (ExtendedTransactionDetails memory details);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`details`|`ExtendedTransactionDetails`|Extended transaction details|


### hasRelayerConfirmed

*Check if a relayer has confirmed a transaction.*


```solidity
function hasRelayerConfirmed(uint256 txId, address relayer) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|
|`relayer`|`address`|Relayer address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the relayer has confirmed|


### getToken

*Getter for the Token object.*


```solidity
function getToken(address token) external view returns (Token memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Token`|Token info object|


### getListings

*Getter for the supported token listings.*


```solidity
function getListings() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|array of listed token addresses|


### isListed

*Getter returns true if token is listed.*


```solidity
function isListed(address token) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|boolean value|


### getListedCount

*Getter returns listed token count.*


```solidity
function getListedCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|number of listed tokens|


### getChainTransactionCount

*Getter returns chain transaction count.*


```solidity
function getChainTransactionCount(uint256 chainId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chainId`|`uint256`|Chain ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|number of transactions for this chain|


### getChain

*Getter returns Chain object.*


```solidity
function getChain(uint256 chainId) external view returns (Chain memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chainId`|`uint256`|Chain ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Chain`|Chain struct|


### getSupportedChains

*Getter returns supported chain IDs.*


```solidity
function getSupportedChains() external view returns (uint256[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256[]`|Array of chain IDs|


### _checkAndUpdateRateLimit

*Internal function to check and update rate limits.*


```solidity
function _checkAndUpdateRateLimit() internal;
```

### _authorizeUpgrade


```solidity
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE);
```

## Structs
### ExtendedTransaction
*Extended Transaction struct with status and expiry*


```solidity
struct ExtendedTransaction {
    address sender;
    address recipient;
    address token;
    uint256 amount;
    uint256 timestamp;
    uint256 destChainId;
    uint256 sourceChainId;
    TransactionStatus status;
    uint256 expiresAt;
    uint256 fee;
    bool isInbound;
}
```

