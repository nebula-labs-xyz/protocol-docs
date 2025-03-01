# IFlashLoanReceiver
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/921edb5eadadd55e1a3bfce4389f11db33e9cb1a/contracts/interfaces/IFlashLoanReceiver.sol)

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### executeOperation


```solidity
function executeOperation(address token, uint256 amount, uint256 fee, address initiator, bytes calldata params)
    external
    returns (bool);
```

