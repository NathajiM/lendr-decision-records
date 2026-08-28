# ADR 0004: Build an ERC-3643 subset, adapt at the identity boundary

**Status:** Accepted. Implemented during the platform build.
**Decided:** 2026-05.

## Context

Every token deployed on the platform had to be a compliant security token: transfers permitted only between verified identities, restricted by jurisdiction, subject to holder caps and issuance ceilings. ERC-3643 is the standard for this.

The reference implementation, T-REX, is MIT licensed, battle tested, widely deployed, and authored by Tokeny. Tokeny is also **the direct competitor whose structural weakness the entire platform strategy was aimed at** ([ADR 0002](0002-whitelabel-platform-before-first-party-assets.md)).

So the question was not only technical.

## Options

**A. Vendor T-REX wholesale.** Fastest path to a working compliant token. Battle tested code with real deployments behind it. Inherits a broad feature set and a design direction set by a competitor.

**B. Fork T-REX and trim.** Keep the proven core, remove what we do not need. Sounds like the moderate option, and in practice means owning a divergent fork that gets harder to reconcile with upstream every month, while still carrying the original design assumptions.

**C. Build a tight subset against the same interfaces.** Implement only the behavior our tier model actually requires. Smallest surface, full ownership, no proven history.

## Decision

Option C for the token itself, **with an adapter that wraps upstream T-REX identity registries at the boundary.**

That split is the actual decision, and it comes from noticing that the technical question and the commercial question have opposite answers.

### Why build the token

**Audit surface was the binding constraint, not development time.** Every vendored line is a line that has to be audited, and re-audited on every upgrade, out of a fixed budget ([ADR 0006](0006-security-sequencing-under-a-fixed-audit-budget.md)). T-REX carries capability we did not need, which is a compliment to it: it serves a broader market than our tier model permitted. But unneeded capability inside a compliance critical contract is not free, it is priced per audit.

**Dependency on a competitor's release cadence was strategically unacceptable on the core issuance path.** Not because Tokeny would act badly. Because their roadmap answers to their enterprise customers, and ours answered to a long tail they had chosen not to serve. Those design pressures diverge over time by construction, and I did not want our critical path governed by a divergence I could not influence.

**What we built instead** was deliberately small. Every transfer, mint, and burn routes through a single balance update override that consults the compliance module and the identity registry. Nothing can move without passing both. Around that: a per tenant minter role, an emergency pause, and an explicit holder tracking hook. Three reference compliance modules covered jurisdiction allowlisting and blocklisting, maximum holder counts, and issuance ceilings wired to the tier registry.

The rule was that a reviewer should be able to hold the whole permission path in their head. If they cannot, the contract is too big to be trusted with compliance.

### Why adapt at the identity boundary

Here the commercial logic inverts the technical logic.

**Issuers migrating off Tokeny are the most qualified leads in the market.** They have already decided to tokenize, already been through KYB, already built identity infrastructure, and have already discovered what enterprise pricing and cycle length feel like. They are pre sold on the category and dissatisfied with the incumbent.

Making them rebuild their identity registry to move is a migration tax that loses the deal. So we wrote an adapter that wraps the upstream T-REX identity registry interface: a migrating tenant keeps their existing on chain identity infrastructure and simply points our token at it. Alongside it, a provider agnostic signed attestation adapter covered the mainstream KYC vendors for new issuers.

**Independence at the token layer, compatibility at the identity layer.** Own what you are differentiated on, interoperate everywhere else.

## Consequences

**We own the correctness of a compliance critical contract permanently.** No upstream maintainer, no shared security. A bug here is a compliance failure, not just a bug, and the blast radius includes tenants.

**No battle testing.** T-REX has years of deployments behind it. Ours had tests and a planned audit. That is a genuine downgrade in assurance and it was the strongest argument for option A.

**Ecosystem tooling that expects full T-REX may not work** against a subset. Third party dashboards, indexers, and analytics that assume the complete interface are a compatibility question we would have kept answering.

**In exchange:** a small enough audit scope to actually afford a good auditor, no competitor in the critical path, and a pluggable compliance module interface, so a tenant needing a rule we never imagined could add one without us touching the token.

## What I would do differently

Nothing about the split. Build the core, adapt at the boundary is the right shape and I would apply it again.

**But I would have committed the audit budget for it earlier.** The whole justification for building rather than vendoring is that a smaller surface buys a better audit. That argument only pays off if the audit is actually funded and scheduled. We carried the ownership cost from the moment we wrote the first line, and we had not yet realized the benefit when the company wound down. Deciding to own compliance critical code and then not immediately booking the audit means you are holding the risk without the compensation.
