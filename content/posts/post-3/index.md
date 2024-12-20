+++
title = 'Proof Of Democracy (PoD): the Consensus Algorithm used in Alastria RedT'
date = 2024-10-26
draft = false
+++

`Proof of Democracy` (PoD) is the consensus algorithm used in Alastria RedT and RedB and in the future ISBE (Spanish Blockchain Services Infrastructure).

In a classical distributed systems implementation, the technical properties of the consensus algorithm determine the properties of the system as a whole.

This is not true in a decentralised system, where the desired properties of the system (trust, decentralisation, inclusivity, fairness, accountability, compliance) are determined by both the technical properties and governance model of the consensus algorithm.

We describe here the Proof of Democracy consensus algorithm used since several years in the Alastria RedT blockchain network, and that will be used in the future Spanish blockchain network.

## The properties of the Proof of Democracy consensus algorithm

I focus in this document of the consensus algorithm, so I will normally talk about the nodes executing the consensus algorithm, which I call `Consensus Nodes` to avoid the confusing terminology used normally in the blockchain space, where the names are usually chosen to catch the attention, not to describe in a precise way the thing they are naming.

### Participation 

Anybody can participate in the operation of the network, subject to a set of rules which are transparent, fair and inclusive.

There is not any formal contract that entities have to sign with some organisation in order to operate a Consensus node and participate in the execution of the consensus algorithm.

But they have to accept to abide to the public Operational Rules for Consensus Nodes, which means that they self-declare things like their compromise to comply with the EU regulatory framework, to make their best efforts in maintaining their nodes 24x7x365, and to collaborate with the operators of the other Consensus Nodes to keep the network functioning.

It is important that identities of the Consensus Nodes are public, including the real-world identities of the organisations operating them. This helps with a lot of things, like transparency, accountability, incentives, etc. More on this later.

### Rule of law

The Consensus Nodes have to comply with the EU legal framework. We are aiming to build a blockchain infrastructure as a Coomon Good, so there is no way around full compliance.

In particular, it is forbidden by the Operational Rules mentioned above to store in the blockchain any type of personal data. This means that:

- It is forbidden to store any form of digital identity of a natural person (whether a DID or any other type of identification)
- It is forbidden to store encrypted personal data (it is still considered personal data).
- It is forbidden to store a hash of personal data if it could be used, combined with other information on-chain or off-chain, to identify a natural person. Only after a formal PIA (Privacy Impact Assessment) could an organisation store a hash derived from personal information.
- It is forbidden to use Tokens or NFTs that have personal information inside it. Most of the use cases that I have seen in public blockchain networks are simply violating the GDPR.

Those prohibitions may seem too restrictive, but they are not. Actually, I have never seen any application useful to improve society and the lives of citizens that requires to store personal data in a public registry (blockchain or not). The only reasons for doing that are the laziness or ignorance of the developers, or just the lack of interest in protecting the citizens because the economic factors are considered more important.

Those behaviours are against democratic values and are not accepted in our network, and the offenders are subject to their expulsion (via voting, as explained later).

### Transparency

Transparency means that decisions taken and their enforcement are done in a manner that follows rules and regulations. It also means that information is freely available and directly accessible to those who will be affected by such decisions and their enforcement. It also means that enough information is provided and that it is provided in easily understandable forms and media.

## The technical properties of the consensus algorithm

The blockchain technology is the Ethereum-compatible <a href="https://consensys.net/quorum/developers/">Hyperledger Besu and GoQuorum</a> blockchain clients which implement a technical consensus algorithm based on a variant of [[[PBFT]]].

If you are an expert on distributed systems you can skip this section, but otherwise I recommend you to continue with me. There is a very high probability that you do not really know how the blockchain works, because most of the descriptions out there are from ignorant people who try to be cool by "explaining" how blockchain technology works.

To be clear, I will use Ethereum as the blockchain technology for the explanations. There are currently several different technologies and they may or may not work like Ethereum. Some of them are very similar in nature, but there is not guarantee that my explanations apply to all of them.

In addition, most of the description below applies to PoW, PoS and PoD consensus algorithms. I will explicitly mention it when I talk about one specific consensus technology.

### The two types of nodes

In a network there are two types of nodes:

1. The nodes that just participate in the network to support applications that read and write to the blockchain. We will call them `Regular` nodes.

2. The nodes that execute the consensus algorithm, which I call `Consensus` nodes. I use this term because there are different names in use in the blockchain space, and like many things in the blockchain space the names are intended to catch the attention of the reader, more than try to fit the actual function of the nodes. Even Consensys (the developers of the blockchain technology used here) call them Validators, when they do not really validate anything, at least from the point of the Regular nodes.

Most of the nodes are `Regular` and they can just come and go, be turned on or off by their owners. They can not affect the operation of the network if they are offline. The only thing that may be impacted is the application of the organisation that uses that node to connect to the network. In other words, reliability of a Regular node only affects the owner of that node and to the applications (off-chain) that depend on that node.

The `Consensus` nodes on the other hand are critical for the functioning of the network