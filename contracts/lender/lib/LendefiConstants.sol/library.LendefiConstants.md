# LendefiConstants
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/lender/lib/LendefiConstants.sol)

**Author:**
alexei@nebula-labs(dot)xyz

Shared constants for Lendefi and LendefiAssets contracts

**Note:**
copyright: Copyright (c) 2025 Nebula Holding Inc. All rights reserved.


## State Variables
### WAD
Standard decimals for percentage calculations (1e6 = 100%)


```solidity
uint256 internal constant WAD = 1e6;
```


### USDC_ETH_POOL
Address of the Uniswap V3 USDC/ETH pool with 0.05% fee tier


```solidity
address internal constant USDC_ETH_POOL = 0x88e6A0c2dDD26FEEb64F039a2c41296FcB3f5640;
```


### PAUSER_ROLE
Role identifier for users authorized to pause/unpause the protocol


```solidity
bytes32 internal constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
```


### MANAGER_ROLE
Role identifier for users authorized to manage protocol parameters


```solidity
bytes32 internal constant MANAGER_ROLE = keccak256("MANAGER_ROLE");
```


### UPGRADER_ROLE
Role identifier for users authorized to upgrade the contract


```solidity
bytes32 internal constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
```


### CIRCUIT_BREAKER_ROLE
Role identifier for users authorized to activate/deactivate circuit breakers


```solidity
bytes32 internal constant CIRCUIT_BREAKER_ROLE = keccak256("CIRCUIT_BREAKER_ROLE");
```


### UPGRADE_TIMELOCK_DURATION
Duration of the timelock for upgrade operations (3 days)


```solidity
uint256 internal constant UPGRADE_TIMELOCK_DURATION = 3 days;
```


### MAX_LIQUIDATION_THRESHOLD
Max liquidation threshold, percentage on a 1000 scale


```solidity
uint16 internal constant MAX_LIQUIDATION_THRESHOLD = 990;
```


### MIN_THRESHOLD_SPREAD
Min liquidation threshold, percentage on a 1000 scale


```solidity
uint16 internal constant MIN_THRESHOLD_SPREAD = 10;
```


### MAX_ASSETS
Max assets supported by platform


```solidity
uint32 internal constant MAX_ASSETS = 3000;
```


