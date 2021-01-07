## Proof-of-Stake, by giaki3003 (Part 2)

# States, epochs, and how they tie in the Proof-of-Stake game theory

In [part 1](https://giaki3003.tech/proof-of-stake-by-giaki3003-part-1) of this series, we became familiar with state-less/epoch-less PoS implementations; namely, the objective of said implementations, how they are generally structured and some projects which currently use them. 
For this part, the objective is defining, categorizing and describing state/epoch based Proof-of-Stake implementations. 

## What are states/epochs?

By generalizing the term **states/epochs**, I'd like to make the two most used terms in Proof-of-Stake papers and academic research more *understandable* to people who haven't been following this particular sphere of crypto-theory. 
At the core, states and epochs serve the purpose of additional chain-independent **context** used for specific rule enforcement and validation. 

> With the term **context**, I refer to any additional data which doesn't have any use outside of the Proof-of-Stake protocol, and does not actually relate to any data which may come from the chain itself.

Essentially, a state-based/epoch-based Proof-of-Stake implementation uses external context for validating how economic value generates work. The catch here is that, due to this characteristic, an underlying chain infrastructure isn't a requirement for these protocols to function; this is what other papers refer to as a BFT-like implementation. 

Be careful, though. This *doesn't mean* that all state/epoch protocols cannot function in a traditional chain-like system. Blocks produced following these protocols heavily abstract rules for validation of said blocks, but do not impede the creation of a chain of blocks. How blocks are arranged is a wholly different part of the story, and not related to protocol specifics.

So, for example, in NXTs Proof-of-Stake implementation, we use two block fields for validation: a work-related parameter (called *base-target-value*, or **Tvb**) which accounts for work-related block validation, and a signature pertaining to the block itself (often referred to as *generating-signature*, **Sg**); As tracing parallels to existing and more mature systems can be a great aid to understanding this, think of the **Tvb** as the "difficulty" field we see in Proof-of-Work systems, and think of the **Sg** as the Proof-of-Work "hash" used to validate a block-primitive against the "difficulty" target.

A system like this is very efficient when it comes at *block-primitive propagation*, a fundamental part of cryptocurrency network design. We will get back at this part later on in the series.

Previous iterations of the so-called state-less/epoch-less PoS protocols do not require *per-se* any additional **context**, as the subset of rules which need to be validated can be calculated based off "block-primitive"-independent chain data look-ups. We will get back at why these rules differ and how this difference impacts projects in another part of the series.

### Notice

While referring to implementations following this criteria can be beneficial for indexing and dividing PoS protocols in different sub-groups, I think it is important to notice how  there are some cases where (although not strictly because of necessity) "PoS-only" extra bytes are added to block primitives in state-less/epoch-less Proof-of-Stake protocols.
The best example I can give of this comes from [QTUM's block primitive,](https://github.com/qtumproject/qtum/blob/master/src/primitives/block.h#L32) where two specific fields are added, and used for chain validation.
What is surprising is that this same protocol works fine without said fields; in fact, these have been added to answer a specific need - they enable **light-client** validation of sorts - which we'll look at in another part of the series.

## Reasoning behind this approach

Adding an amount of data to *anything* implies a **heavier, higher footprint** system.
Cryptocurrency network design must heavily favor decentralization, and thus a **lower-footprint, lighter and more efficient implementation** should be a key objective for anyone designing alternative consensus systems of any type.
Bitcoin was designed with this characteristic in mind, and I personally support this choice.
Splitting protocols in two groups gives us an immediate way to recognize this, while also helping us keep track of key differences.

*This however does **not** consider each individual networks usage of each protocol; as the QTUM example shows, protocol is heavily influenced by each codebase we look at, so indexing by project rather than by protocol would result in a heavily counter-productive miasma of information.*

We will now focus on protocols which have **mandatory** state/epoch requirements for security assumptions and rule validation.

### State/epoch-based UTXO-based PoS

Interestingly enough, I am not aware of any UTXO-based Proof-of-Stake protocol which extensively uses states/epochs for validation and rule enforcement. 
This may be due to how primitive PoS implementations were first developed when the UTXO system was a predominant model for alternative cryptocurrency design, but this is only a supposition of mine. 
With NXT, for example, we are able to actually pin-point the moment when account-based networks became prevalent in altcoin design, thanks to its "n-based" (traditionally only implemented in UTXO models) approach tied to one of the first account systems ever created. 
Unfortunately, we cannot say the same here; nevertheless, this small parenthesis does provide some insight on how the UTXO model has lost traction in time, despite the success of its first iteration, found in Bitcoin.

### State/epoch-based Account-based PoS

Almost every single document, paper and work I've been able to put my hands on since starting this research refers to the accounting system which has grown so popular for altcoin design. If anything, it can serve as a great metric for measuring the performance of said model in the cryptocurrency sphere; so many new projects choose it over the UTXO model, and I'll try my best at identifying the key reasons, benefits and drawbacks of both models in another part of the series.

#### *Slasher*

> The greater your signing privilege, the greater your chances at producing a valid block.

Written in 2014, Slasher is the first approach at a **punitive Proof-of-Stake** protocol. Economic value's work does not actually directly lead to the creation of blocks, but gives signing privileges on future blocks based off the last *commitment block*.

A commitment block, in this case, is actually a lightly generated Proof-of-Work block, and this design is very similar to "version-n" PoS we looked at in part 1. The best way of tracing a line back is by considering Slasher as a delayed signature PoSn, so all blocks are actually PoW blocks which require additional rules on top.

Since this a state-based protocol, blocks require an additional field containing a hash of a random value; once this value has been released on-chain with a transaction, the staker gains access to his Proof-of-Stake incentive, in the form of a monetary reward.

#### *Casper*

> As of now, Casper is used as a so-called "Friendly-Finality-Gadget" (FFG). There is no block creation involved in this process, and a Proof-of-Stake instead is used for defining the concept of **finality**. We will get back at what *finality* is, why it is so important and why traditional blockchains do not define said concept in another episode of this series. Eventually, the Casper protocol , in the "Correct-by-Construction" (or CBC) flavor will be used in a Proof-of-Stake only system, although said dynamics are still rapidly evolving.

An evolution of Slasher, Casper (name inspired from the omonimous movie, which also gives it a nickname, "the friendly ghost") takes its name from attempting to push the *Greediest Heaviest Observed Sub-Tree* or [GHOST](https://eprint.iacr.org/2013/881) (hence the wordplay) rule-set, which was originally intended for Proof-of-Work systems, to Proof-of-Stake systems. It is currently slated for the [ETH2](https://ethereum.org/en/eth2/) upgrade. 

Casper is often compared to Slasher as the two share the same punitive design, although Casper only punishes nodes creating blocks on the wrong chain. States are used with the concept of *dunkle(s)*.
A *dunkle* represents the block-primitive which is used as on-chain proof for demonstrating a staker created a block on a fork.

Good documentation on this protocol can be found on the [ETH developer resources,](https://eth.wiki/en/concepts/proof-of-stake-faqs) or [by reading EIP-1011.](https://eips.ethereum.org/EIPS/eip-1011)

##### *Different Casper flavors*

The two main Casper flavors which are being evaluated for Ethereum development purposes are Casper-FFG and Casper-CBC. Both implementations make a heavy use of the Casper fundamental game-theory, although the FFG variant is severely restricted in its usage as it is only a second-layer applied on top of the existing PoW infrastructure. Currently, the last piece of up-to-date documentation I could find on Casper-CBC comes from [this Github repo](https://github.com/ethereum/cbc-casper), which stopped getting regularly updated in 2018.
I did reach out to Vlad Zamfir, lead developer and author of most of Casper-related work for the Ethereum codebase, and [he did reassure me](https://twitter.com/VladZamfir/status/1345829466090713088?s=20) he is still working on it, albeit privately.

#### *Ouroboros*

> Ouroboros is currently the Proof-of-Stake protocol which presents more variations than any other I have examined. This, and how well-written and self-explanatory the documentation is, sparked great interest in me. We will get back at why I like it so much in another part of the series.

Initially written in 2016, evaluated and improved up to 2019, with different flavors such as *Praos*, *Chronos*, *Crypsinous*,  developed during that timeframe, Ouroboros is a Proof-of-Stake protocol which heavily abstracts what are considered two key characteristics of a sound chain: **stability** (as in a concept similar to *finality*) and **liveness**, defined as a concept which evaluates how the chain reacts to new transactions being written to it. 
By leveraging these two concepts, and a [novel reward system](https://iohk.io/en/research/library/papers/ouroborosa-provably-secure-proof-of-stake-blockchain-protocol/), the protocol makes extensive use of a provably random *coin-flip* algorithm for choosing which staker will be able to propose a block at a certain *epoch*. In this context, an *epoch* is a chain snapshot of current stakers, which  permits a fair distribution of said work between different *economic value*-based stakers.
To make up to the limitations of the protocol (specifically, the need of a constant protocol-level synchronization between nodes), the [*Praos*](https://iohk.io/en/research/library/papers/ouroboros-praosan-adaptively-securesemi-synchronous-proof-of-stake-protocol/) flavor was developed.

The [*Chronos*](https://iohk.io/en/research/library/papers/ouroboros-chronospermissionless-clock-synchronization-via-proof-of-stake/) flavor was formalised to enable time standardization and offset risks of nodes losing protocol synchronization, which is a fundamental part of the Ouroboros set of assumptions.

The [*Crypsinous*](https://iohk.io/en/research/library/papers/ouroboros-crypsinousprivacy-preserving-proof-of-stake/) flavor implemented [*ZK-SNARKS*](https://z.cash/technology/zksnarks/) or *"Zero-Knowledge Succinct Non-Interactive Argument of Knowledge"* proofs. 

Instead, the [*BFT*](https://iohk.io/en/research/library/papers/ouroboros-bfta-simple-byzantine-fault-tolerant-consensus-protocol/) flavor was developed around *Bizantine-fault-tolerant* consensus, and managed to greatly increase the security model in the scenario of a *covert attack*, although it is based off a *permissioned* environment.
*Ouroboros* is currently live on the [Cardano](https://github.com/input-output-hk/cardano-node) project, and the *Praos* flavor is slated for release with the [**Shelley**](https://roadmap.cardano.org/en/shelley/) upgrade.

## Conclusions

This rounds up my list of state-based/epoch-based Proof-of-Stake implementations. I will take care of updating this list when new protocols which fit this subgroup are published. 
With this second part, I have now fully indexed all major PoS protocols which do hold significant part of the current *cryptocurrency economic value*, or, in better words, have the most amount of money involved in stakers.

Now what? In the next article of this series, we'll go through the shortcomings of state-less/epoch-less Proof of Stake protocols, and we'll thoroughly analyze some attacks which are possible.

### Edits

*Edit 7/1/2021: Fixed some naming conventions.*