# Fair Proof of Stake \(FPoS\)

## **Fair Proof-of-Stake**

A very common consensus algorithm is the "pure" Proof of Stake \(PoS\) model, as proposed by [Nxt](https://nxtwiki.org/wiki/Whitepaper:Nxt). In this model, the choice of account that has the right to generate the next block and receive the corresponding transaction fees is based on the number of tokens in the account. The more tokens that are held in the account, the greater the chance that account will earn the right to generate a block.

In LTO, we are convinced that each participant in the blockchain should participate in the block generation process proportionally his stake: we have decided to use a corrected version of the PoS formula, designed by WAVES, called Fair Proof of Stake.

This is an improved PoS algorithm that makes the choice of block creator fair and reduces vulnerability to the multi-branching attacks, in accordance with the shortcomings of the current algorithm. We analyzed the model of the new algorithm for its correspondence to the stake share and the share of blocks, and the results were positive. Also, the algorithm was analyzed for vulnerability to attacks, and results obtained with the new model were better than with the old one. The attacks’ results for the attacker were not so successful in terms of the profits gained. The number of forks and their length were decreased.

