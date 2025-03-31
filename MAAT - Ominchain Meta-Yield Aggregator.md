**Omnichain Meta-Yield Aggregation**

Authors: 0xKima, 0xCompose, 0xyz, 0xCrater, 0xCheetos, 0xKubikRubik, 0xMarvin, 0xNazarr

**Abstract**

MAAT is an omnichain meta-yield aggregator designed to unify and simplify yield generation across multiple blockchain ecosystems. By leveraging cross-chain messaging and value transfer protocols, specifically LayerZero and Stargate, MAAT enables users to access optimized APYs through automatic rebalancing, cross-chain ZAPs, permissionless strategy layer and yield searcher model.

This paper outlines MAAT's architecture and vision for a fully interoperable DeFi future.

**Introduction**

Current DeFi infrastructure suffers from fragmented liquidity, complex user experiences, and limited cross-chain capabilities. MAAT solves these issues by providing a single, intuitive interface for accessing and managing multi-chain yield strategies. This document details MAAT's motivation, goals, and the technology stack that supports its mission to abstract complexity and maximize capital efficiency.

**System Architecture**

In the MAAT Protocol, the architecture is divided into three primary layers: Off-Chain, Execution, and Strategy. Each layer plays a crucial role in ensuring the protocol's functionality and efficiency.

The goal of the MAAT Protocol is to modernize yield aggregation infrastructure, leveraging recent breakthroughs in cross-chain technologies.

With that in mind, all of our systems are designed chain agnostic.

The general architecture was inspired by Ethereum's layered structure: Data Availability Layer, Consensus Layer, and Execution Layer. MAAT Protocol is also composed of three layers:

- Off-Chain
- Execution
- Strategy

Layers are switchable and upgradeable. This type of architecture ensures that each layer can be replaced or upgraded without affecting other parts of the system.

### **Off-Chain Layer**

The Off-Chain layer is responsible for decision-making and aggregating data required for that process.

Layer components:

- Yield Searchers - people or organizations creating their own Yield Searching Policies (strategies), configuring levels of risk and return, protocols in use, cross-chain protocol to use, fees, etc. For example, the MAAT Team is a Yield Searcher, building our own implementation.
- Data Hub (Indexer) - a single data hub that aggregates data from all chains in parallel.
- Commander - a bot that executes actions, responsible for moving assets between chains and protocols.
- Math Module - decision-making module.
- Compounder - a bot responsible for executing transactions of claiming, swapping, and compounding incentives appearing on Strategy contracts.

### **Execution Layer**

The Execution layer is where actions passed from the off-chain layer are executed. Here, all cross-chain interactions occur, and all data comes from here.

This layer involves smart contracts that handle deposits, withdrawals, and rebalancing of assets across different strategies and networks. It ensures that user funds are securely managed.

Layer components:

- Vault - a group of ERC4626 standard smart contracts, one on each chain, modified to handle cross-chain operations with synchronized Price Per Share to act as a universal entry/exit point across all chains.
- Bridge Adapter - a smart contract for unifying multiple cross-chain value transfer protocols (Stargate, CCTP, USDT0, etc.).

This layer is named Execution because it handles all actions necessary to realize decisions made at the Off-Chain Layer by the Commander and Math Module. The Commander passes a decision into the Vault's `execute(Command[])` function in terms of batches of commands.

![Execution Layer Diagram](https://mermaid.ink/img/pako:eNqVVG1vmzAQ_iuWpUqpRCIweRmoqpSt_bZqU9NV2iAfHLgQVLCRMUuzvPz22RDyStbMH_xyvnvuufP5ljjgIWAXt9ttn8lYJuCip-HwBbXR4zsEhYw5QwldgPBZqTNN-DyYUSHRy2efITV-5CByT89j1G7fo5WPc0nfYMjCB0ggohJakr8BMxBNecGkgcKtHG59vCr9VVAjnvwG4VXLDo1mWbIYSaEMokWr3qB8u7kKQ4o4ikA8w4QmlAXQOrCq7PJiEgmazQ6g9AhjAUGZha_Pe-nNDXqlRSItdFc6oHkOMr-biPuApxnPIUQBTZJ8hUaSCh2q5dW78RFMw32FWeKTC1anl5VJfSQf-CKNvuw6E3p8y2ScxnmKNptNI0ctH4pJLEVxrEROlL7zZBHxJuiG1KWUhSppNfB_GZ052tG7wmZvtauD2uX-6rge6vqvR1UQXrlUdfcTqGCWVy7jj5SHw9dHy9PzgSqwsJHbeYKu4EYO3ak_EYKq1Fi2bnVO5rGchYLOq49RUiaXmR9B_RtLR0SujevsDa8Iyz5LuX2ZuH2acruR2m6DDZyCSGkcqia51Beql8wgBR-7SIc91XA-9tlaqdJC8tGCBdhVjwMGFryIZtid0iRXpyIL1a94iKmKNd1JIYwlF09VGy67sYEzyn5xntYw6ojdJX7HLiG9jmPbpqkmq0t6XQMvlNQxO45DzIEz6DvWpz7prw38pwQwO_1ujzjEMm2l1esNHANHQkezZaiiBPFFt2XsWtb6L_Pg42o?type=png)

<div style="text-align: center;">
    Picture 1. Execution Layer
</div>

### **Strategy Layer**

The Strategy layer focuses on management of yield-generating strategies.

This layer unifies interfaces of different DeFi protocols, such as Yearn Finance, Harvest Finance, AAVE, Compound, Stargate, etc., enabling users to access a wide range of yield opportunities.

Vault contracts from the Execution Layer distribute funds across Strategies to maximize yield while also trying to diversify risks.

Layer Components:

- Strategy - a smart contract proxying yield-generating protocols for interface unification.
- Incentives Controller - the entry point for all operations with incentives - claiming, swapping, and compounding.

![Strategy Layer Diagram](https://mermaid.ink/img/pako:eNqFVG1v2jAQ_iuWpUqtFCihCYV8mERppyEVqWo7po30g5scwWqwkeNAWdP_vrPNS4rK6g-xfffc25M7v9FEpkAj2mg0YqG5ziEio37_kTRIoRXTkK1JDkvIY2Eh01yukhlTmjxexYLgcujGNzJmZa79id2erKTfH9840MkJSeR8IUuRnp6RAvK8IFwsAUMuofAIEylRYCSFRo1GpZZkaVztHLC8kCTJGZ8XeM4Rv2IqLaxtodkLED2DOTnlUyIggaJgan0Wi8-yDOtZPmzq_MX1bLBJkotsa1mUz5lii9kx3GQrJytUkJrmyTkwK-UKEs2l2NFm1uB-bDMYSCz89TsXTCTgJKbMif2e3ymZS5ERw43xagEV2lYfDZ1fEKk7HNRdxTSFhSy4Pj07N5mmiq3wuP8tMa0cO-1j7Awxivtj_yVnDzvghu8UX1JzC5ZCm8KVVEqutrfquj-sMB572Ur28RzAtBciVmxxL0sNaiO-efxRbf3WuPqsP4LPGOg_K5awlOF2tPwa5qD2WxwixTL4svKfD9eDpuuCKt8Ykdew4bcq8huYEnuovTrkUGCh2CDkGWWGGC1fQFSklpADjoajOjdO6GJ-8H-Um4s6N9bgYmI3rIx6dA5qzniKT8qbsY6pGUqIaURMA07tRNNYvCOUlVo-rEVCI61K8KiSZTaj0RQHHW_lIkX6rjlDhudbCKRcSzVyb5Z9ujy6YOKPlDsIXmn0Rl9p5HfazW7QC4PgstcKW72LjkfXNOr6zUu_FwTtsNfuhGHLf_foX-ug1ex22r1Oq-uHAe7dy9CjmTLFbBJEUkANcFw0jcL3fxbFs3o?type=png)

<div style="text-align: center;">
    Picture 2. Strategy Layer
</div>

**Core Principles**

- **Chain-agnostic design** - our goal is to break boundaries, not create them.
- **Transparency** - everything that can be put on-chain MUST be made on-chain and clear analytics should be always accessible for users
- **Simplicity** - try to make everything as simple as possible to achieve a "poka-yoke".

**Yield Searchers Paradigm**

We envision the MAAT Protocol as a framework for everyone to find their own solution to the yield generation problem. MAAT MUST be permissionless for everyone to participate.

The MAAT Team is the ultimate builder to push forward new paradigms of yield searching, but we understand that this job SHOULD be delegated to others to keep the protocol going.

We've been inspired by the CoW Protocol Solver Model and decided to go with a similar approach. We named it the Yield Searcher Model.

Everyone can participate in the decision-making process of how assets should be distributed, how often in-chain or inter-chain rebalances should occur, how much fee to take, and how to properly build the decision-making module.

With that goal in mind, we are building the MAAT Vault to be universal, easily deployable, and configurable, allowing everyone to take the initiative and create their own Yield Searching Policies. This enables users to incorporate their vision on where to position themselves on the risk/reward scale.

We, as the MAAT Team, are going to explore the world of the most crazy, groundbreaking strategies, involving newly created chains, new bridges, and just out-of-the-oven technologies to push progress forward and to have fun in the process.

![Solver Model Diagram](https://mermaid.ink/img/pako:eNqVVNuOmzAQ_RXLUqREAgQh0ISHSmm2b5sq2qRbbUsfvGECVsGOjEmbzeXbO0AuFG20W548Z2aOzxxs7-hSRkADappmKDTXKQRkOh4viEnkamUuE8YFrnOZbkCZGRanoaiKV6n8jWmlyeJTKAh-X3NQOTHNj2SPZRAzDXsyrzqtkvNHSBsR6TqObXfIePbUwx10AuQOYhBECgjpz7cpJ5KLZ5bDlfaMkK5X8j703sUzHj9-vnKUEen2_f8gmBS5lllDRhWTruvYDZaap9NpOlIRlot2ciIFbrhhmm_gZlEl9Zq8pL8lXEPKcw3R6cfVqfbGOMmKk6gyPdcKR4q3-8ZObVernmVT2WtdKOCL1OYbIi7a91KkW1KFN9jeR3gyvdZYr9t8df1UCq6l4iKui0Magw7IbDbv5kZqxD2DLB7vjfJchrQppdloYWfDztcL9hHTrHVKLlPdYCuNOCvNi-dYsXXS0FB-EVew1FwKcv9wRZ-AKUGOxyO5YnPNVHlSW3BlNkJXZCKztSxE9C86XzP1qwXVlnLILcuqYRARNWgGKmM8wpdkV8Ihxfuc4T0OcJnyONEhDcUBC1mh5XwrljTQqgCDKlnECQ1WLM0xKtboGdxxhoNn5xKISqem9UNVvVcGXTPxXcpLCYY02NE_NHAcx_Lcvmu7fcce-APfNeiWBn3PsQbDwWDkDFzb9_2hdzDoS8VgW0PP8x3P_uB6fW_ke9gRq3KWk0KcENQE_dE0GPmHv3VMnFg)

<div style="text-align: center;">
    Picture 3. Solver Model
</div>

**Conclusion**

MAAT delivers a seamless yield experience by abstracting chain complexity and automating capital allocation. Its omnichain-first design, strong security guarantees, and permissionless extensibility make it a powerful tool for the next generation of DeFi users.

**References**
[1] Ethereum. https://ethereum.org
[2] LayerZero. https://layerzero.network
[3] Stargate. https://stargate.finance
[4] Omnichain Vaults. coming soon...
[5] Yearn Finance. https://yearn.fi
[6] Harvest Finance. https://harvest.finance
[7] AAVE. https://aave.com
[8] Compound. https://compound.finance/
[9] CCTP. https://www.circle.com/cross-chain-transfer-protocol
[10] USDT0. https://usdt0.to/
[11] CoW Protocol. https://cow.fi/
