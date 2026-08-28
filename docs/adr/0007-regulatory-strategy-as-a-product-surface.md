# ADR 0007: Treat compliance as a product surface, not a legal review gate

**Status:** Accepted early, held throughout.
**Decided:** 2023 to 2026, reinforced by the platform pivot.

## Context

Tokenized real world assets sit inside securities law. There is no version of this product where that is someone else's problem.

The default pattern in crypto is to build the product, then hand it to counsel, then discover that the thing you built cannot ship in the jurisdiction you need. Legal becomes a gate at the end, and the gate's only available verdict is no. The predictable outcome is a product that either ships somewhere it should not or does not ship at all.

We took the opposite position: **compliance is a product surface, with design decisions, tradeoffs, and users, and it should be enforced by the system rather than promised by the company.**

## Decision

Three concrete expressions of that position.

### 1. Enforce compliance in the contracts

Transfer restrictions, jurisdiction allowlists, holder caps, and issuance ceilings are properties of the token, checked on every transfer, mint, and burn against an identity registry and a set of compliance modules.

The test I used: **a token that cannot be transferred to an unverified address is compliant even if the front end is gone and the company no longer exists.** A token whose restrictions live in an API is compliant only while someone is paying the hosting bill and nobody has learned to call the contract directly.

For an asset class where the whole promise is that ownership is enforceable, that distinction is the product, not an implementation detail. It is also what lets the platform make a claim about its tenants rather than merely a claim about its own conduct.

### 2. Split the entity before the question is tested

Commercial operations and the front end sat in an operating company. The core contracts were built to be a neutral, governance controlled protocol, with admin authority migrating to a timelock and then toward DAO governance. The shape is the same one Uniswap uses to separate Uniswap Labs from the Uniswap Protocol.

This is a structural answer to who is liable for what, decided **before** it was tested rather than reconstructed afterwards under pressure. It also disciplines the roadmap: the split forces you to say out loud which levers are commercial and which belong to protocol governance, and that question is much harder to answer after you have already built a product that assumes you control both.

### 3. Engage the regulator directly and early

I was invited by the U.S. SEC to provide input on tokenization policy.

The value of showing up early is not a favorable ruling, and anyone who tells you otherwise is selling something. The value is **learning which shape of the product is defensible before you have built the other shape.** Regulatory posture is information, and like any other kind of user research it is cheapest to gather before the thing is built. Teams that avoid the conversation until they are forced into it are choosing to get that information at the most expensive possible moment.

The asset class blocklist in [ADR 0003](0003-tiered-access-model.md) is downstream of this. Blocking direct operating company equity from self serve issuance is not caution, it is a specific control aimed at the specific scenario that ends the company.

## Consequences

**Compliance competes with features for the same scarce resources.** The same audit budget ([ADR 0006](0006-security-sequencing-under-a-fixed-audit-budget.md)) and the same 24KB contract size ceiling ([ADR 0005](0005-tenant-composition-clone-not-inherit.md)). On chain compliance is not free, and the currency it is paid in is the currency everything else needs.

**Some markets were closed deliberately.** Jurisdiction allowlists and asset class restrictions mean the product refuses business it could have taken. Every one of those refusals is revenue not earned, and they are visible in the funnel long before the risk they prevent would have been.

**The entity split imposes ongoing governance overhead** and constrains what the operating company can unilaterally change. That constraint is the point, and it is still overhead.

**On chain compliance concentrates a new kind of risk.** A bug in a compliance module is now a smart contract bug with a compliance blast radius, affecting every tenant relying on it. Moving a control from policy into code makes it real, and it also makes it exploitable. That is a genuine trade and not a pure win.

## What I would do differently

**Get the legal opinion on the entity split written earlier.** It was still an open question in the platform plan at the point the platform build started, which is late for a question that determines what the operating company is allowed to do. Structural legal questions should be settled before the architecture that depends on them is built, because the architecture is the expensive thing to change.

**Be clearer, earlier, about which decisions are protocol governance and which are commercial.** We made that call case by case as it came up. A written scope, defined before the DAO existed rather than negotiated once it did, would have prevented an entire category of ambiguity about who could change what.
