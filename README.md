# Transaction Analysis
## [Transaction to analyze](https://etherscan.io/tx/0xb4e17296635461d2ca5a26a9fa4aaeb24e073e0eb83a2fc6a207c1f97767a1b9)

## Transaction details
- Attacker address- 0xa6dc0fe6c94e7f2f34fcc63e05c59707a13942da
- Contract created- 0x376a2a023a105bc2e19ce19ad275b9bbbcb23e1a
- Victim's address- 0xfad1cb78101cf717cf97663e681eb87291dfcbe2

The attacker drained the victim's account for 4207352.56 USDC.

## Attacker created the following contract
![](/images/attack1.png)

## Attacker's fund flows 
![](/images/attack2.png)

## Attack visualized
![](/images/attack3.png)

## Transaction Call Trace
![](/images/trace.png)
- [View trace](https://tx.eth.samczsun.com/ethereum/0xb4e17296635461d2ca5a26a9fa4aaeb24e073e0eb83a2fc6a207c1f97767a1b9)

## Technical analysis
- According to the call trace, the attacker had first created a malicious contract and had called the following function
![](/images/attack1.png)
The attacker had called this function by passing the USDC contract address(here address(varg1)) and the victim's address(here address(varg2))
- When we look inside this function, on line: 209 we find that the fullExit function is called from the _chef
```
v14, v15 = _chef.fullExit(address(varg0), v10, (block.number << 64) + ((block.timestamp << 32) + uint32(0xffffffff - (keccak256(v2) >> 224) + 1)), v10, address(varg1), msg.sender, MEM[MEM[64]]).gas(msg.gas);
```
![](/images/attack2.png)
