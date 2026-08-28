# ADR 0002: Ship the issuer platform before our own asset launches

**Status:** Accepted 2026-05-25. Reversed the phase ordering the company had operated under since inception.
**Decided:** 2026-05-25.

## Context

The original plan was straightforward: launch our own tokenized assets first, prove the model, then offer the stack to third party issuers as a post MVP add on. Four vaults were built to that plan: an S&P 500 tracker, gold, US real estate, and an AI infrastructure basket.

By May 2026 three things about that plan had become hard to ignore.

**Revenue was concentrated in a handful of assets we picked.** If the S&P vault did not attract TVL, that was not a slow quarter, it was a thesis failure. We had built a business whose revenue depended on being right about which four assets crypto natives wanted, which is a fund manager's skill, not a platform's.

**Each asset launch carried a fixed, serial cost.** Custodian relationship, legal wrapper, market making, listing, distribution. Per asset. These do not parallelize well because the bottleneck is legal and counterparty work, not engineering. Four assets meant four of everything before the fifth could start.

**Our actual differentiation was the stack, not the asset picks.** We had built peg management, NAV accounting with high water mark fee accrual and drawdown guards, Proof of Reserve integration, a withdraw queue, fee routing, cross chain transfer, and timelocked governance. Anyone launching a tokenized asset needs all of that. Almost nobody wants to build it.

Meanwhile the incumbent shape of the market was visible. Tokeny, who authored ERC-3643, runs enterprise sales: six to twelve month cycles, customer acquisition cost in the tens to hundreds of thousands, minimum AUM thresholds in the millions, and on the order of a hundred issuers total. That is not a failure of execution. It is what a high touch sales motion produces, and it **structurally cannot serve the long tail**. An issuer with a two million dollar real estate SPV is not worth a six month enterprise cycle to them, and there are far more of those issuers than there are of the ones who are.

## Options

**A. Continue as planned.** Launch our four assets, add whitelabel later. Preserves the community narrative and the existing roadmap. Keeps revenue tied to our own TVL.

**B. Flip the order.** Build the self serve issuer platform first. Our own assets become the first tenants deployed on it.

**C. Do both in parallel.** Considered briefly and rejected. We did not have the team to run two go to market motions, and the platform refactor touched the same contracts the asset launches depended on. Parallel here means neither ships.

## Decision

Option B. The platform became Phase 1. Own asset launches became Phase 2, and were redefined as **the first set of Lendr deployed tenants on the platform** rather than a separate product line.

The bet: product led, self serve tokenization. Thirty to sixty minutes to deploy instead of three to six months. The analogy we used internally was Stripe against traditional merchant acquiring. Stripe did not win by being a better enterprise payments vendor. It won by making the thing self serve and letting the long tail come to it.

The reason this was affordable is the part worth noting. **The pivot did not throw work away, it changed what the existing work was for.** The peg managers, the staking vault, the fee router, the reserve fund, the withdraw queue, the CCIP bridging stack, and the timelock and governance migration were all already built, and all of them became per tenant infrastructure. The refactor needed was collapsing four hardcoded vaults into one parametric vault plus a factory, and building the compliance layer on top. Single use infrastructure became multi tenant leverage.

## Consequences

**Revenue model changes shape entirely.** From performance fees on our own TVL, which is volatile and concentrated, to a recurring take rate across many tenants, which compounds and is diversified. Every tenant onboarded is a permanent fee stream rather than a one time launch.

**The sales motion changes from consumer to B2B.** Different buyer, different cycle, different marketing, and a team hired for the first motion. This is the underrated cost of a pivot like this: the org was shaped for the old plan.

**Own asset launches were deferred indefinitely.** They were the thing the community and token holders had been sold. A pivot that is correct on the merits is still a narrative problem, and I do not think we had solved the narrative problem when the company wound down. Announcing a platform to an audience that showed up for an index fund is a real cost, and it lands on the people who trusted you earliest.

**A contract refactor became the critical path.** Four concrete vaults collapse to one parametric vault. That change sat underneath everything else, which made it the highest leverage and highest risk work in the plan. Mistakes there would have been expensive to undo, which is why it was sequenced first and gated hard.

**Exit gates were defined up front**, which I would do again. The testnet gate was ten external testers deploying a tenant from clean documentation in under sixty minutes each, which tests the product claim rather than the code. The mainnet gate was five paying tenants and five million dollars aggregate TVL, which tests whether anyone will pay.

## What I would do differently

**Run the issuer discovery before building four bespoke vaults.** The insight that made this pivot correct, that the long tail is structurally unserved by enterprise tokenization sales, was available from the outside at any point. It required looking at Tokeny's customer count, cycle length, and minimums and asking who is left over. That is a week of work. We did it after building the asset specific product instead of before, and the cost of that ordering was roughly a year.

The pivot itself was right. The timing of the insight that produced it was the failure.
