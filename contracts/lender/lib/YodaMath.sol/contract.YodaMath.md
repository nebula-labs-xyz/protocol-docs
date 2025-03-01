# YodaMath
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/lender/lib/YodaMath.sol)

**Inherits:**
[IYODAMATH](/contracts/interfaces/IYodaMath.sol/interface.IYODAMATH.md)

**Author:**
Nebula Labs Inc

Compunding and beyond

**Note:**
security-contact: security@nebula-labs.xysz


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


