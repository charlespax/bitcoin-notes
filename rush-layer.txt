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

* Sidechains
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

## Rush block

"""
Hello
"""


# Notes
These are notes while writing this document.

* Can the sidechain be implemented as a series of coinjoin transactoins?
* There are two options here: 1) One Rush block per Bitcoin block. 2) One Rush block each time a block nonce is found that meets the Rush difficulty, but not necessarily the Bitcoin difficulty. The simple implementation is to have one Rush block per Bitcoin block.

# Icebox Notes
These notes were generated while writing this document and ar not currently relevant, but may become so.
* A difficulty window can be used, so the main chain nonce cannot be used. Then there is no incentive to ignore other miner's sub-blocks if you are also a miner.
