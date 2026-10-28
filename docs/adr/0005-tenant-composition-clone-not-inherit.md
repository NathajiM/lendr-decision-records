# ADR 0005: Compose first party assets as tenants rather than inherit compliance

**Status:** Accepted 2026-06-04. Implementation deliberately parked behind the audit readiness batch.
**Decided:** 2026-06-04.

## Context

After [ADR 0002](0002-whitelabel-platform-before-first-party-assets.md), our own four assets were redefined as tenants on the platform. But they had been built before the compliance layer existed, as standalone vaults with no ERC-3643 behavior.

To ship them under the new model they needed compliance. There were two ways to get there, and the choice determined the shape of the codebase for everything after it.

## Options

**A. Inheritance.** Fold the ERC-3643 compliance layer directly into the vault contract. One contract per asset, one deployment, no cross contract calls on transfer. Conceptually tidy: the vault share simply *is* a compliant token.

**B. Composition.** Deploy each first party asset as a compliance token plus a paired vault clone, through the same factory that deploys third party tenants. Two contracts per asset, wired together at deploy time.

## Decision

Option B, composition.

**1. Contract size was a hard ceiling, and we were near it.**

The vault already carried NAV accounting, fee first accrual, high water mark tracking, a daily performance cap, a drawdown circuit breaker, a Proof of Reserve hook, withdraw queue integration, and a two step NAV pre commit to defend against front running. That is a lot of contract.

EIP-170 caps deployed bytecode at 24KB. Adding a full compliance layer on top risked crossing it. **The worst possible time to discover a size ceiling is halfway through an audit**, when the fix is to restructure the contract that the auditor has already reviewed. Inheritance took a known ceiling and bet the schedule against it for a stylistic gain.

**2. Inheritance couples the audit scope permanently.**

If compliance lives inside the vault, then every compliance change re-scopes the vault audit. Fee logic, NAV math, and transfer restrictions become one audit unit that must be reviewed together forever. Composition keeps two units that can be reviewed and re-reviewed independently, which directly controls the recurring cost of the thing that was already our largest line item ([ADR 0006](0006-security-sequencing-under-a-fixed-audit-budget.md)).

Architectural decisions that reduce future audit scope are worth real complexity. This is specific to security critical on chain code, where review is metered and expensive, and it is the sort of constraint that does not transfer from ordinary software judgment.

**3. Dogfooding stops being a virtue and becomes structural.**

If our own assets deploy through the same factory that paying tenants use, then **the tenant path cannot silently rot**, because we are running on it. Every bug in the tenant deployment flow hits us first. With inheritance, first party assets would have taken a private path, and the customer path would have been exercised only by customers, which is where you find out about breakage last and most expensively.

This converted first party assets into "Lendr owned tenants," which was also the honest description of what they had become strategically.

## Consequences

**Two contracts per asset instead of one.** Higher deployment cost, more wiring, more to get wrong at deploy time. Mitigated by routing everything through the factory so the wiring is executed by tested code rather than by a human running scripts.

**A cross contract call on every transfer.** Real gas cost on every user action, paid permanently for the separation.

**The migration touched everything.** Moving four existing vaults to the composed form meant rewriting every vault test and changing the storage layout, which for upgradeable contracts is the single most dangerous kind of change.

**So it was parked.** The decision was made, the implementation outline was written, and the work was deliberately not done, because it sat behind the audit readiness batch. Changing the storage layout would have invalidated the exact upgrade safety baseline that had to stay frozen for the audit.

Parking a finished design is uncomfortable. It looks like indecision in a standup and it feels like waste. It was correct: the sequencing in [ADR 0006](0006-security-sequencing-under-a-fixed-audit-budget.md) says the audit surface freezes before the audit is bought, and a storage layout migration is the loudest possible violation of that. **A decision recorded and scheduled is not the same as a decision deferred**, and keeping the distinction visible is most of what a decision log is for.

## What I would do differently

Very little on the substance. The size ceiling and the audit coupling arguments are both correct and both would apply again.

**I would have written the decision down before the debate rather than after it.** This choice was litigated more than once in different forms over several weeks, because the reasoning lived in people's heads and in scattered pull request comments. Inheritance could push the contract past Ethereum's 24KB limit. Splitting the contract fixes that, but splitting halfway through an audit means reworking code the auditor already reviewed. Composition avoided the risk entirely. Writing it down the first time it came up would have saved the second and third rounds.

That is the general case for this repository existing at all.
