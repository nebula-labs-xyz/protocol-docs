# FlashLoanRecipient
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/liquidator/Liquidator.sol)


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

