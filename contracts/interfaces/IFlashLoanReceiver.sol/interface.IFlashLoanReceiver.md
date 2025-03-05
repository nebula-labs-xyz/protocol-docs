# IFlashLoanReceiver
[Git Source](https://github.com/nebula-labs-xyz/lendefi-protocol/blob/aaed57cb7ee1c677c0c943d32a39d9411c489fc9/contracts/interfaces/IFlashLoanReceiver.sol)

**Note:**
security-contact: security@nebula-labs.xyz


## Functions
### executeOperation


```solidity
function executeOperation(address token, uint256 amount, uint256 fee, address initiator, bytes calldata params)
    external
    returns (bool);
```

