# RFC 0002 — Researcher tier alignment: research-standing (R0–R3) gating of risk classes

- **Status:** **Adopted (in force) — open for refinement.** The research-standing ladder (R0–R3) and the submit-time R-gate are deployed and operative on AuspexAI's coordinator as of 2026-06-24 (warn-but-allow, consistent with the human-in-the-loop default), so the Maintainer has adopted §5.4 Researcher-alignment internally; it governs the running system now. This RFC stays open **not** to re-decide the core, but to gather refinements as real researchers graduate the ladder — most concretely, where the **low/medium boundary** falls in practice. The text folds into `RESEARCH_ETHICS_POLICY.md` on `main` once the refinement settles.
- **Comment period:** open-ended (refinement). _(Original 30-day window: 2026-06-24 → 2026-07-24, per [`GOVERNANCE.md`](../GOVERNANCE.md) §8.2 / [`RESEARCH_ETHICS_POLICY.md`](../RESEARCH_ETHICS_POLICY.md) §8.)_
- **Amends:** `RESEARCH_ETHICS_POLICY.md` — retitles **§5.4** ("Worker tier alignment" → "Tier alignment"), adds the **Researcher alignment** paragraph, and adds a **§10 v1.3** changelog entry. Companion: `GOVERNANCE.md` — **§11** names research-standing demotion and BYOT-revocation as contestable adverse actions (**§9 v1.5**). The exact text is the diff in the pull request that carries this RFC.
- **Bar:** substantive §8 amendment (simple §8 process; this adds no prohibited-content category, so the §4 60-day bar does not apply). The GOVERNANCE §11 companion is a routine clarifying amendment (§8.1).

## Summary

This RFC adds the **symmetric researcher half** to §5.4. The policy already aligns the *workers* that **compute** an experiment to its risk (sensitive outputs route only to sufficiently-trusted workers). This RFC aligns the *Researcher* who **proposes** it: a Researcher's research-standing tier (R0–R3) gates which dual-use risk classes (§6.1) they may propose, and supply their own experiment code ("BYOT") for. Both sides of every experiment are then trust-aligned to its risk — the firewall philosophy, end to end.

## Motivation

The on-ramp (RFC 0001) lets an unverified newcomer run a curated, certified starter. But the platform's purpose is for researchers to *graduate past the starter* — earn standing, bring their own code, and (eventually) propose higher-risk work. That graduation needs a policy footing: a stated, fair relationship between **earned standing** and **what you may propose**.

Without it, the only two states are "run the certified starter" and "file a full §6 application," with nothing in between and no articulated path. The ladder fills that gap with an explicit, contestable progression — and it does so **without inventing any new authority or any new risk taxonomy.**

## The change

The full proposed text is the diff in the accompanying pull request:

- **§5.4 retitle + Researcher alignment** — the R0–R3 → risk-class table (R1 = certified starters / low-risk only; R2 = BYOT for low + medium; R3 = eligible for high-risk), the necessary-never-sufficient principle, and the contestability of an R-demotion / BYOT-revocation.
- **§10 v1.3** changelog entry.
- **`GOVERNANCE.md` §11** — research-standing demotion and BYOT-revocation named in the contestable-actions list (§9 v1.5).

## The ladder

| Tier | Earned by | May propose / supply code for |
|---|---|---|
| **R1** identity-verified | linked, verified identity | certified reference experiments only (no own code); **low-risk** designs |
| **R2** established | competence record **+ ethics review** | **BYOT** own code; **low + medium**-risk; under STRICT containment |
| **R3** trusted | extended clean R2 record **+ Maintainer vetting** | **eligible** to propose **high-risk** designs (still per-experiment Approver review) |

## Why this is *not* new authority

R-promotions are **human decisions recorded with reasons**, exactly like every other trust action on the network. The standing record computes only *eligibility to be reviewed*; it never auto-promotes — R1→R2 is an ethics review, R2→R3 is Maintainer vetting. Building R-promotion as an automatic formula would be the dishonest version, and is explicitly rejected. The gate reuses the existing §6 review machinery; it invents no new approval body.

## Why this defines *no new risk taxonomy*

"Low / medium / high" are the **existing** §6.1 dual-use self-assessment classes, reviewed (and reclassifiable) by the Approver pool, with §6.3 already setting high-risk = unanimity + documented review. This RFC ties the *ladder* to those classes; it does **not** redefine them. The exact **"medium" boundary** therefore stays a **reviewed judgment**, not a bright line — consistent with how §6.1/§6.3 already operate. Pinning that boundary in practice is the main thing this RFC stays open to refine.

## Necessary, never sufficient

Research-standing confers only eligibility to *propose*. Every experiment is still classified and authorized on its own merits under §6: high-risk designs still require §6.3 elevated review (Approver unanimity; external advisor where warranted), raw harmful-output publication still requires unanimity and documented rationale under §5.2, and §4-prohibited designs remain prohibited **at every tier**. The ladder places a *floor beneath* those gates; it never relaxes them. Standing and per-experiment review are **two independent locks** — the worst capability needs the most locks.

## Contestation (companion to A8 / §11)

An R-demotion or a BYOT-revocation is an **adverse action**, contestable under `GOVERNANCE.md` §11. The §11 companion edit names both explicitly. By §11's standard, research-standing is reduced **only by provable misconduct, never by a divergent or dissenting result** — the firewall-#1 corollary, now applied to the researcher axis: you cannot lose standing for being wrong, only for misconduct.

## What this does *not* do

- It does **not** auto-promote anyone. Standing earns the *review*, never the promotion.
- It does **not** define new risk classes, nor a mechanical rubric for "medium." Classification stays a reviewed Approver call.
- It does **not** relax any §6.3, §5.2, §5 containment, or §4 prohibited-design requirement. It is additive eligibility, gated above the existing review.
- It does **not** gate *workers* (that is the unchanged worker half of §5.4) — it gates who may *propose*.

## Alternatives considered

- **Auto-promote R-tiers from an attested-history formula.** Rejected: the corroborated compute tier *can* auto-promote because it is mechanically verifiable; research judgment (ethics fit, BYOT trust, high-risk readiness) is not, so its promotion must stay a human decision. The formula computes eligibility-to-be-reviewed only.
- **A mechanical low/medium/high rubric in the policy.** Rejected: §6.1/§6.3 deliberately keep classification a reviewed, reclassifiable judgment; a bright-line rubric would either over- or under-include and invite gaming. The boundary stays reviewed.
- **Gate only at the promotion action, not at submit.** Rejected: the submit-time R-gate is the keystone — it is what "BYOT" actually means in enforcement. Gating only at promotion would leave the proposal path ungoverned.

## How to comment

Comment on the pull request that carries this RFC, on or before **2026-07-24**. Concerns about the low/medium boundary, the necessary-never-sufficient framing, or the R-tier earning criteria are especially welcome. On consensus at the close of the comment period, the PR is merged and the date in the §10 v1.3 entry is finalized.
