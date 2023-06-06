# Quick Start: Code Samples

## Retrieving the KLAY/USD pair price

### Solidity Example

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.16;

import "./KlayOracleInterface.sol";

contract OracleConsumerSample {
    address public immutable oracleAddress;

    uint256 public klayOutput;

    constructor(address _oracleAddress) {
        require(_oracleAddress != address(0));
        oracleAddress = _oracleAddress;
    }

    function swapEthtoKlay() public returns (bool) {
        KlayOracleInterface oracle = KlayOracleInterface(oracleAddress);

        bool replied = oracle.newOracleRequest(
            this.swap.selector,
            address(this)
        );

        return replied;
    }

    function swap(uint256 _klayOutput) public {
        klayOutput = _klayOutput;
        //Swap eth to klay
    }
}
```

### JavaScript example

```javascript
const oracleAddress = "0xbc884088e406422a3ef39aedd1c546de7ac4be7c";

const abi = [
  {
      "inputs": [],
      "name": "latestRound",
      "outputs": [
        {
          "internalType": "bytes32",
          "name": "answer",
          "type": "bytes32"
        },
        {
          "internalType": "uint256",
          "name": "roundTime",
          "type": "uint256"
        },
        {
          "internalType": "uint256",
          "name": "timestamp",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    }
]

const provider = new ethers.providers.JsonRpcProvider("https://api.baobab.klaytn.net:8651");

const oracleContract = new ethers.Contract(oracleAddress, abi, provider);

const [answer, roundTime, timestamp] = await oracleContract.latestRound()
```
