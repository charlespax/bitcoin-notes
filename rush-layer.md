                    Bitcoin Rush Network

# Description
This paper describes the Bitcoin Rush Network a Bitcoin layer-2 network designed for large-scale off-chain transactions.

# The Problem
There are several problems Bitcoin faces.

* Transaction confirmation times. Ten minutes per block. One hour for full confirmation.
* Limited transaction capacity. Each block is limited to 1 MB
* Taking transaction and fees off-chain reduces miner incentive to mine Bitcoin

# Ingredients
The Bitcoin Rush Network utilizes several Blockchian technologies.

* Sidechain
* Merged mining
* Two-way peg

## Sidechains

## Merged mining
I'm not sure if merged mining" is the correct term here. Rush might do something a little different.

On the Rush Network a block is created by whoever created a Bitcoin block. When a miner solves the Bitcoin mainchain puzzle and proposes a new block, they are able to create a new Rush block. This is done by including a transaciton in the Bitcoin network that includes the Rush block header (or maybe just a hash). This makes the Rush network blockchain as immutable as the Bitcoin chain, but does not necessarily provide the same level of security.

## Two-way peg
While a two-way peg is not strictly necessary for a Rush network implementation, a two-way peg would allow Rush network to act as an integrated Bitcoin layer. There would be no need for an external exchange or trusted third party.

# The Rush Network
The Rush network is basically a Bitcoin sidechain that reuses as much of the Bitcoin infrastructure as possible.

Rush block header hashes are posted to the Bitcoin network as transactions. Any participant may post a proposed block header to the Bitcoin blockchain, but only one will be chosen as the try header. How? Maybe by its position in the merlek tree. Because the miner that publishes a new block can define its contents, it has the ultimate power to include whichever Rush block header transaction it cares to. In particular, it can creat its own transaction for inclusion. Thus, during Rush network bootstrapping no special Bitcoin miner participation is necessary as they treat the Rush block transaction as a normal transaction. However, there is an incentive for Bitcoin miners to also merge mine Rush.

A Rush transaction is confirmed if it is included in the merkel tree of a block that is included in the Bitcoin network and valid on the Rush network. How do we guarantee a block is valid on the Rush network?

A Rush header includes the pervious bitcoin block hash and the pervious Rush block hash. It would be good if we can reference this value from the current block header. This way we don't need to track previous Bitcoin blocks; just see if the previous Bitcoin block header as defined in the Rush block matches the previous bitcoin block header as defined int he current Bitrcoin block.

How does the Rush network reject bad Rush blocks? Bitcoin blocks with invalid transactions are rejected by fellow miners. Is there a way to make a Rush block transaction valid if and only if there is an invalid Rush transaction?

What happens if a Bitcoin minder publishes an invalid Rush header?

What does a Rush node do? Listen on the Rush network for transactions. Validates transactions. Rebroadcast valid transactions. Listens to the Bitcoin network for block headers and and Rush header transactions.

The Rush block header should include a Bitcoin transaction id for the pervious Rush block header transaction on the Bitcon network.

# Structure

## Bitcoin block anatomy

```
Block
 * Header
  - Version
  - Merkle root
  - Difficulty
  - Previous block hash
  - Timestamp
  - Nonce
 * Transaction count
  - Total bitcoin in + coinbase transaction
 * Block content
  - Coinbase Tx
  - Bitcoin Tx
    > Technical data
      * Version
      * Number of inputs
      * Lock time
      * Number of outputs
    > Inputs
      * Previous Tx hash / output index
      * Private unlick script
      * Script length
    > Outputs
      * Amount
      * Public locking scrit
      * script length
```

## Rush block anatomy
```
Block
 * Header
  - Rush version
  - Rush merkle root
  - Pervious Rush block hash
  - Previous Bitcoin block hash
  - Bitcoin Tx id of the previous Rush block
  - Nonce of Bitcoin block
 * Transaction count
  - Total Rush in
 * Block content
  - UNUSED: Coinbase Tx
  - Rush Tx
    > Technical data
      * Version
      * Number of inputs
      * Lock time
      * Number of outputs
    > Inputs
      * Previous Tx hash / output index
      * Private unlick script
      * Script length
    > Outputs
      * Amount
      * Public locking scrit
      * script length
```
The structure of a Rush block is very similar to a Bitcoin block. Timestamp is not necessary because we use the timestamp from the Bitcoin block in which the Rush block header appears. Nonce and Difficulty are not used because they are only relevant to Bitcoin. Coinbase Tx is not used because there is no coinbase in Rush.

# Notes
These are notes while writing this document.

* Can the sidechain be implemented as a series of coinjoin transactoins?
* There are two options here: 1) One Rush block per Bitcoin block. 2) One Rush block each time a block nonce is found that meets the Rush difficulty, but not necessarily the Bitcoin difficulty. The simple implementation is to have one Rush block per Bitcoin block.
* We need a mechanism of exchanging between Bitcoin and Rush. Two-way peg? Maybe take the Rootstock approach. We can have one method that works with the current Bitcoin system without modification. Later replace it with with a more cryptographically secure method.
* Pick the transaction in the Bitcoin block that includes the lowest Bitcoin hash value. This might allow us to bootsrtap with only a tiny amount of mining power. The miner who finds a golden nonce will be able to have the hash with the lowest difficulty.
* What happens if a minder chooses to not include transactions with Rush block info? Hopefuly, another miner will take it in a bitcoin blck they mine. It is the same method for any censored Bitcoin transaction.
* If a miner includes a Rush block header for an invalid block, the next successful miner will simply ignore it and link to the most recent valid Rush block. This could be from the same Bitcoin block. We would just look at the Rush block transaction with the next lowest Bitcoin hash.
* Lots of people can broadcast Bitcoin Rush block transactions with the lowest hash value they found. They might choose not to if they think it is not sufficiently low enough. They might just quit looking as soon as they hear one on the network that they think is much lower than what they might do. However, they are still looking for the Bitcoin block hash, so this is free anyway.
* People would not spam the Bitcoin network with Rush block transactions because there is a fee. But anyone can try if they want.
* There is an incentive for a Rush-enabled miner to simply ignore all Rush block transactions from other miners. However, they will miss out on the fees from these miners. They may as well include them in a block if the transaction fee is good enough.
* It may become customary to add a Rush transaction as the last transaction in a block. A miner who finds a golden nonce they will immeditely add the golden nonce hash to their block and broadcast it. The miner is likely to include Rush block transactions from other miners if they know they have a lower hash value.
* Rush-enabled miners are likely to include all Rush block transactions that have a greater value than one they have found themselves.
* If a miner broadcasts an Bitcin transaction for an invalid Rush block, another miner can include it in their own block and collect the fee. The Rush network will be ignoring that invalid block anyway.
* If there is a two-way peg, it might be good to have the cross-chain transactions be linked to the Rush block transaction. Maybe this would help if avoid any transactions that roll back the cross-chain exchange.
* I wonder if Rush transactions can be valid Bitcoin transactions, but with some other information, so they are processed differently. This is how soft forking works.

# Icebox Notes
These notes were generated while writing this document and ar not currently relevant, but may become so.
* A difficulty window can be used, so the main chain nonce cannot be used. Then there is no incentive to ignore other miner's sub-blocks if you are also a miner.
