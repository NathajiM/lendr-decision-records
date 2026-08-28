# ADR 0001: Chainlink CCIP over LayerZero OFT for canonical cross chain supply

**Status:** Accepted, reversing an inherited design. Held until wind down.
**Decided:** 2023 to 2025, across the V2 to V2.1 rebuild.

## Context

Lendr needed two tokens to exist on multiple chains: USDL, the USD settlement asset users deposit into vaults, and RWAL, the governance token. Ethereum and Avalanche at launch, with BSC and Base on the roadmap.

The binding constraint was not throughput or cost. It was that **the underlying assets are regulated real world assets**. That imposed three requirements a general purpose bridge does not have to meet:

1. **One canonical supply.** No wrapped derivatives, no chain specific variants of USDL trading at different prices. A settlement asset for a security token cannot have ambiguous supply.
2. **A provable supply invariant.** Total supply across all chains must equal total mints minus total burns, continuously monitorable, so that an inflation bug is detectable rather than inferred from a depeg.
3. **A bridge story that survives institutional diligence.** Counsel and allocators for tokenized RWA ask who secures the bridge. The answer has to be a sentence, not an architecture lecture.

The design I inherited specified LayerZero OFT. We prototyped against it and then abandoned it.

## Options

**A. LayerZero OFT.** Native omnichain fungible token standard. Burn on source, mint on destination, canonical supply preserved. Broad chain coverage, low fees, and a configurable DVN set: the issuer selects which verifier networks must attest to a message.

**B. Chainlink CCIP with the Cross Chain Token standard.** Burn and mint via a token pool per token per chain, with per route rate limits. Secured by CCIP's committing and executing DONs plus the Risk Management Network, an independent network that must separately attest and can halt the lane.

**C. A third party liquidity network bridge.** Rejected before serious evaluation. These produce wrapped assets and fragmented liquidity, which violates requirement 1 outright. Not viable for a settlement asset backing security tokens.

## Decision

CCIP with burn and mint token pools.

Four factors weighed, and I am listing them in the order they actually mattered rather than the order that sounds most rigorous.

**1. Vendor consolidation.** We already depended on Chainlink for two things in the critical path: Data Feeds for pricing and Proof of Reserve for attesting the off chain reserve before a NAV update could settle. The NAV path already failed closed on a stale Chainlink feed. Adding LayerZero would have introduced a **second independent trust assumption and a second failure domain without removing the first**. One vendor in the critical path is a risk. Two vendors in the critical path is two risks, not a hedge.

**2. Chainlink BUILD.** We were accepted into BUILD, which has a sub 3% acceptance rate, and it came with engineering access, credits, and ecosystem distribution. This was a commercial factor influencing an architecture decision. It is worth saying plainly rather than dressing up, and ADR 0008 covers what that pattern cost us generally.

**3. Institutional credibility.** For a product selling tokenized real world assets, "the bridge is secured by Chainlink" ends a diligence conversation. At the time, no other answer did. This is not a technical argument and I do not claim it as one, but it was a real input to a real decision in a market where the buyer's counsel has a veto.

**4. Security model preference, specifically the absence of a knob.** LayerZero's configurable DVN set is genuine flexibility: the issuer chooses the verifiers. For a security token, that flexibility is **liability**. If I select the verifier set, I own the consequences of that selection, and I own explaining it to a regulator. CCIP's Risk Management Network is not mine to configure. Choosing the option with less control was choosing the option with less to defend.

## Consequences

### What it cost

**Narrower chain coverage.** OFT supported substantially more chains than CCIP did at the time. BSC and Base bringup was gated on CCIP routes going live and on Chainlink confirming canonical RMN proxy addresses for those chains. We gave up expansion optionality, and the roadmap absorbed the delay.

**Higher per message cost.** CCIP fees were meaningfully above OFT. For a protocol where users might bridge small amounts frequently, this is a direct user experience cost. We partially reasoned around it by expecting most holders to hold rather than bridge, which was an assumption we never got to validate at scale.

### Residual risk accepted

A Risk Management Network halt stops all bridging with no fallback path. Our own operational runbook lists the recovery as waiting for the halt to lift. That is genuinely bad availability, and we accepted it, because the obvious mitigation is a fallback bridge and a fallback bridge reintroduces exactly the second trust assumption factor 1 was about removing. Availability was traded for a smaller attack surface, deliberately.

### What it required operationally

The supply invariant had to be monitored off chain rather than assumed. Cumulative burns on source chains must always be greater than or equal to cumulative mints on destination chains. If destination mints ever exceed source burns, that is protocol level inflation and it needs to page someone, not appear in a weekly report.

Per route rate limits, expressed as a bucket capacity plus a refill rate, bounded the blast radius of a compromised pool. This is the control that turns "the bridge was drained" into "the bridge leaked at a bounded rate until someone paused it."

## What I would do differently

**Run the chain coverage constraint before the partnership conversation, not after.** We would have reached the same answer. But we would have reached it with the BSC and Base delay already priced into the roadmap, instead of discovering the gate later and absorbing it as a surprise. When a partnership is one of the inputs to an architecture decision, the honest sequence is to establish what the technical constraint costs first, so you can see clearly what the partnership is worth.
