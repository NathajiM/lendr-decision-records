# External Artifacts

Third party and public material that still exists. Listed because independently verifiable evidence is worth more than anything I write about my own work.

Links only. Nothing here is republished, and third party audit reports remain the property of the firms that produced them.

## Ecosystem recognition

| Artifact | What it evidences | Date |
|---|---|---|
| [Chainlink BUILD announcement](https://lendr.medium.com/lendr-network-a-decentralized-rwa-tokenization-protocol-joins-chainlink-build-780d9ac1aace) | Acceptance into Chainlink BUILD, sub 3% acceptance rate. Context for [ADR 0001](../docs/adr/0001-chainlink-ccip-over-layerzero-oft.md) and [ADR 0008](../docs/adr/0008-partnership-led-distribution.md) | 2023 |

Also: Avalanche Codebase accelerator and Brinc, backed by Animoca Brands, both sub 5% acceptance. First place finishes in the Binance BNB Chain hackathon and a Chainlink hackathon.

## Product

| Artifact | What it evidences | Date |
|---|---|---|
| [Product demo](https://www.youtube.com/watch?v=K0GlQrM86IU) | The working product, presented end to end | 2025 |
| [FAQ playlist](https://www.youtube.com/playlist?list=PLzXdgt-VKgeR5Peozjz8efe7_im8ar_LA) | Direct user question handling, the discovery loop behind roadmap prioritization | 2025 |
| [Public documentation](https://whitepaper.lendr.fi/) | The system as documented for users, including token model and security posture | 2023 to 2025 |

Note on the documentation: it reflects an earlier design generation and diverged from the implementation over time. The bridging stack is the clearest case, since the published docs describe the LayerZero design that [ADR 0001](../docs/adr/0001-chainlink-ccip-over-layerzero-oft.md) replaced. Read it as a historical artifact rather than a description of the final system.

## Press and third party coverage

| Artifact | Publisher | Date |
|---|---|---|
| [Testnet launch](https://www.globenewswire.com/news-release/2025/09/22/3154117/0/en/Lendr-fi-Launches-Testnet-Providing-Simple-Compliant-Gateway-to-Tokenized-Real-World-Assets.html) | GlobeNewswire | 2025-09-22 |
| [Liquid staked RWA token launch](https://www.benzinga.com/markets/cryptocurrency/25/05/45469069/lendr-fi-launches-liquid-staked-real-world-asset-tokens) | Benzinga | 2025-05 |
| [Token generation event](https://natlawreview.com/press-releases/lendrfi-announces-tge-rwal-governance-utility-token-powering-rwa-defi) | National Law Review | 2025-09 |
| [Protocol overview](https://web3.bitget.com/en/academy/what-is-lendr-fi-rwal-a-defi-protocol-bridging-rwas-through-tokenization-and-governance-utility) | Bitget Academy | 2025 |

Press releases are company issued and should be read as such. The Bitget Academy piece is independently written.

## Aggregator listings

[ICO Drops](https://icodrops.com/lendr-fi/) and [Forbes digital asset data](https://www.forbes.com/digital-assets/assets/lendr-rwal-2/) carry independent listing records.

## Security reviews

Third party smart contract audits were carried out by SolidProof and CertiK, with reports referenced from the [public security documentation](https://whitepaper.lendr.fi/general-documentation/security-and-audits). Those reports belong to the firms that produced them and are not reproduced here.

Two clarifications, because that page is easy to misread.

**It covers more than one generation of the system.** The audits listed against the original codebase are inherited from an upstream protocol that an earlier version of Lendr was derived from. They are that protocol's audits, not ours, and they say nothing about the architecture described in these records.

**The system described in this repository was never externally audited.** The external audit was scoped and sequenced but not commissioned before wind down. [ADR 0006](../docs/adr/0006-security-sequencing-under-a-fixed-audit-budget.md) covers the internal security process that was completed, and is explicit that internal review is not a substitute for an external one.

## Not published

Investor materials, internal security findings, deployment addresses, contract source, and anything covered by confidentiality or not solely mine to publish.
