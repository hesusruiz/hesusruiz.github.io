---
title: "Consensus algorithms and blockchain governance"
date: 2022-05-08T17:58:38+02:00
draft: false
cover:
    image: ""
    # can also paste direct link from external site
    # ex. https://i.ibb.co/K0HVPBd/paper-mod-profilemode.png
    alt: "<alt text>"
    caption: "<text>"
    relative: true # To use relative path for cover image, used in hugo Page-bundles

---

This article analyses the influence of consensus algorithms in the characteristics of governance models of Public-Permissioned blockchain networks, especifically when using Quorum, Besu and Fabric.

For many years before the blockchain, consensus algorithms have been used in distributed systems to achieve the desired technical properties of safety and liveness, while improving performance and reducing cost. However, with the advent of the blockchain technology, consensus algorithms are additionally important components of the governance model of the network infrastructure.

# Summary

In order to maximise decentralisation, the Decentralised Governance Model of Public-Permissioned blockchain networks dictates that the nodes of the blockchain network should be geo-distributed, operated by different and independent entities and avoiding that any individual entity has a disproportionate amount of control over any part of the system, whether at the technical or the governance level.

Consensys Quorum, Hyperledger Besu and Hyperledger Fabric are technologies used in some blockhain networks aiming at the Public-Permissioned model, like in EBSI or Alastria. They use different consensus algorithms:
- Quorum and Besu use QBFT, a Byzantine Fault Tolerant (BFT) protocol.
- Fabric uses RAFT, a Crash Fault Tolerant (CFT) protocol.

With respect to resilience, the types of faults that each type of algorithm can handle are different. **While CFT protects only from crash faults (fail-stop), BFT assumes an extremely powerful adversary that can fully control at the same time 1/3 of the machines and also the message delivery schedule across the entire network**.

CFT protocols do not offer protection against malicious actors or arbitrary failures of nodes. It protects only against fail-stop errors, essentially when a process stops executing and does not send any further message to the rest of the network. With CFT, if a malicious actor controls a single node the whole network is compromised.

BFT protocols are robust against malicious actors or arbitrary failures of nodes. If one node is compromised the safety of the network is not affected at all. The bad actor should take control of one third of the nodes and the whole network communications simultaneously in order to affect the safety of the network.

The additional protection of BFT implies that for fail-stop errors (non-malicious) CFT is cheaper and faster than BFT for the same level of resiliency (obviously this is because of the null protection of CFT against malicious actors).

The choice of consensus algorithm executed by the critical nodes in the network has not only technical implications, but more importantly, governance implications:
- With BFT the set of critical nodes can be operated by different Member States, maximising at the same time the decentralisation and the resilience of the network.
- With CFT the standard approach is that a single centralised entity operates all critical nodes, like in private consortium networks. This reduces the attack surface, but it still provides a lower protection against malicious actors and it is much more centralised than with BFT protocols.

Recommendation: if a Decentralised Governance Model is important, then it is recommended that only blockchain technology using BFT protocols is deployed at full scale. That means that full-scale deployment of Fabric in Public-Permissioned blockchain networks can not be performed until Fabric implements a BFT consensus algorithm, which is in its roadmap (though there is no planned date yet).

# BFT and CFT consensus algorithms

Replication is a technique that has been used for many years to achieve fault tolerance and reliability.

In 1952 John von Neumann [1] and in 1956 Ed Moore and Claude Shannon [2] showed how reliable subsystems in general (von Neumann) and reliable relay circuits in particular (Moore-Shannon) can be built out of unreliable components. Also relevant is the 1960 paper of Paul Baran [3] on implementing reliable communications despite unreliable network nodes, which was influential in the early days of the ARPAnet.

In the beginning, work on reliability via replication was mainly focused on hardware techniques. It was not until the 70’s when software started to be used to achieve the desired reliability properties, especially in mission-critical systems.

At the beginning of the 70’s it became clear that computers were going to be flying commercial aircraft and NASA began funding research to figure out how to make them reliable enough for the task. An example was the SIFT (Software Implemented Fault Tolerance) project [4] which designed an ultra reliable computer for critical aircraft control applications that achieved fault tolerance by the replication of tasks among processing units.

One of the most notable results of the project was the formal description of two types of errors, described first in 1978 [5] and finally in 1982 [6] in the famous paper The Byzantine Generals Problem:
-	CFT (Crash Fault Tolerant): where the process stops functioning.
-	BFT (Byzantine Fault Tolerant): where the process continues to operate but performs one or more operations incorrectly in an arbitrary way, including malicious behaviour.

In the 90’s the paper The Part-Time Parliament [7] described Paxos, a consensus algorithm solving the CFT problem and later in that decade the paper Practical Byzantine Fault Tolerance [8] described a consensus algorithm solving the more general problem of reliability under byzantine failures.

Even though BFT systems are much more resilient than CFT ones, in 40 years of distributed systems most commercial implementations have used only CFT consensus algorithms, like Paxos or Raft [9]. There are several reasons for this, most notably the additional cost of BFT in terms of resources, protocol complexity and performance.

For example, in a partially synchronous setting, BFT requires 3f + 1 replicas to tolerate f faults while CFT needs only 2f + 1 replicas for the same number of faults. In addition, the number of messages required in BFT grows quadratically with the number of nodes, while in CFT the growth is linear.

Of course, the types of faults that each type of algorithm can handle are different. While CFT protects only from crash faults (fail-stop), BFT assumes an extremely powerful adversary that can fully control at the same time 1/3 of the machines and also the message delivery schedule across the entire network.

Most distributed systems have been deployed and operated inside a single administrative domain (e.g., Amazon, Google, Facebook, etc.) and for the engineers operating those centrally managed distributed systems, a CFT consensus algorithm is “good enough”. The adversary model assumed by BFT is too strong for what they normally observe in practice.
In this situation, the engineers prefer to focus their efforts in increasing the redundancy of the network among replicas or trying to make the processes fail silently instead of in a byzantine way (at least while they are not compromised by an attacker).

Those engineers use additional mechanisms trying to protect against malicious actors and so avoid implementing a BFT consensus, like protecting the machines inside firewalls and implementing mechanisms for monitoring and intrusion detection.
However, none of these efforts has been able to avoid completely byzantine failures in those infrastructures, as described in these reports (TODO). But until recently this did not change the overall panorama regarding the almost non-existent implementation of industrial BFT systems.

# Bitcoin and new BFT consensus algorithms

Until Bitcoin appeared, the main research and implementation efforts on consensus algorithms for distributed systems assumed that there was a single administrative entity operating the whole distributed system and that the objectives were to achieve the maximum reliability with the highest performance and with the minimum cost.

Bitcoin used a completely different approach. The designer(s) used a different set of optimization objectives: while trying to maximise safety, they designed a novel BFT consensus algorithm to enable many different anonymous entities to coordinate in the operation of the system, and surprisingly to all existing practitioners in traditional distributed systems performance was less important than decentralisation (a precise definition of the term decentralisation does not exist, but here it means that the system is operated collaboratively by more than one administrative entity).
Bitcoin was the first practical distributed system where a BFT consensus algorithm was used not only to achieve safety but where it was also embedded in the governance of the infrastructure. Consequently, the consensus algorithm is radically different from any other one that was developed before.

In this setting, it was impossible to use a CFT consensus algorithm because in a permissionless network like Bitcoin there may be many attackers with full access to the system. None of the techniques that engineers were using before with “in-house” CFT distributed systems could be used to protect the Bitcoin system.

Suddenly, the powerful adversary model assumed by BFT did not seem so unrealistic in this new setting.
The additional problem that the designers of Bitcoin had to overcome is that almost all research on BFT before considered a permissioned setting, that is, all replicas are known among them.

This is another of the breakthroughs of Bitcoin: the new BFT consensus algorithm works with anonymous participants where they can come and go without anybody having to permission any of the nodes in the consensus set or even in the whole network.

However, this anonymous nature of the participants, including the ones executing the consensus algorithm is at the core of the performance problems of those permissionless blockchain networks.

In contrast, Public-Permissioned networks useful for the real economy, require from participants strong digital identities linked to their real-world identities. These digital identities enable the usage of much more efficient BFT consensus algorithms where practitioners can apply more than 30 years of research in resilient distributed systems.

# Hyperledger Fabric consensus algorithm

As stated in the official documentation for Hyperledger Fabric [13]:

> One of the most important of the platform’s differentiators is its support for pluggable consensus protocols that enable the platform to be more effectively customised to fit particular use cases and trust models. For instance, when deployed within a single enterprise, or operated by a trusted authority, fully byzantine fault tolerant consensus might be considered unnecessary and an excessive drag on performance and throughput. In situations such as that, a crash fault-tolerant (CFT) consensus protocol might be more than adequate whereas, in a multi-party, decentralised use case, a more traditional byzantine fault tolerant (BFT) consensus protocol might be required.

Hyperledger Fabric supports pluggable consensus protocols. However, the current version of Fabric (2.4) only supports CFT protocols with Raft as the recommended production choice. The inclusion of a BFT protocol is in the strategic roadmap, though there is no planned date yet.

Raft is a CFT (Crash-Fault-Tolerant) consensus algorithm that provides strict transaction finality, but it is not Byzantine fault tolerant. The protocol tolerates technical faults, where the participants are assumed to be always honest, and the machines can not behave in arbitrary ways.

In other words, if an attacker compromises just one of the consensus nodes (called orderers in Hyperledger Fabric) then the whole network may be compromised. This contrasts with a BFT (Byzantine Fault Tolerant) consensus algorithm where the network is resilient to more than one byzantine failure (at most one third of the number of consensus nodes).

Another way to look at this is that in a network where each consensus node is operated by a different entity, with a CFT algorithm just one operator can compromise the safety of the whole network. With a BFT algorithm there must be at least one third of the entities agreeing to attack the network. With BFT, increasing the number of entities running the consensus nodes increases arbitrarily the resilience of the network against malicious actors.

The Raft protocol is suitable for environments which:
- Are properly isolated from external attackers (e.g., an intranet isolated via firewalls from the external world),
- All the consensus nodes are operated by a single administrative domain (that is, a single entity).
- There is an extremely high level of trust among the participants in the network, especially with respect to the entity operating the consensus nodes.

In other words, even though the current version of Hyperledger Fabric is suitable for implementing decentralised use cases (decentralised at the application level), the governance of the underlying infrastructure tends to be centralised, requiring a high level of trust with respect to the entity operating the consensus nodes.

This may change in a future version of Hyperledger Fabric, but for the moment it is a serious limitation for the implementation of Public-Permissioned networks, where one of the critical properties is the decentralised nature of the governance model for the infrastructure.

# Consensus algorithms in Public-Permissioned blockchain networks

With regards to resiliency, a Public-Permissioned blockchain network shares many of the properties of permissionless networks like Bitcoin or Ethereum. Even though it is permissioned, the decentralised governance model implies that nodes may deviate arbitrarily from the protocol for any reason.

They may be broken (e.g., misconfigured, compromised, malfunctioning, or badly programmed) or may just be optimising for an unknown utility function that differs from the utility function used by rational nodes — for instance, ascribing value to harm inflicted on the system or its users.

That means that in Public-Permissioned blockchain networks we must make the same assumptions about adversarial behaviours as in permissionless networks, to achieve the required safety and liveness properties.

However, not all BFT consensus algorithms are equally suitable, as there are several other characteristics, apart from reliability, that are important. In the research literature there are several surveys and detailed analyses of the different types of consensus algorithms for blockchain networks and their properties. See for example [3], [1] or even for specialized fields like IoT [2].

In this document we will focus only on the properties of blockchain consensus algorithms which are most suitable for the type of use cases that will be typically implemented in Public-Permissioned networks.
