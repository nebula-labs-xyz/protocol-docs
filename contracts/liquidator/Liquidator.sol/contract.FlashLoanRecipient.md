# FlashLoanRecipient
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/liquidator/Liquidator.sol)


## 1. Overview

The `FlashLoanRecipient` contract serves as a liquidation bot for the Lendefi Protocol, using Balancer flash loans to liquidate underwater positions and swap collateral assets to USDC via Uniswap V3. It integrates with multiple DeFi protocols to execute profitable liquidations in a single atomic transaction.

## 2. Architecture

The contract implements the following system architecture:

1. **Core Components**:
   - Balancer Vault integration for flash loans
   - Uniswap V3 Router for collateral swaps
   - Lendefi Protocol interaction for liquidations
   - Governance token requirement mechanism

2. **Key Functions**:
   - `liquidate(address)`: Initiates liquidation of underwater positions
   - `receiveFlashLoan()`: Executes liquidation strategy within flash loan callback
   - `uniswapV3()`: Swaps received collateral to USDC
   - `withdraw()`: Extracts profits and governance tokens

3. **Security Measures**:
   - `onlyOwner` access control for critical functions
   - `onlyVault` modifier restricting flash loan callbacks
   - Slippage protection on swaps (99% of expected value)
   - Profit verification before repaying flash loans

## 3. Strengths

### 3.1 Operational Security
- Well-implemented access control using Ownable pattern
- Proper validation of flash loan callback to prevent unauthorized execution
- Immutable contract references reduce tampering risk
- Use of SafeERC20 for token transfers

### 3.2 Transaction Efficiency
- Single atomic transaction for entire liquidation process
- Efficient collateral processing with batch swap optimization
- Slippage protection to prevent sandwich attacks

### 3.3 Implementation Quality
- Clear function documentation
- Proper error handling with meaningful error messages
- Logical segmentation of functionality
- Safe approval pattern with `forceApprove`

## 4. Vulnerabilities and Concerns

### 4.1 High Risk

**MEV Vulnerability (H-01)**:
- Liquidation transactions are visible in the mempool and susceptible to front-running
- Sophisticated MEV bots could extract value by observing and racing your liquidations
- **Recommendation**: Consider using private mempools or flashbots to mitigate front-running



## 5. Code Analysis

### 5.1 Flash Loan Implementation
```solidity
function receiveFlashLoan(
    IERC20[] memory tokens,
    uint256[] memory amounts,
    uint256[] memory feeAmounts,
    bytes memory userData
) external override onlyVault {
    // [...] implementation
}
```
The flash loan callback executes the core liquidation logic:
1. Identifies the target position and collateral assets
2. Approves and executes the liquidation call
3. Swaps resulting collateral to USDC
4. Verifies profitability before repaying the loan

This implementation correctly handles the flash loan pattern, but the marked line with "🚩" flags is concerning - this is where the actual liquidation occurs, and adequate error handling should be present.

### 5.2 Asset Swapping
```solidity
function uniswapV3(address asset, uint256 swapAmount, uint256 amountOutMin) internal returns (uint256) {
    // [...] implementation
}
```
The swap function properly uses price information from the protocol's oracles and includes slippage protection, but the fixed pool fee tier (3000) may not be optimal for all assets.

## 6. Gas Optimization

The contract could be optimized for gas usage:
- Consider using unchecked blocks for arithmetic operations when safe
- Optimize array handling in asset processing
- Consider batch swaps instead of multiple individual swaps when possible

## 7. Recommendations

1. **Enhanced Position Selection**:
   - Add ability to target any position, not just the most recent one
   - Consider implementing a "most profitable position" selection algorithm

2. **Improved Risk Management**:
   - Add configurable slippage tolerance parameters
   - Implement adaptive fee tier selection for Uniswap swaps
   - Add circuit breakers for extreme market conditions

3. **Operational Improvements**:
   - Add comprehensive events for monitoring
   - Implement a profit estimation function before executing liquidation
   - Add support for partial liquidations if protocol allows

4. **MEV Protection**:
   - Integrate with private transaction services like Flashbots
   - Consider time-delayed revealing of liquidation targets


**Inherits:**
[IFlashLoanRecipient](/contracts/vendor/@balancer-labs/v2-interfaces/contracts/vault/IFlashLoanRecipient.sol/interface.IFlashLoanRecipient.md), Ownable

**Author:**
alexei@nebula-labs(dot)xyz


**Note:**
security-contact: security@nebula-labs.xyz


## State Variables
### USDC_INSTANCE
*USDC token instance*


```solidity
IERC20 public immutable USDC_INSTANCE;
```


### BALANCER_VAULT
mainnet address 0xBA12222222228d8Ba445958a75a0704d566BF2C8

*balancer vault instance*


```solidity
IVault public immutable BALANCER_VAULT;
```


### PROTOCOL_INSTANCE
*Lendefi instance*


```solidity
IPROTOCOL public immutable PROTOCOL_INSTANCE;
```


### TOKEN_INSTANCE
*gov token instance*


```solidity
IERC20 public immutable TOKEN_INSTANCE;
```


### UNISWAP_ROUTER
*Uniswap router instance*


```solidity
ISwapRouter public immutable UNISWAP_ROUTER;
```


## Functions
### onlyVault

*Throws if called by any account other than the owner.*


```solidity
modifier onlyVault();
```

### constructor


```solidity
constructor(address usdc, address nebula, address balancerVault, address uniswapRouter, address govToken)
    Ownable(msg.sender);
```

### liquidate

*Liquidates borrower positions in the Nebula protocol*


```solidity
function liquidate(address account) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|address|


### withdraw

*withdraws profit in USDC, and gov tokens required to make the liquidation (20_000e18)*


```solidity
function withdraw() external onlyOwner;
```

### receiveFlashLoan

*receives Balancer flash loan*


```solidity
function receiveFlashLoan(
    IERC20[] memory tokens,
    uint256[] memory amounts,
    uint256[] memory feeAmounts,
    bytes memory userData
) external override onlyVault;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokens`|`IERC20[]`|IERC20 instances array|
|`amounts`|`uint256[]`|corresponding amounts array|
|`feeAmounts`|`uint256[]`|corresponding fee amounts array|
|`userData`|`bytes`|borrower address|


### makeFlashLoan

*triggers Balancer flash loan*


```solidity
function makeFlashLoan(IERC20[] memory tokens, uint256[] memory amounts, bytes memory userData) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokens`|`IERC20[]`|IERC20 instances array|
|`amounts`|`uint256[]`|corresponding amounts array|
|`userData`|`bytes`|borrower address|


### uniswapV3

*perform asset swap to USDC*


```solidity
function uniswapV3(address asset, uint256 swapAmount, uint256 amountOutMin) internal returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|address|
|`swapAmount`|`uint256`|amount of asset you want to swap|
|`amountOutMin`|`uint256`|how much to get back in USDC|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amountOut of the swap|


### _checkVault

*Throws if the sender is not the balancer Vault.*


```solidity
function _checkVault() internal view virtual;
```

## Errors
### CustomError
*CustomError message*


```solidity
error CustomError(string msg);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`msg`|`string`|error desciption message|

