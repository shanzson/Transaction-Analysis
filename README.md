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
![](/images/image1.png)

-The attacker had called this function by passing the USDC contract address(here address(varg1)) and the victim's address(here address(varg2))
- When we look inside this function, on line: 209 we find that the fullExit function is called from the _chef
![](/images/image2.png)
```
v14, v15 = _chef.fullExit(address(varg0), v10, (block.number << 64) + ((block.timestamp << 32) + uint32(0xffffffff - (keccak256(v2) >> 224) + 1)), v10, address(varg1), msg.sender, MEM[MEM[64]]).gas(msg.gas);
```

- This is the point where the actual attack got triggered. Now when we look at _chef, we get the follwing address 
![](/images/image3.png)
- On searching this address on [etherscan](https://etherscan.io/address/0xE51e9bFf39baA85bD74865254D647188e1672612#code), it turns out to be a proxy contract
with the following [implementation](https://library.dedaub.com/contracts/Ethereum/0xb11Ce4677929f8B57B90f08A1319E4D31642b25b/decompiled?line=1)
- On further analysis, it can be seen that it is likely that the victim might have approved the _chef contract for maximum number of USDC tokens while interacting with the _chef contract. The attacker then most likely exploited a bug in the chef contract, while tricking the _chef contract into transferring the USDC from the victim to the attacker instead.
