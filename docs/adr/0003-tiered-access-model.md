# ADR 0003: Three tier access model, enforced on chain rather than in the UI

**Status:** Accepted 2026-05-25 as part of the platform plan.
**Decided:** 2026-05-25.

## Context

[ADR 0002](0002-whitelabel-platform-before-first-party-assets.md) committed us to self serve tokenization. That commitment immediately produces the hardest question in the business:

**If anyone can deploy a compliant security token in thirty minutes with no human review, what stops someone from tokenizing something they should not?**

The two obvious answers are both wrong.

*Pure DIY, no gating.* Maximum reach, maximum product velocity, and a regulator magnet. The first time someone uses your platform to tokenize equity in a company they do not own and sells it to retail, you are not a platform, you are a participant. This ends the company.

*Full gatekeeping, review every issuer.* Safe, and it makes you Tokeny. Human review is the exact cost structure that produces six month cycles and a hundred customers, which is the market weakness we pivoted to attack. Adding a human to the loop deletes the reason the product exists.

The constraint, then: **bound the risk without adding a human to the loop.**

## Options

**A. Single tier with a conservative allowlist.** One product, tightly restricted asset classes, KYB on everyone. Simple to build and reason about. Kills the free funnel entirely, since a prospective issuer cannot try anything without clearing KYB first.

**B. Tiered access with escalating verification.** Multiple tiers, each with its own verification requirement, issuance ceiling, and permitted asset classes. More surface to build, but the risk scales with the tier rather than being uniform.

**C. Tiered access enforced in the application layer.** Same tier model, gates implemented in the API and the dashboard. Fastest to build and to change.

## Decision

Option B, with the enforcement question resolved explicitly against option C.

### The tiers

**Tier 0. Anyone, testnet only.** No verification, no caps, all asset classes, free and fully self serve. Risk is structurally zero because nothing on a testnet has value. This is the funnel: it lets an issuer's engineer evaluate the product on a Tuesday afternoon without talking to us, which is the entire product led motion in one sentence.

**Tier 1. Verified entities, mainnet, capped.** Automated business verification through established KYB providers. Issuance ceiling in the low millions. KYC providers restricted to an allowlist. Asset classes restricted to treasuries, commodities, real estate through an SPV, and fund shares. Roughly a five minute onboarding flow with **no human on our side**.

**Tier 2. Enterprise, reviewed.** Manual review by our team, unlimited issuance, custom asset classes including operating company equity where a legal opinion supports it, gated behind a fee, a third party audit, and that legal opinion. This is the high touch tier, and it is priced like one.

### Two design points that carry the model

**The asset class blocklist at Tier 1 is the single highest leverage line in the whole system.** Direct operating company equity is blocked from self serve entirely. That is the asset class most likely to be an unregistered securities offering with a retail victim on the other side, and it is the one where a platform's exposure is worst. Treasuries, commodities, real estate SPVs, and fund shares have established tokenization paths and clearer regulatory footing. Blocking one asset class removes most of the tail risk from the self serve tier while costing almost none of the addressable market, because the issuers who genuinely need equity tokenization are large enough to be worth a Tier 2 conversation.

**Enforcement lives in the contracts, not the interface.** A tier registry holds the per tier configuration and allowlists on chain. The deployment factory checks it before it will deploy. The compliance modules check it before they will permit issuance. This is more work than gating the dashboard, and it is not optional: **a control you can bypass by calling the contract directly is not a control, it is a suggestion.** If the risk model says a Tier 1 tenant cannot exceed an issuance ceiling, then the token itself has to refuse, whether the request arrives through our UI, through a script, or after our company no longer exists.

## Consequences

**Three onboarding funnels to build, document, and support** instead of one. Real cost, absorbed knowingly.

**The Tier 1 ceiling caps self serve revenue.** The largest and most valuable issuers are pushed into a manual process by design. We chose a lower revenue ceiling on the automated path in exchange for a bounded liability on it. I would make that trade again.

**Automated verification false negatives become a support queue.** A legitimate issuer wrongly rejected by an automated KYB check has no human to appeal to, which is the flip side of having no human in the loop. This needed an escalation path and we had not built one.

**Tier boundaries invite argument.** Every issuer who wants Tier 2 capability at Tier 1 pricing will make a case, and some of those cases will be good. Holding the line is a discipline problem, not a technical one, and it is where this kind of model usually erodes.

**On chain enforcement makes the tier registry security critical.** We moved risk from a policy layer into a contract, which means a bug there has a compliance blast radius. That is the right trade because the alternative is a control that does not work, but it is a trade, and it feeds directly into the audit scope discussed in [ADR 0006](0006-security-sequencing-under-a-fixed-audit-budget.md).

## What I would do differently

**Launch Tier 1 with treasuries only, then expand.** We planned to open four asset classes at once. That question was still sitting in the open questions list when the phase began, which means it was going to get decided under launch pressure rather than deliberately.

Treasuries alone would have proven the self serve claim, produced the operational learning, and kept the initial regulatory surface as small as it can get. The other three asset classes could then each be a decision with evidence behind it rather than a launch day assumption. Opening everything at once means that if something goes wrong you do not know which asset class caused it.
