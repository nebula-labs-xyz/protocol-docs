# IBRIDGE
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/IBridge.sol)

**Author:**
Nebula Labs LLC

**Notes:**
- security-contact: security@nebula-labs.xyz

- copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Functions
### initialize

*Initializes the bridge contract.*


```solidity
function initialize(address guardian, address timelock, address feeRecipient) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`guardian`|`address`|Admin address|
|`timelock`|`address`|Governance timelock address|
|`feeRecipient`|`address`|Address to collect fees|


### pause

*Pauses the contract.*


```solidity
function pause() external;
```

### unpause

*Unpauses the contract.*


```solidity
function unpause() external;
```

### listToken

*Lists a token for bridging.*


```solidity
function listToken(string calldata name, string calldata symbol, address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Token name|
|`symbol`|`string`|Token symbol|
|`token`|`address`|Token address|


### removeToken

*Removes a token from bridging.*


```solidity
function removeToken(address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|


### bridgeTokens

*Bridge tokens to another chain.*


```solidity
function bridgeTokens(address token, address to, uint256 amount, uint256 destChainId) external returns (uint256 id);
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

*Process an inbound bridge request.*


```solidity
function processBridgeInbound(
    uint256 sourceChainId,
    uint256 sourceTxId,
    address sender,
    address recipient,
    address token,
    uint256 amount
) external returns (bool success);
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


### collectFees

*Collects accumulated fees for a token.*


```solidity
function collectFees(address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|


### confirmTransaction

*Confirms a pending transaction.*


```solidity
function confirmTransaction(uint256 txId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|


### expireTransaction

*Marks a transaction as expired if its timeout has passed.*


```solidity
function expireTransaction(uint256 txId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`txId`|`uint256`|Transaction ID|


### updateBridgeParameter

*Updates bridge parameters.*


```solidity
function updateBridgeParameter(string calldata paramName, uint256 value) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paramName`|`string`|Name of the parameter to update|
|`value`|`uint256`|New value|


### updateFeeCollector

*Updates the fee collector address.*


```solidity
function updateFeeCollector(address newCollector) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newCollector`|`address`|New fee collector address|


### addChain

*Adds a supported chain.*


```solidity
function addChain(string calldata name, uint256 chainId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Chain name|
|`chainId`|`uint256`|Chain ID|


### removeChain

*Removes a supported chain.*


```solidity
function removeChain(uint256 chainId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chainId`|`uint256`|Chain ID|


### getTransaction

*Gets transaction details by ID.*


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
|`details`|`TransactionDetails`|Complete transaction details|


### hasRelayerConfirmed

*Checks if a relayer has confirmed a transaction.*


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

*Gets token information.*


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

*Gets list of supported tokens.*


```solidity
function getListings() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of listed token addresses|


### isListed

*Checks if a token is listed.*


```solidity
function isListed(address token) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the token is listed|


### getListedCount

*Gets number of listed tokens.*


```solidity
function getListedCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of listed tokens|


### getChainTransactionCount

*Gets transaction count for a chain.*


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
|`<none>`|`uint256`|Number of transactions for this chain|


### getChain

*Gets chain information.*


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
|`<none>`|`Chain`|Chain information|


### getSupportedChains

*Gets list of supported chains.*


```solidity
function getSupportedChains() external view returns (uint256[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256[]`|Array of chain IDs|


### accumulatedFees

*Gets accumulated fees for a token.*


```solidity
function accumulatedFees(address token) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount of accumulated fees|


### requiredConfirmations

*Gets required confirmations for large transactions.*


```solidity
function requiredConfirmations() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|Number of required confirmations|


### feeBasisPoints

*Gets fee basis points.*


```solidity
function feeBasisPoints() external view returns (uint16);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|Fee in basis points (1 = 0.01%)|


### challengeThreshold

*Gets challenge threshold for large transactions.*


```solidity
function challengeThreshold() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Amount threshold for requiring confirmations|


### getExtendedTransaction

*Gets extended transaction details by ID including source chain ID.*


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
|`details`|`ExtendedTransactionDetails`|Extended transaction details including source chain and inbound status|


### version

*Gets contract version.*


```solidity
function version() external view returns (uint32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint32`|Contract version number|


## Events
### Initialized
*Emitted when contract is initialized.*


```solidity
event Initialized(address indexed src);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|Initializer address|

### Upgrade
*Emitted when contract is upgraded.*


```solidity
event Upgrade(address indexed src, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`src`|`address`|Upgrader address|
|`implementation`|`address`|New implementation address|

### ListToken
*Emitted when a token is listed.*


```solidity
event ListToken(address indexed token);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Listed token address|

### DelistToken
*Emitted when a token is delisted.*


```solidity
event DelistToken(address indexed token);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Delisted token address|

### BridgeOutbound
*Emitted when tokens are bridged to another chain.*


```solidity
event BridgeOutbound(
    uint256 indexed id,
    address indexed sender,
    address indexed recipient,
    address token,
    uint256 amount,
    uint256 fee,
    uint256 destChainId
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|
|`sender`|`address`|Source address|
|`recipient`|`address`|Destination address|
|`token`|`address`|Token address|
|`amount`|`uint256`|Amount of tokens (after fee)|
|`fee`|`uint256`|Fee amount|
|`destChainId`|`uint256`|Destination chain ID|

### BridgeInbound
*Emitted when tokens are received from another chain.*


```solidity
event BridgeInbound(
    uint256 indexed id,
    uint256 indexed sourceChainId,
    address indexed sender,
    address recipient,
    address token,
    uint256 amount
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|
|`sourceChainId`|`uint256`|Source chain ID|
|`sender`|`address`|Source address|
|`recipient`|`address`|Destination address|
|`token`|`address`|Token address|
|`amount`|`uint256`|Amount of tokens|

### TransactionStatusUpdated
*Emitted when a transaction status is updated.*


```solidity
event TransactionStatusUpdated(uint256 indexed id, TransactionStatus status);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|
|`status`|`TransactionStatus`|New transaction status|

### TransactionConfirmed
*Emitted when a transaction is confirmed by a relayer.*


```solidity
event TransactionConfirmed(
    uint256 indexed id,
    address indexed relayer,
    uint256 indexed sourceChainId,
    uint8 confirmationsCount,
    uint8 requiredConfirmations
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|
|`relayer`|`address`|Relayer address|
|`sourceChainId`|`uint256`|Origin chain ID (for inbound transactions)|
|`confirmationsCount`|`uint8`|Current confirmation count|
|`requiredConfirmations`|`uint8`|Required confirmation count|

### FeesCollected
*Emitted when fees are collected.*


```solidity
event FeesCollected(address indexed token, uint256 amount, address indexed recipient);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|
|`amount`|`uint256`|Fee amount|
|`recipient`|`address`|Fee recipient|

### BridgeParameterUpdated
*Emitted when a bridge parameter is updated.*


```solidity
event BridgeParameterUpdated(string indexed param, uint256 value);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`param`|`string`|Parameter name|
|`value`|`uint256`|New value|

### FeeCollectorUpdated
*Emitted when the fee collector is updated.*


```solidity
event FeeCollectorUpdated(address indexed oldCollector, address indexed newCollector);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldCollector`|`address`|Previous fee collector|
|`newCollector`|`address`|New fee collector|

### AddChain
*Emitted when a chain is added to supported chains.*


```solidity
event AddChain(uint256 chainId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chainId`|`uint256`|Added chain ID|

### RemoveChain
*Emitted when a chain is removed from supported chains.*


```solidity
event RemoveChain(uint256 chainId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chainId`|`uint256`|Removed chain ID|

## Errors
### CustomError
*Error for generic errors with custom message.*


```solidity
error CustomError(string msg);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`msg`|`string`|Error message|

### ZeroAddress
*Error for zero address inputs.*


```solidity
error ZeroAddress();
```

### ZeroAmount
*Error for zero amount inputs.*


```solidity
error ZeroAmount();
```

### TokenNotListed
*Error when token is not listed.*


```solidity
error TokenNotListed();
```

### ChainNotSupported
*Error when chain is not supported.*


```solidity
error ChainNotSupported();
```

### InsufficientBalance
*Error when sender has insufficient balance.*


```solidity
error InsufficientBalance();
```

### RateLimitExceeded
*Error when hourly transaction limit is exceeded.*


```solidity
error RateLimitExceeded();
```

### PendingConfirmations
*Error when transaction requires more confirmations.*


```solidity
error PendingConfirmations();
```

### InvalidTransactionStatus
*Error when transaction status is invalid for the operation.*


```solidity
error InvalidTransactionStatus();
```

### TransactionExpired
*Error when transaction has expired.*


```solidity
error TransactionExpired();
```

### AlreadyConfirmed
*Error when relayer has already confirmed a transaction.*


```solidity
error AlreadyConfirmed();
```

### InvalidParameter
*Error when parameter value is invalid.*


```solidity
error InvalidParameter();
```

## Structs
### Token
*Token information struct.*


```solidity
struct Token {
    string name;
    string symbol;
    address tokenAddress;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Token name|
|`symbol`|`string`|Token symbol|
|`tokenAddress`|`address`|Token contract address|

### Chain
*Chain information struct.*


```solidity
struct Chain {
    string name;
    uint256 chainId;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Chain name|
|`chainId`|`uint256`|Chain ID|

### TransactionDetails
Contains all transaction data including confirmations

*Transaction details struct for external viewing.*


```solidity
struct TransactionDetails {
    address sender;
    address recipient;
    address token;
    uint256 amount;
    uint256 timestamp;
    uint256 destChainId;
    TransactionStatus status;
    uint256 expiresAt;
    uint256 fee;
    uint8 confirmCount;
}
```

### ExtendedTransactionDetails
Contains all transaction data including source chain and inbound flag

*Extended transaction details struct with additional chain information.*


```solidity
struct ExtendedTransactionDetails {
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
    uint8 confirmCount;
}
```

## Enums
### TransactionStatus
*Transaction status enum.*


```solidity
enum TransactionStatus {
    Pending,
    Completed,
    Failed,
    Expired
}
```

