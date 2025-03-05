# IYODAMATH
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/IYodaMath.sol)

**Author:**
Nebula Labs LLC

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### rmul

*rmul function*


```solidity
function rmul(uint256 x, uint256 y) external pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`uint256`|amount|
|`y`|`uint256`|amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|value|


### rdiv

*rdiv function*


```solidity
function rdiv(uint256 x, uint256 y) external pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`uint256`|amount|
|`y`|`uint256`|amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|value|


### rpow

*rpow function*


```solidity
function rpow(uint256 x, uint256 n) external pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`uint256`|amount|
|`n`|`uint256`|amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|value|


### annualRateToRay

*Converts rate to rateRay*


```solidity
function annualRateToRay(uint256 rate) external pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|rate|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|rateRay|


### accrueInterest

*Accrues compounded interest*


```solidity
function accrueInterest(uint256 principal, uint256 rateRay, uint256 time) external pure returns (uint256);
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
function getInterest(uint256 principal, uint256 rateRay, uint256 time) external pure returns (uint256);
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
function breakEvenRate(uint256 loan, uint256 supplyInterest) external pure returns (uint256);
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


