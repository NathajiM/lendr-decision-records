# ADR 0006: Sequence security work to front load the cheap fix window

**Status:** Accepted and followed through wind down.
**Decided:** 2026-05.

## Context

A tier one external audit was the largest single line item between us and mainnet. It is priced by scope and by time, and for a protocol holding other people's money it is not optional.

The budget was fixed. The scope was not. That asymmetry is the whole problem, and it is a resource allocation problem before it is a security problem.

Two constraints shaped the answer.

**Fix cost is not constant over time.** While the deployer still held direct upgrade authority, a fix shipped immediately. Once admin authority migrated to the timelock, every upgrade paid a 48 hour delay, and every emergency became a two stage operation. **The price of a fix rises permanently at that boundary**, and the boundary is one you choose when to cross.

**Auditors are the most expensive way to find a bug you could have found yourself.** Paying by scope for someone to discover what a careful internal read would surface is a straightforward waste of the budget that could instead buy depth on the things only an expert will catch.

## Decision

A single governing sequence: **runtime exploits, then governance hardening, then external audit.**

**Runtime exploit fixes first**, while direct upgrade authority still existed and fixes were instant. Anything already known or discoverable internally got fixed inside the cheap window rather than after it closed.

**Governance hardening second.** Timelock migration, two step ownership transfers, role migration to the multisig. This is what makes a system safe to hand to users and to an auditor, but it is a known shape change with low discovery value. Doing it first would have burned the cheap fix window on work that had no chance of surprising us.

**External audit last**, against a frozen surface that had already been swept. Buy expert attention for the class of problem only expert attention finds.

### What we spent internally before spending externally

**A multi pass deep read of every production contract, run as independent parallel passes.** The parallelism was the point: reviewers who compare notes early anchor on each other and converge on the same blind spots. Independent passes that are reconciled afterwards preserve genuinely different attack intuitions. This produced the highest yield per dollar of anything we did.

**A written threat model.** An explicit trusted and untrusted boundary, including the uncomfortable entries: the off chain custodian is trusted and the protocol cannot enforce its solvency. A role inventory naming, for every privileged role, who holds it and what the blast radius is if that key is compromised. STRIDE per contract.

The organizing invariant was that **no single private key can drain the protocol in one transaction.** Every total loss path had to route through the timelock, where there is a delay to react, or require the off chain custodian to actively collude. Hot wallets that had to exist, the NAV poster and the peg bot, were bounded by caps rather than trusted: a daily performance cap, a drawdown breaker, and a per epoch mint ceiling meant a fully compromised hot key had a bounded and monitorable blast radius instead of an unbounded one.

**Storage layout snapshotting** as an upgrade safety baseline, so that an accidental layout change in an upgradeable contract would fail loudly rather than corrupt state silently.

**A fuzz harness** for NAV math and fee splits, which are where arithmetic edge cases hide, and a static analysis sweep for the mechanical classes.

**An on chain audit attestation registry**, so a tenant's audit status was a queryable property of their deployment rather than a PDF in a folder. This is where a security control becomes a product feature: on a multi tenant platform, investors need to see which tenants have been reviewed, and putting that on chain makes it a signal the platform can surface rather than a claim an issuer can make.

## Consequences

**A designed and ready migration was parked** ([ADR 0005](0005-tenant-composition-clone-not-inherit.md)) because it would have changed the storage layout and invalidated the frozen baseline. Sequencing means saying no to finished work, which is the part that is hard to hold to.

**The findings from the internal review are not in this repository and will not be.** Some of that code lineage may still exist somewhere I do not control. Publishing specific vulnerabilities with file and line references, for a protocol I no longer operate, would be indefensible regardless of how it reads on a resume. The process is the transferable part. The findings are not mine to hand out.

**The sequence assumes a wind down never happens mid plan.** It did. Front loading the cheap fixes meant the work that got done was the highest value work available at each point, which is the best defense available against an ordering you did not choose.

## What I would do differently

**Commission the internal deep read far earlier.** It was the single highest yield activity per dollar spent and it was scheduled late, close to the audit rather than close to the code being written. Running it continuously from the first production contract would have caught issues while they were single line fixes rather than after they had load bearing code built on top of them.

**Build integration level tests against real infrastructure earlier.** The internal review surfaced a class of problem that unit tests structurally could not catch: assumptions about who the caller would be in production that hold perfectly in a test harness and fail against real infrastructure. Every one of those is a deployment blocker that only appears at deployment. A thin end to end path exercised against real testnet infrastructure, early and continuously, finds them while they are cheap.

**Separate the audit budget from the runway.** When the audit is funded out of the same pool as everything else, it is the line item under pressure every time the plan slips, and it slides right. It should have been ring fenced at the moment we decided to own compliance critical code ([ADR 0004](0004-build-erc3643-subset-vs-vendor-t-rex.md)), because that decision is only correct if the audit actually happens.
