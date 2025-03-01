# YodaMath
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/lender/lib/YodaMath.sol)

# Security Review: YodaMath Contract

## 1. Overview

The YodaMath contract implements fixed-point mathematical operations for financial calculations, focusing on compound interest and rate conversions. It's designed as a utility library for the Lendefi protocol to handle precise financial computations that aren't natively supported in Solidity.

## 2. Contract Architecture

The contract uses two precision levels:
- **WAD**: 10^6 (1e6) - 6 decimal places of precision
- **RAY**: 10^27 (1e27) - 27 decimal places of precision for higher accuracy

Core functionality includes:
1. Basic fixed-point arithmetic operations (`rmul`, `rdiv`)
2. Exponentiation with high precision (`rpow`)
3. Interest calculation utilities
4. Rate conversion functions

## 3. Detailed Analysis

### 3.1. Core Mathematics Implementation

**Fixed-Point Multiplication (`rmul`)**
```solidity
function rmul(uint256 x, uint256 y) public pure returns (uint256 z) {
    z = ((x * y) + RAY / 2) / RAY;
}
```
- Uses RAY precision (10^27)
- Includes rounding with `+ RAY / 2` to minimize precision loss
- Correctly handles multiplication overflow with Solidity 0.8.x built-in checks

**Fixed-Point Division (`rdiv`)**
```solidity
function rdiv(uint256 x, uint256 y) public pure returns (uint256 z) {
    z = ((x * RAY) + y / 2) / y;
}
```
- Properly scales numerator by RAY before division
- Includes rounding with `+ y / 2` to minimize precision loss
- No explicit zero divisor check (relies on Solidity 0.8.x revert)

**Binary Exponentiation (`rpow`)**
```solidity
function rpow(uint256 x, uint256 n) public pure returns (uint256 z) {
    z = RAY;
    if (n == 0) { return z; }
    if (n == 1) { return x; }
    
    while (n > 0) {
        if (n & 1 == 1) { z = rmul(z, x); }
        x = rmul(x, x);
        n = n >> 1;
    }
}
```
- Implements the efficient square-and-multiply algorithm (O(log n) complexity)
- Properly handles base cases (n=0, n=1)
- Algorithm is mathematically sound and gas-efficient

### 3.2. Financial Utilities

**Annual Rate Conversion**
```solidity
function annualRateToRay(uint256 rate) public pure returns (uint256 r) {
    r = RAY + rdiv((rate * RAY) / WAD, SECONDS_PER_YEAR_RAY);
}
```
- Correctly converts annual percentage rate to per-second rate in RAY precision
- Handles the precision difference between WAD and RAY

**Interest Accrual**
```solidity
function accrueInterest(uint256 principal, uint256 rateRay, uint256 time) public pure returns (uint256) {
    return rmul(principal, rpow(rateRay, time));
}
```
- Implements compound interest formula: principal * (rate ^ time)
- Uses binary exponentiation for efficient calculation

**Breakeven Rate Calculation**
```solidity
function breakEvenRate(uint256 loan, uint256 supplyInterest) public pure returns (uint256) {
    return ((WAD * (loan + supplyInterest)) / loan) - WAD;
}
```
- Calculates minimum borrow rate needed for protocol profitability
- Result is in WAD precision (not RAY)

## 4. Strengths

1. **Mathematical Precision**: Uses high precision (RAY = 10^27) to minimize rounding errors in financial calculations.

2. **Algorithmic Efficiency**: The binary exponentiation algorithm is implemented correctly and efficiently.

3. **Clear Documentation**: Functions have descriptive NatSpec comments explaining their purpose and parameters.

4. **Rounding Approach**: Consistent use of rounding (`+ divisor / 2`) in division operations to minimize precision loss.

5. **Gas Efficiency**: The binary exponentiation algorithm is optimized for gas consumption compared to naive approaches.




**Inherits:**
[IYODAMATH](/contracts/interfaces/IYodaMath.sol/interface.IYODAMATH.md)

**Author:**
Nebula Holding Inc

Compunding and beyond

**Note:**
security-contact: security@nebula-labs.xyz


## State Variables
### WAD
*base scale*


```solidity
uint256 internal constant WAD = 1e6;
```


### RAY
*ray scale*


```solidity
uint256 internal constant RAY = 1e27;
```


### SECONDS_PER_YEAR_RAY
*seconds per year on ray scale*


```solidity
uint256 internal constant SECONDS_PER_YEAR_RAY = 365 * 86400 * RAY;
```


## Functions
### rmul

*rmul function*


```solidity
function rmul(uint256 x, uint256 y) public pure returns (uint256 z);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`uint256`|amount|
|`y`|`uint256`|amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`z`|`uint256`|value|


### rdiv

*rdiv function*


```solidity
function rdiv(uint256 x, uint256 y) public pure returns (uint256 z);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`uint256`|amount|
|`y`|`uint256`|amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`z`|`uint256`|value|


### rpow

*rpow function - Calculates x raised to the power of n with RAY precision*


```solidity
function rpow(uint256 x, uint256 n) public pure returns (uint256 z);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`uint256`|base value (in RAY precision)|
|`n`|`uint256`|exponent|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`z`|`uint256`|result (in RAY precision)|


### annualRateToRay

*Converts rate to rateRay*


```solidity
function annualRateToRay(uint256 rate) public pure returns (uint256 r);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|rate|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`r`|`uint256`|rateRay|


### accrueInterest

*Accrues compounded interest*


```solidity
function accrueInterest(uint256 principal, uint256 rateRay, uint256 time) public pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`principal`|`uint256`|amount|
|`rateRay`|`uint256`|rateray|
|`time`|`uint256`|duration|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount (pricipal + compounded interest)|


### getInterest

*Calculates compounded interest*


```solidity
function getInterest(uint256 principal, uint256 rateRay, uint256 time) public pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`principal`|`uint256`|amount|
|`rateRay`|`uint256`|rateray|
|`time`|`uint256`|duration|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|amount (compounded interest)|


### breakEvenRate

*Calculates breakeven borrow rate*


```solidity
function breakEvenRate(uint256 loan, uint256 supplyInterest) public pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`loan`|`uint256`|amount|
|`supplyInterest`|`uint256`|amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|breakeven borrow rate|


