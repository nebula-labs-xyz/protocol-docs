# UniswapTickMath
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/lender/lib/UniswapTickMath.sol)

Uses Uniswap v4 core libraries for accuracy and consistency, backward compatible with v3

*This library provides functions to calculate prices from Uniswap V3 pool data*


## Functions
### getNextSqrtPriceFromInput

Calculates the next sqrt price after a token input

*Wrapper around Uniswap's SqrtPriceMath library*


```solidity
function getNextSqrtPriceFromInput(uint160 sqrtPriceX96, uint128 liquidity, uint256 amountIn, bool isToken0)
    internal
    pure
    returns (uint160);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sqrtPriceX96`|`uint160`|The current sqrt price in X96 format|
|`liquidity`|`uint128`|The amount of usable liquidity|
|`amountIn`|`uint256`|The amount of token being swapped in|
|`isToken0`|`bool`|Whether the token being swapped in is token0 or token1|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint160`|The next sqrt price after the swap in X96 format|


### getPriceFromSqrtPrice

Calculates price from sqrt price for token0/token1 pair

*Uses different formulas based on which token's price is being calculated*


```solidity
function getPriceFromSqrtPrice(uint160 sqrtPriceX96, bool isToken0, uint256 precision)
    internal
    pure
    returns (uint256 price);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sqrtPriceX96`|`uint160`|The sqrt price in X96 format|
|`isToken0`|`bool`|Whether to calculate price for token0 or token1|
|`precision`|`uint256`|The desired precision (e.g., 1e6, 1e18)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|The calculated price in the specified precision|


### getRawPrice

Gets the raw price from a Uniswap V3 pool using TWAP

*Calculates time-weighted average price using tick cumulative data*


```solidity
function getRawPrice(IUniswapV3Pool pool, bool isToken0, uint256 precision, uint32 twapPeriod)
    internal
    view
    returns (uint256 rawPrice);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`pool`|`IUniswapV3Pool`|The Uniswap V3 pool to query|
|`isToken0`|`bool`|Whether to get the price for token0 or token1|
|`precision`|`uint256`|The desired precision for the returned price|
|`twapPeriod`|`uint32`|The period over which to calculate the TWAP (in seconds)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`rawPrice`|`uint256`|The calculated raw price with the specified precision|


### getSqrtPriceAtTick

Gets the sqrt price in X96 format for a given tick

*Wrapper around Uniswap's TickMath library*


```solidity
function getSqrtPriceAtTick(int24 tick) internal pure returns (uint160);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tick`|`int24`|The tick to convert to a sqrt price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint160`|The sqrt price in X96 format for the given tick|


