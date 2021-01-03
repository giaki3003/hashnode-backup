## Proof-of-Stake, by giaki3003 (Part 1)

# Where our journey starts...

Proof-of-Stake, or PoS, is a term used to describe a consensus mechanism used by a variety of alternative cryptocurrencies, or *altcoins*.
The idea revolves around shifting **work** from *energy-wise intensive computations to economic value itself*, represented on-chain and wrapped to the coin of each network.

## The reasoning behind this approach

In this overview of State-less/Epoch-less Proof-of-Stake cryptocurrency implementations, my objective ideally is to divide PoS implementations in two big subgroups, this being the first one. 
In fact, while all Proof-of-Stake implementations try and achieve the shift of **work** to on-chain economic value,  a radical difference comes to how their own block primitive is structured; a group of PoS implementations always requires a certain amount of *"Proof-of-Stake-only bytes"* in the block primitive header, which are not **work** related.
Tracing a line here helps us identify such a broad set of different implementations.

## A look at the main implementations

Notice: most modern proof-of-stake implementations are *account-based*, instead of *unspent-transaction-output* (UTXO) based. This is due to how the first altcoins inherited from Bitcoin most of their code, while newer altcoins tend to have their own, independent codebase. We will get back at this peculiar difference in another part of the series.

### State-less/epoch-less UTXO-based PoS

There are currently various cryptocurrency networks which run state-less/epoch-less UTXO-based PoS implementations. Although naming conventions are quite confusing, I'll try my best at indexing them based off original versioning. This approach ties in well with the language and specifications most developers I have talked with use, and finding a common ground to which expand upon is mandatory for any type of analysis and discussion. All of these implementations do not require any notion of blockchain **states or epochs.**
States/epochs are a term used for describing chain snapshots used for consensus-critical validation of blocks. They're currently used in most account-based Proof-of-Stake implementations for a variety of security assumptions and as a way of mitigating a wide range of attacks. 

#### *Version nil-n*

> The older your economic commitment, the greater your chance at producing a valid block.

The first implementation we have information of is what I will refer to as *Proof-of-Stake version nil-n* (PoSv0.n), originating from [Peercoin.](https://github.com/peercoin/peercoin)
Economic value's work is greatly influenced by **how long it has been on-chain**.
The latest version, PoSv0.5, improves over the original concept, and can be viewed [here](https://github.com/peercoin/peercoin/blob/master/src/kernel.cpp#L346).
Documentation about this protocol can be found reading the [Peercoin PoS documentation.](https://docs.peercoin.net/#/proof-of-stake)

#### *Version nil-n with "inverted time" flavor*

> The older your economic commitment, the lower your chance at producing a valid block.

Although this protocol is often referred to as *Proof-of-Stake-Time* or PoST, it is really just a flavor of "nil-n" PoS, where economic value's work is shifted from gradually increasing based off how long your value has been on the network to the **exact opposite**. 
The latest, and only PoST implementation can be found in [Verium's codebase](https://github.com/vericoin/vericoin/blob/master/src/kernel.cpp#L260), and I couldn't find any proper documentation about it, unfortunately.

#### *Version n*

> The greater your economic commitment, the greater your chance at producing a valid block.

A fork of "nil-n" PoS, *Proof-of-Stake version-n* was first implemented in Blackcoin, as described in the [Blackcoin protocol whitepaper.](https://blackcoin.org/blackcoin-pos-protocol-v2-whitepaper.pdf)
The latest version, PoSv3, can be seen live on [QTUM](https://github.com/qtumproject/qtum/blob/master/src/pos.cpp#L44). 
The main difference here is how economic value's work **isn't tied to any on-chain time bonus**.
The best documentation available for this protocol can be found [in this devblog by QTUM founder Earlz](http://earlz.net/view/2017/07/27/1904/the-missing-explanation-of-proof-of-stake-version).

### State-less/epoch-less Account-based PoS

While account-based blockchains are a more popular choice in recent projects, some older chains retain the logic of UTXO-based PoS. When presenting them, I'll use the same versioning in hopes of improving readability and lessening confusion.

#### *Version n with "deterministic election algorithm" flavor*

> The greater your eligibility score, the greater your chance at producing a valid block.

A flavor of n-PoS, this implementation shifts the economic value's work from a randomly, hash-chosen winner to a **deterministically elected winner**, following an algorithm which permits nodes to compute an *eligibility score*. This score follows version-n rules, so the greater your economic commitment, the greater your eligibility score.
There seems to have been a single iteration of said flavor, and it comes from [NXT](https://bitbucket.org/JeanLucPicard/nxt/src/master/).
I couldn't find any proper documentation for this, but instead found a [great paper](https://www.researchgate.net/publication/319647471_Securing_Proof-of-Stake_Blockchain_Protocols) by Wenting Li, Ghassan Karame and Sebastien Andreina which also describes this protocol very effectively (kudos for their very well thought and written publication, I recommend reading it).

## Conclusions

This rounds up my list of State-less/Epoch-less Proof-of-Stake implementations.
I will take care of updating this list when new protocols which fit this subgroup are published.
What are the main differences which could emerge when we add states to mix?
In my next part, we'll have a look at the other big subgroup, State/Epoch based Proof-of-Stake, and hopefully address that question thoroughly.




