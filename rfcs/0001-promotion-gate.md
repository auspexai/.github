# RFC 0001 — Promotion gate: standing certification of curated starter profiles

- **Status:** **Adopted (in force) — open for refinement.** The promotion-gate architecture is deployed and enforced on AuspexAI's coordinator as of 2026-06-22, so the Maintainer has adopted §6.7 internally; it governs the running system now. This RFC stays open **not** to re-decide the core, but to gather refinements and surface new requirements as real researchers use the on-ramp. The text folds into `RESEARCH_ETHICS_POLICY.md` on `main` once the refinement settles.
- **Comment period:** open-ended (refinement). _(Original 30-day window: 2026-06-21 → 2026-07-21, per [`GOVERNANCE.md`](../GOVERNANCE.md) §8.2 / [`RESEARCH_ETHICS_POLICY.md`](../RESEARCH_ETHICS_POLICY.md) §8.)_
- **Amends:** `RESEARCH_ETHICS_POLICY.md` — adds **§6.7** and a **§10 v1.2** changelog entry (the exact text is the diff in the pull request that carries this RFC)
- **Bar:** substantive §8 amendment (simple §8 process; this adds no prohibited-content category, so the §4 60-day bar does not apply)

## Summary

This RFC proposes a **promotion gate**: a way for AuspexAI to grant a **standing, profile-scoped approval** to a curated, declawed *starter profile* of an already-approved experiment, so that an onboarding Researcher can run it **without filing their own per-experiment application**. It is a specialized application of the existing §6 review — not a new governance authority — and it automates two cases that are gated manually today while preserving human review exactly where it adds value.

## Motivation

AuspexAI wants new researchers to *learn the platform by running a real experiment*, not by reading docs and waiting in an approval queue. The cleanest on-ramp is a **catalog of curated, declawed reference experiments** — fixed, benign, hash-only configurations a newcomer can run as-is.

But running any experiment today requires either accrued trust tier (which a newcomer lacks) or a per-experiment review (which doesn't scale to "every newcomer's first run" and defeats the frictionless intent). The promotion gate resolves this by moving the human judgment **from per-run to per-release**: a curated starter is reviewed and certified **once**, and then any eligible Researcher's run of *that exact certified profile* clears §6 automatically.

This automates two currently-manual cases:

1. **Newcomers within a controlled risk footprint** — an onboarding Researcher runs the declawed starter immediately, no per-run approval.
2. **Re-running certified work** — any eligible Researcher (at any accrued tier) can re-run a certified experiment without re-gating, which makes certified experiments freely **reproducible** (a first-class scientific goal, §3).

The manual review queue then shrinks to what actually warrants human judgment: **novel or uncertified work**. The dividing line is *vetted vs. novel*, not newcomer vs. veteran.

## The change

The full proposed policy text is the diff in the accompanying pull request:

- **§6.7 Standing certification of curated starter profiles** — what may be certified (§6.7.1), the safe-to-expose bar (§6.7.2), the declared-safe-knob allowlist (§6.7.3), authority and procedure (§6.7.4), the standing approval (§6.7.5), re-certification (§6.7.6), and its relationship to per-experiment review (§6.7.7).
- **§10 v1.2** changelog entry.

## Worked example

The first profile this gate would certify is the **Vigiles** behavioral-drift starter — a public, inspectable, declawed configuration:

- Tenant repo: <https://github.com/auspexai/vigiles-tenant>
- The signed release snapshot: <https://github.com/auspexai/vigiles-tenant/releases/tag/v0.1.0>
- The starter profile lives in that repo's `experiment.toml` as `[profiles.starter]`: a fixed, benign probe panel, run at temperature 0, reduced to a SHA-256 anchor plus light lexical features — **no raw text leaves the worker** (per §7 containment). It is short, bounded, and reproducible.

So the gate's safe-to-expose bar (§6.7.2) can be checked against a concrete, public artifact rather than in the abstract.

## Why this is *not* new authority

The Research Ethics Policy already grants the first tenant a **standing approval** subject to §6.4 periodic re-review (§7.4). §6.7 generalizes exactly that shape, with two differences: it is scoped to a **profile + released version** (not a whole tenant), and it holds a **higher containment bar** because the audience is an unverified stranger rather than a vetted tenant team. Certification is granted by the same authority as a §6.2 standard review; it invents no new approval body.

## Independence, proportionate to risk

A certified starter is low-risk by construction (§6.7.2). Requiring a heavyweight external-advisor pre-approval for benign, declawed work would be disproportionate and risks being a rubber stamp — *manufactured* assurance. So for low-risk certifications, independence comes from **transparency and contestability**: the certification is **published openly** (the exact certified envelope + the signature, anchored in the transparency log) and **anyone may challenge it** through the contestation path. A named external advisor co-signature is reserved for **high-risk** certifications — which, by definition, a certified starter never is.

This makes the certification mechanism itself an instance of the governance pattern AuspexAI favors: *propose in public, open to challenge.* (This RFC is the same shape.)

Defense in depth backstops it: a certificate gates *approval*, not *containment*. Even an erroneous certification still runs inside the platform's strict execution sandbox under §5.4 routing and bounded resource caps, so its blast radius is bounded and any error is publicly visible and revocable (§6.7.6).

## What this does *not* do

- It does **not** certify *researchers*. It certifies *experiments* (profiles). A Researcher's own novel code is still reviewed under §6.1–§6.3.
- It does **not** replace per-experiment review (§6.7.7) — it complements it.
- It does **not** weaken any containment, prohibited-design, or worker-tier-alignment requirement (§4, §5).

## Alternatives considered

- **A heavyweight external advisor for every certification.** Rejected for low-risk work: disproportionate and rubber-stamp-prone (false assurance). Retained for *high-risk* certifications, where independent pre-review is warranted.
- **A plaintext "certified" flag/registry row.** Rejected: it would be forgeable and *less* verifiable than the research results the gate guards. Certifications should be signed, content-bound, published, and contestable.
- **Gating the researcher's accrued "standing" instead of the experiment's risk.** Rejected for this purpose: accrued standing is a weak proxy for a specific config's risk. The gate certifies the *experiment*; per-experiment review (and containment) governs *novel* work.

## How to comment

Comment on the pull request that carries this RFC, on or before **2026-07-21**. Substantive concerns about the safe-to-expose bar, the proportionate-independence model, or the scope of the standing approval are especially welcome. On consensus at the close of the comment period, the PR is merged and the date in the §10 v1.2 entry is finalized.
