# Blockchain Basic
Reference: https://updraft.cyfrin.io/courses/blockchain-basics/basics/welcome-to-updraft
## Courses Quiz

**Smart contract：** A smart contract is an agreement that is deployed on a decentralized blockchain. Once deployed, it cannot be altered, and its terms are public.

**Blockchain：** A digital ledger that records transactions across many computers in a secure and decentralized manner.

**How does a blockchain maintain data integrity and security?** By linking each new block to the previous one in a tamper-resistant chain.

**What does the term "Oracle Problem" refer to in the context of smart contracts?** The challenge of smart contracts accessing real-world data.

**What is the role of the transaction fee in Ethereum transactions?** The amount rewarded to the block producer for processing the transaction.

**What is the role of the gas price in Ethereum transactions?** To set the cost per unit of gas specified for the transaction.

**What could be the consequence of a compromised private key in a blockchain system?** The attacker gains full control over the wallet and associated assets.

**What is the purpose of a secret phrase/seed phrase/mnemonic phrase?** It is used to generate wallets, each wallet with their own private key. Access to the secret phrase means access to all generated wallets.

**What is the primary purpose of a blockchain block's hash?** To link the block to the previous one and ensure data integrity.

**What is the primary benefit of rollup solutions?** They reduce transaction fees and congestion by processing transactions off the main chain.

**What is a zero-knowledge proof?** A method for proving knowledge of something without revealing the thing itself.

**What is a "Rollup stage"?** A categorization system used to describe the decentralization and maturity of a rollup.

**What role does a "Sequencer" play in a rollup?** Orders and bundles transactions before they are submitted to the main blockchain.

## Blockchains structure

Reference: https://updraft.cyfrin.io/courses/blockchain-basics/basics/how-do-blockchains-work

## Private key/Public key/Singing

Reference: https://updraft.cyfrin.io/courses/blockchain-basics/basics/signing-ethereum-transactions

Private key generates public key through elliptic curve algorithm.

Sign data with a private key and verify signed data with a public key.

Secret Phrase >> Private key ||| Public key / Address  

## Gas
Reference: https://updraft.cyfrin.io/courses/blockchain-basics/basics/gas-in-depth

**Transaction Fee** (pay to miners) = **Gas Price Per Gas** * **Gas Used**

EIP1559: https://www.youtube.com/watch?v=MGemhK9t44Q

base fee: The minimum fee that must be paid in the transaction.

**Transaction Fee** = (**Block Base Fee Per Gas** + **MaxPriorityFee Per Gas**) * **Gas Used**

**Base Fee** = **Block Base Fee Per Gas** * **Gas Use**  was ultimately burned. Pay the remaining gas in the transaction fee to the miner.

## L1s L2s And Rollups

Reference: https://updraft.cyfrin.io/courses/blockchain-basics/basics/l1s-l2s-and-rollups?lesson_format=video

A **Layer 2** is any application built on outside an L1 blockchain that _hooks back into it_. There are different types of Layer 2, for example **Chainlink**, a decentralized Oracle networks and event indexing networks like **The Graph**, which enable applications to access on-chain data. But the most popular type of L2 is the **rollup**, or **L2 chain**.

**Rollups** are L2 scaling solutions that enable to increase the number of transactions on Ethereum by bundling multiple transactions into one, reducing gas costs.







