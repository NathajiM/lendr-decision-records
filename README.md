# Lendr.fi Decision Records

Lendr.fi was a DeFi protocol for tokenized real world assets: ERC-4626 vaults holding RWA exposure, a USD settlement stablecoin, canonical cross chain transfer via Chainlink CCIP, and a self serve platform that let third party issuers deploy compliant security tokens without rebuilding the stack.

**The protocol is no longer operating.** This repository contains no protocol source, no deployable contracts, no ABIs, and no addresses. It is a record of the decisions behind the system and the reasoning that was available when each one was made.

I was the founding head of product from March 2023 to May 2026, owning roadmap, architecture direction, security posture, regulatory strategy, and partnerships across a 20 person cross functional team.

## Why decision records

Shipped code shows what a team built. It rarely shows what was rejected, which constraint forced the shape, or what the choice cost later. Those are the parts that disappear first when a company winds down.

Each record states the situation, the options genuinely on the table, the call, and the consequences including the ones that went badly. Where I would decide differently now, it says so.

## Contents

| Record | Decision |
|---|---|
| [ADR 0001](docs/adr/0001-chainlink-ccip-over-layerzero-oft.md) | Chainlink CCIP over LayerZero OFT for canonical cross chain supply |
| [ADR 0002](docs/adr/0002-whitelabel-platform-before-first-party-assets.md) | Ship the issuer platform before our own asset launches |
| [ADR 0003](docs/adr/0003-tiered-access-model.md) | Three tier access model, enforced on chain rather than in the UI |
| [ADR 0004](docs/adr/0004-build-erc3643-subset-vs-vendor-t-rex.md) | Build a compliance token subset, adapt at the identity boundary |
| [ADR 0005](docs/adr/0005-tenant-composition-clone-not-inherit.md) | Compose first party assets as tenants rather than inherit compliance |
| [ADR 0006](docs/adr/0006-security-sequencing-under-a-fixed-audit-budget.md) | Sequence security work to front load the cheap fix window |
| [ADR 0007](docs/adr/0007-regulatory-strategy-as-a-product-surface.md) | Treat compliance as a product surface, not a legal review gate |
| [ADR 0008](docs/adr/0008-partnership-led-distribution.md) | Buy distribution with integration work instead of ad spend |

[Architecture overview](docs/architecture.md) maps the system the records refer to.
[External artifacts](evidence/external-artifacts.md) lists what still exists publicly, with dates.

## Deliberately absent

Contract source, ABIs, deployed addresses, and deployment parameters. Internal security findings. Investor materials. Third party audit reports, which are linked rather than republished. Any figure covered by confidentiality.

Everything here is either my own work product or already public.

## License

Documentation is released under [CC BY 4.0](LICENSE).
