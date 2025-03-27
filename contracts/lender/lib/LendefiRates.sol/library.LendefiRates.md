# LendefiRates
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/d0b15d8d57415f38e3db367bb9e72ba910580c33/contracts/lender/lib/LendefiRates.sol)

Library for core calculation functions used by Lendefi protocol

*Contains math-heavy functions to reduce main contract size*


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
function rmul(uint256 x, uint256 y) internal pure returns (uint256 z);
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
function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z);
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
function rpow(uint256 x, uint256 n) internal pure returns (uint256 z);
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
function annualRateToRay(uint256 rate) internal pure returns (uint256 r);
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
function accrueInterest(uint256 principal, uint256 rateRay, uint256 time) internal pure returns (uint256);
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
|`<none>`|`uint256`|amount (principal + compounded interest)|


### getInterest

*Calculates compounded interest*


```solidity
function getInterest(uint256 principal, uint256 rateRay, uint256 time) internal pure returns (uint256);
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
function breakEvenRate(uint256 loan, uint256 supplyInterest) internal pure returns (uint256);
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


### calculateDebtWithInterest

Calculates debt with accrued interest


```solidity
function calculateDebtWithInterest(uint256 debtAmount, uint256 borrowRate, uint256 timeElapsed)
    internal
    pure
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtAmount`|`uint256`|Current debt amount|
|`borrowRate`|`uint256`|Borrow rate for the tier|
|`timeElapsed`|`uint256`|Time since last accrual|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total debt with interest|


### getSupplyRate

Calculates supply rate based on protocol metrics


```solidity
function getSupplyRate(
    uint256 totalSupply,
    uint256 totalBorrow,
    uint256 totalSuppliedLiquidity,
    uint256 baseProfitTarget,
    uint256 usdcBalance
) internal pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalSupply`|`uint256`|Total LP token supply|
|`totalBorrow`|`uint256`|Current borrowed amount|
|`totalSuppliedLiquidity`|`uint256`|Total liquidity supplied|
|`baseProfitTarget`|`uint256`|Protocol profit target|
|`usdcBalance`|`uint256`|Current USDC balance|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Supply rate in parts per million|


### getBorrowRate

Calculates borrow rate for a tier


```solidity
function getBorrowRate(
    uint256 utilization,
    uint256 baseBorrowRate,
    uint256 baseProfitTarget,
    uint256 supplyRate,
    uint256 tierJumpRate
) internal pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`utilization`|`uint256`|Protocol utilization rate|
|`baseBorrowRate`|`uint256`|Base borrow rate|
|`baseProfitTarget`|`uint256`|Protocol profit target|
|`supplyRate`|`uint256`|Current supply rate|
|`tierJumpRate`|`uint256`|Jump rate for the tier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Borrow rate in parts per million|


