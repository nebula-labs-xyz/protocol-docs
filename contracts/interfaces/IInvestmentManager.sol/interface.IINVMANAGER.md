# IINVMANAGER
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IInvestmentManager.sol)

Defines the interface for managing investment rounds and token distribution

*Implements events and data structures for investment lifecycle management*

**Note:**
copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## Events
### Initialized
Emitted when contract is initialized


```solidity
event Initialized(address indexed caller);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|Address that initialized the contract|

### CreateRound
Emitted when a new investment round is created


```solidity
event CreateRound(uint32 indexed roundId, uint64 start, uint64 duration, uint256 ethTarget, uint256 tokenAlloc);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Unique identifier for the round|
|`start`|`uint64`|Start timestamp|
|`duration`|`uint64`|Duration in seconds|
|`ethTarget`|`uint256`|ETH investment target|
|`tokenAlloc`|`uint256`|Token allocation for the round|

### RoundStatusUpdated
Emitted when a round's status changes


```solidity
event RoundStatusUpdated(uint32 indexed roundId, RoundStatus status);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`status`|`RoundStatus`|New status|

### Invest
Emitted when an investment is made


```solidity
event Invest(uint32 indexed roundId, address indexed investor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`investor`|`address`|Investor address|
|`amount`|`uint256`|ETH amount invested|

### RoundComplete
Emitted when a round reaches its target


```solidity
event RoundComplete(uint32 indexed roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|

### CancelInvestment
Emitted when an investment is cancelled


```solidity
event CancelInvestment(uint32 indexed roundId, address indexed investor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`investor`|`address`|Investor address|
|`amount`|`uint256`|ETH amount refunded|

### RoundFinalized
Emitted when a round is closed


```solidity
event RoundFinalized(
    address indexed caller, uint32 indexed roundId, uint256 totalEthRaised, uint256 totalTokensDistributed
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|Address that closed the round|
|`roundId`|`uint32`|Round identifier|
|`totalEthRaised`|`uint256`|ETH raised|
|`totalTokensDistributed`|`uint256`|Tokens|

### RoundCancelled
Emitted when a round is cancelled


```solidity
event RoundCancelled(uint32 indexed roundId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|

### DeployVesting
Emitted when a vesting contract is deployed


```solidity
event DeployVesting(uint32 indexed roundId, address indexed investor, address vestingContract, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`investor`|`address`|Investor address|
|`vestingContract`|`address`|Address of deployed vesting contract|
|`amount`|`uint256`|Token amount vested|

### WithdrawTokens
Emitted when tokens are withdrawn


```solidity
event WithdrawTokens(uint32 indexed roundId, address indexed caller, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`caller`|`address`|Address that initiated withdrawal|
|`amount`|`uint256`|Token amount withdrawn|

### Upgrade
Emitted when contract is upgraded


```solidity
event Upgrade(address indexed caller, address indexed implementation);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|Address that initiated upgrade|
|`implementation`|`address`|New implementation address|

### InvestorAllocated
Emitted when an investor is allocated to a round


```solidity
event InvestorAllocated(uint32 indexed roundId, address indexed investor, uint256 ethAmount, uint256 tokenAmount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`investor`|`address`|Investor address|
|`ethAmount`|`uint256`|ETH allocation|
|`tokenAmount`|`uint256`|Token allocation|

### InvestorAllocationRemoved
Emitted when an investor's allocation is removed from a round

*Triggered by removeInvestorAllocation function*

**Notes:**
- security: Event includes both ETH and token amounts for complete tracking

- security: Uses indexed parameters for efficient filtering


```solidity
event InvestorAllocationRemoved(
    uint32 indexed roundId, address indexed investor, uint256 ethAmount, uint256 tokenAmount
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|The identifier of the investment round|
|`investor`|`address`|Address of the investor whose allocation was removed|
|`ethAmount`|`uint256`|The ETH amount that was allocated|
|`tokenAmount`|`uint256`|The token amount that was allocated|

### RefundClaimed
Emitted when an investor is allocated to a round


```solidity
event RefundClaimed(uint32 indexed roundId, address indexed investor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roundId`|`uint32`|Round identifier|
|`investor`|`address`|Investor address|
|`amount`|`uint256`|ETH allocation|

## Structs
### Round
Represents an investment round's configuration and state

*All monetary values stored with 18 decimals precision*

*Time values stored as uint64 for gas optimization*

**Notes:**
- security: Uses uint64 for timestamps to support dates until year 2554

- security: Packs smaller uints together for gas optimization

- security: Status transitions are unidirectional and sequential


```solidity
struct Round {
    uint256 etherTarget;
    uint256 etherInvested;
    uint256 tokenAllocation;
    uint256 tokenDistributed;
    uint64 startTime;
    uint64 endTime;
    uint64 vestingCliff;
    uint64 vestingDuration;
    uint32 participants;
    RoundStatus status;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`etherTarget`|`uint256`|Target amount of ETH to raise in round (18 decimals)|
|`etherInvested`|`uint256`|Current amount of ETH invested in round (18 decimals)|
|`tokenAllocation`|`uint256`|Total tokens allocated for distribution in round (18 decimals)|
|`tokenDistributed`|`uint256`|Amount of tokens already distributed to investors (18 decimals)|
|`startTime`|`uint64`|Unix timestamp when round begins accepting investments|
|`endTime`|`uint64`|Unix timestamp when round stops accepting investments|
|`vestingCliff`|`uint64`|Duration in seconds before vesting begins|
|`vestingDuration`|`uint64`|Total duration in seconds for complete token vesting|
|`participants`|`uint32`|Number of unique investors in round|
|`status`|`RoundStatus`|Current state of round (PENDING/ACTIVE/COMPLETED/CANCELLED/FINALIZED)|

### Allocation
Tracks an investor's ETH and token allocations in a round

*All monetary values stored with 18 decimals precision*

*Used to manage individual investor positions*

**Notes:**
- security: Values use uint256 to prevent overflow

- security: Struct packing not used due to 256-bit values

- security: Used in mapping for O(1) investor position lookups


```solidity
struct Allocation {
    uint256 etherAmount;
    uint256 tokenAmount;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`etherAmount`|`uint256`|The amount of ETH allocated to the investor (18 decimals)|
|`tokenAmount`|`uint256`|The amount of tokens allocated to the investor (18 decimals)|

## Enums
### RoundStatus
Defines the possible states of an investment round

*Used to control round lifecycle and permissions*


```solidity
enum RoundStatus {
    PENDING,
    ACTIVE,
    COMPLETED,
    CANCELLED,
    FINALIZED
}
```

