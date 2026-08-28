# ADR 0008: Buy distribution with integration work instead of ad spend

**Status:** Accepted early, held throughout.
**Decided:** 2023 to 2026.

## Context

We were a protocol with no consumer brand, selling a product that requires trust, to an audience that has been repeatedly harmed by products that asked for trust.

Paid acquisition was the wrong instrument for that. Crypto retail acquisition is expensive, the cohorts it produces are mercenary and leave when incentives end, and none of it addresses the actual objection, which is not awareness. It is credibility. **No amount of ad spend makes a stranger comfortable custodying real world asset exposure with you.**

What does move that objection is a credible party being publicly associated with you.

## Decision

Buy distribution with integration work rather than ad spend.

We were accepted into Chainlink BUILD (sub 3% acceptance), the Avalanche Codebase accelerator (sub 5%), and Brinc, backed by Animoca Brands (sub 5%), and did ecosystem work with OKX, Circle, and Avalanche.

### Why this works structurally

Every major ecosystem runs a team whose job is to find and amplify projects building meaningfully on their stack. That team has budget, an audience, and a quota. **Deep integration is the qualifying criterion**, and the currency they pay in is exactly what a trust constrained product needs: association, audience, and engineering support.

So the engineering that made the product work also bought its distribution. Cross chain transfer via CCIP, Data Feeds and Proof of Reserve for NAV attestation, Trader Joe on Avalanche and Uniswap on Ethereum for peg defense. Each of those was a technical requirement first. Each was also an entry ticket.

The multiplier is that these are **credibility transfers, not impressions.** An allocator who will not take a cold pitch will take a warm introduction from an ecosystem team, and the diligence conversation starts from a completely different place when the answer to "who secures your bridge" is a name they already trust.

## Consequences

### What it cost: roadmap control

Integrating deeply with a partner's stack is also a commitment to it, and that commitment shows up later inside architecture decisions.

[ADR 0001](0001-chainlink-ccip-over-layerzero-oft.md) is the clean example. The BUILD relationship was one of four factors in choosing a bridging stack, and the chain coverage we gave up as a result was a real product cost that the roadmap absorbed. I still think the decision was right on the technical merits alone. But **partnership led distribution means partner incentives sit inside your architecture decisions**, and pretending otherwise is how a team ends up with a stack it cannot explain.

That is a trade, not a free lunch. It should be made knowingly and it should be written down when it is made, which is the point of recording it here.

### What it cost: schedule dependency

Partner timelines are not your timelines. BSC and Base expansion was gated on Chainlink shipping routes and confirming canonical infrastructure addresses for those chains. No amount of internal prioritization moves that date. When your distribution strategy and your technical dependencies are the same relationship, a delay on one side is a delay on both.

### What it bought

Access to allocators and integration partners that would not have taken a cold approach. Engineering support during integration. Third party validation that appears in the public record, which for a product in a trust constrained category is worth more than the equivalent spend on advertising, and unlike advertising it persists after the company stops paying for it.

## What I would do differently

**Write down, for each partnership, which architecture decisions it is permitted to influence.**

We made those calls case by case, as they arose, on the merits as they appeared at the time. That is not unreasonable and each individual call was defensible. But the influence was therefore **invisible until you look back at the pattern**, and by then it is load bearing. A one page note per partnership, stating what we are integrating, what we are getting, and explicitly which technical decisions are in scope for the relationship and which are off limits, would have made a real force in the architecture visible while it was still steerable.

The failure mode this prevents is the one where a team eventually cannot articulate why its stack looks the way it does, because each individual decision was locally reasonable and nobody was tracking the accumulated direction.
