# AuspexAI

*A volunteer compute network for AI safety research. AGPL-3.0, donation-and-recognition, no crypto-economy.*

A volunteer-driven, open-source distributed compute network being built for AI safety research. Researchers will propose experiments; volunteers worldwide will donate compute by running a sandboxed worker. Tenant-neutral by design; the first tenant — multi-agent LLM behavioral drift research, carrying forward the Maintainer's prior work in the [Sentinel](https://github.com/jasongagne-git/sentinel) research program — is in development.

## Status

**Phase 2 — Closed beta.** The coordinator daemon, tenant SDK, and multi-platform worker installer are built and running (the live coordinator is up at `coord.auspexai.network`); the operator console and public receipt-verifier are live. Current focus is the first end-to-end tenant validation (a continuous multi-agent run) before opening trusted-beta participation more widely. Phase 3 opens to public volunteers.

**On scale.** Network compute capacity will be directly proportional to the volunteer base. We are starting from zero — every volunteer who joins is part of what makes the next experiment possible.

Public site live at [auspexai.network](https://auspexai.network).

## Principles

- **AGPL-3.0** licensed — strong copyleft so derivative work and network-served forks remain open
- **No crypto-economy** (durable) — no cryptocurrency, no blockchain incentive layer, no on-chain payment, no speculative tokens. Volunteers donate compute under the current operating model; trust runs through signed receipts and named recognition, not tokens
- **Receipts that build trust, not collectibles** — every contribution becomes a signed receipt, a persistent and citable record linking a volunteer's machine to a specific experiment (like a DOI for compute contribution). Receipt history accumulates as the network's trust substrate, unlocking higher-trust roles over time: unique work assignments, vouching power, Approver eligibility. Plus mandatory tenant acknowledgment in publications. No leaderboards, no scores, no badges.
- **Volunteers never paste keys** — OAuth Device Flow + OS-keystore for all credentials
- **Untrusted-worker by default** — result replication and signed submissions are core, not optional
- **Multi-tenant from day one** — the platform is being designed for multiple tenants; the first tenant is in development for Phase 1, and the SDK and tenant-acceptance process will open to other research projects from Phase 1 onward

## Governance & policies

- **[Governance](https://github.com/auspexai/.github/blob/main/GOVERNANCE.md)** — roles, decision rules, recruitment, conflict of interest
- **[Code of Conduct](https://github.com/auspexai/.github/blob/main/CODE_OF_CONDUCT.md)** — community standards, reporting, escalation pathway
- **[Contributing](https://github.com/auspexai/.github/blob/main/CONTRIBUTING.md)** — how to contribute (Platform Contributor and Researcher paths)
- **[Research Ethics Policy](https://github.com/auspexai/.github/blob/main/RESEARCH_ETHICS_POLICY.md)** — what AI safety research will be accepted on the network and how it's reviewed

## Contact

- General: [contact@auspexai.network](mailto:contact@auspexai.network)
- Research / tenant inquiries: [research@auspexai.network](mailto:research@auspexai.network)
- Security: [security@auspexai.network](mailto:security@auspexai.network)
- Conduct: [conduct@auspexai.network](mailto:conduct@auspexai.network)

Watch this organization for repository activity as Phase 1 begins.
