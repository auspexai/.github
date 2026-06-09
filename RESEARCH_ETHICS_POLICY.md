# AuspexAI Research Ethics Policy

**Status:** v1, 2026-04-27
**Applies to:** all tenant experiments that run on the AuspexAI public network, and tenant repositories hosted in the `auspexai` GitHub organization
**Adjacent governance documents:** [`GOVERNANCE.md`](GOVERNANCE.md) §3.4 (Approver acceptance bar incorporates this policy), [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) (agent behavior in experiments points here)

---

## 1. Purpose

AuspexAI is built to enable AI safety research. A meaningful share of useful AI safety research requires observing AI behavior that, were it produced for non-research purposes, would be considered harmful — alignment failures, drift toward toxicity, jailbreak susceptibility, adversarial responses, capability emergence under pressure. The community's tolerance for such research is conditional on its being conducted responsibly: with clear scientific purpose, appropriate containment, and outputs that contribute to understanding rather than to harm.

This policy makes the conditions explicit. It defines what AuspexAI considers legitimate research, what we will not host, how the Approver pool reviews experiments against ethics criteria, and what containment we expect from approved experiments.

The policy is a top-level governance document at the same weight as `GOVERNANCE.md` and `CODE_OF_CONDUCT.md`. Adopting it is a precondition of running tenant experiments on the AuspexAI network.

This policy draws on established dual-use research of concern (DURC) frameworks adapted for AI research contexts, on AI safety community norms around responsible disclosure of harmful capabilities, and on institutional review board (IRB) practice for non-human-subjects ethics review. It does not adopt any single existing framework wholesale; it is purpose-built for AuspexAI's specific posture.

---

## 2. The dual-use distinction

The central distinction in this policy is between two superficially similar but ethically different activities:

- **Studying harmful behavior** — designing an experiment whose research subject is some category of unwanted AI behavior (alignment failure, drift toward toxicity, jailbreak susceptibility, capability emergence, etc.), with the goal of producing scientific understanding. The harmful behavior is the *phenomenon under study*; the research output is *findings about that phenomenon*.

- **Producing harmful artifacts** — designing an experiment whose primary deliverable is harmful content or capability (harassment messages for distribution, weaponized code, instructions enabling mass-casualty harm, illegal content), regardless of whether the design is framed in research language. The harmful artifact is the *output product*; the "research" framing is incidental.

The first is permitted with containment. The second is not. The distinction is in **purpose and output handling**, not in what the agents do during the experiment. A drift study and a harassment-content-generation tenant could superficially involve identical agent behavior; they are not the same kind of work, and the difference is what the experiment is *for* and what it produces *as deliverables*.

The Approver pool applies this distinction as the analytical frame for every tenant application.

---

## 3. Permitted research

The following categories of AI safety and AI behavioral research are explicitly within scope and welcome on AuspexAI:

- **Alignment failure analysis** — studies of when, how, and under what conditions language models or agent systems produce outputs that misalign with stated goals or values
- **Behavioral drift detection** — studies of how multi-agent systems, agents under sustained operation, or agents under adversarial input drift in their behavior over time (Sentinel's domain)
- **Jailbreak research** — controlled study of prompts, manipulations, or contexts that cause models to bypass safety constraints, including research that requires producing successful jailbreaks for analysis
- **Adversarial robustness** — how models behave under hostile inputs, including research that requires generating those inputs
- **Capability emergence** — study of when and how harmful or dual-use capabilities emerge in models or agent systems under specific conditions
- **Red-teaming and safety stress-testing** — controlled evaluation of model or agent safety properties under designed pressure
- **Interpretability research that surfaces harmful behavior** — probing studies where the goal is mechanistic understanding, even when the probe surfaces concerning behavior
- **Replication and meta-research** — studies that verify, extend, or systematically review prior research findings, including those involving harmful behavior

This list is illustrative, not exhaustive. The Approver pool may approve other research that fits the dual-use legitimate-study principle in §2, with documented rationale.

---

## 4. Prohibited experiment designs

The following are not approved on AuspexAI. Applications proposing them are rejected:

- **Primary purpose: producing harmful content for non-research use.** Experiments whose intended deliverable is harmful artifacts (harassment messages for distribution, weaponized capabilities, illegal content) for any purpose other than research analysis. The "research framing" test: if the experiment's value disappears when the harmful output is contained to research records, the design fails this test.

- **Experiments directed at non-consenting human targets.** Research in which human subjects (real people, real conversations, real social environments) are the target of harmful agent behavior without explicit, prior, informed consent obtained under appropriate human-subjects-research procedures.

- **Containment-evasion experiments.** Research that explicitly aims to leak, distribute, or otherwise propagate harmful outputs beyond research-controlled environments. Experiments that test containment systems are permitted; experiments designed to defeat them are not.

- **Categorically prohibited content.** Experiments whose intended outputs include or aim to include any of:
  - Child sexual abuse material (CSAM) of any kind
  - Content depicting non-consenting individuals in sexual or otherwise compromising contexts
  - Content directly enabling specific terrorist attacks or mass-casualty operations
  - Content directly enabling synthesis of mass-casualty weapons (biological, chemical, radiological, nuclear)
  - Content otherwise illegal under applicable law in major jurisdictions (US, EU/UK, with cross-jurisdictional refinement deferred — see §11)

  These prohibitions hold regardless of research framing. Even purely-evaluative or synthetic-only research designs in these domains are out of scope on AuspexAI; researchers seeking to study these areas must use other infrastructure with domain-appropriate review.

- **Experiments lacking genuine research value.** Research framings whose primary purpose appears to be accessing AuspexAI compute for non-research goals. The burden is on the application to demonstrate scientific merit; the Approver pool may reject applications that fail to do so, with documented rationale.

- **Experiments organized around tradeable contribution receipts.** Tenants may not structure their experiments such that the contribution receipts produced are framed as tradeable assets, financial instruments, or speculative-value tokens. Specific examples that fail this test: marketing volunteer recruitment with "earn-and-sell" or "stake-and-trade" promises; partnering with an off-protocol marketplace to enable resale of receipts produced by the tenant's experiments; wrapping receipts as NFTs or fungible tokens with implied monetary value; using receipt accumulation as collateral for off-platform financial products. Contribution receipts are credentials of work performed for AI safety research; per the AuspexAI Principles & Scope §4 #7(a) durable no-crypto-economy stance and §6.8.2 structural-defenses framework, they are not transferable assets. Tenants who organize around tradeable-receipt framing are subject to revocation under §6.6. (Added 2026-05-17 alongside `shadow_crypto_economy_guardrails.md`.)

This list is not exhaustive. The Approver pool may reject other applications that fail the dual-use legitimate-study principle in §2.

---

## 5. Containment requirements

Approved experiments that produce harmful outputs as part of their research process must include containment plans covering each of the following:

### 5.1 Output handling

- Where generated content lives (storage system, access controls)
- Who has access (Researcher team, named collaborators, no broader access)
- How long it is retained (retention period tied to research purpose; default: as long as analysis is ongoing, then deletion or archival under tighter access)
- How it is destroyed when retention ends

### 5.2 Public reporting

Research findings published from the experiment use one or more of:

- **Redaction** — harmful outputs replaced with descriptions, blanks, or metadata
- **Aggregation** — findings reported at the level of patterns, frequencies, conditions; not individual outputs
- **Synthetic representation** — illustrative examples reconstructed to match the analytical pattern without reproducing actual harmful outputs
- **Controlled disclosure** — vetted-researcher access models for the subset of cases where raw outputs are scientifically essential (this is rare and requires specific justification)

Direct publication of raw harmful outputs is not permitted as a default. Exceptions require Approver-pool unanimity and documented rationale that publication's research value clearly exceeds harm potential.

### 5.3 Audit logging and storage layer

- Experiment systems distinguish research-internal data (potentially harmful outputs) from publicly-distributable artifacts (results, summaries, metadata) at the storage layer
- Audit logs record who accessed what, when, and for what purpose
- The Tenant SDK and platform features are designed to support these requirements (see Plan §5.12); Researchers are responsible for using the supporting features correctly

### 5.4 Worker tier alignment

Experiments handling sensitive outputs are routed only to workers at trust tiers appropriate to the sensitivity. Capability-emergence studies and similar high-sensitivity research are routed to T2+ workers; less-sensitive drift research can use T1+; T0 anonymous workers do not handle research-internal harmful outputs (T0 work is replicated and quorum-checked, not assigned to sensitive outputs).

### 5.5 Incident response

Containment failure (leak, accidental disclosure, worker compromise, unanticipated output category) triggers documented incident response within 24 hours of Researcher awareness:

- Notification to the Maintainer team (`security@auspexai.network`)
- Pause of experiment operation pending review
- Assessment of harm scope
- Remediation steps appropriate to harm scope
- Public disclosure of the incident, redacted to preserve containment of the leaked content itself

Detailed incident response procedure is specified in [`SECURITY.md`](SECURITY.md).

---

## 6. Application and review process

### 6.1 Application contents

Tenant applications include, alongside the standard application materials defined by the Approver pool, the following ethics-specific materials:

- **Research purpose statement** — 1–3 paragraphs covering what is being studied and why; includes the scientific question and the expected research output
- **Dual-use risk classification** — Researcher self-assessment of low / medium / high risk, with reasoning
- **Anticipated harmful outputs** — candid description of what categories of harmful outputs the experiment expects to generate (alignment-failure transcripts, drift-toward-toxicity examples, jailbreak prompts, etc.) and why the research requires producing them
- **Containment plan** — covering each subsection of §5 as applicable
- **Mitigation and revocation triggers** — under what conditions the Researcher commits to pausing, redesigning, or terminating the experiment
- **Compliance with §4** — explicit confirmation that the experiment does not fall within any prohibited-design category, with reasoning

### 6.2 Standard Approver review

The Approver pool reviews each application against:

1. Research merit — does the experiment have genuine scientific purpose?
2. Dual-use distinction (§2) — is this studying harmful behavior or producing harmful artifacts?
3. Containment adequacy (§5) — does the plan match the risk profile?
4. Compliance with prohibited-design list (§4)
5. Code of Conduct alignment — does experiment design meet the constraint in `CODE_OF_CONDUCT.md` "Agent behavior in experiments"?

Standard approval requires simple majority of the Approver pool, per `GOVERNANCE.md` §4.3.

### 6.3 Elevated review for high-risk experiments

Experiments classified high-risk (by Researcher self-assessment or by Approver-pool reclassification) require:

- **Approver pool unanimity** rather than simple majority
- **Documented dual-use review** explicitly addressing the legitimate-study vs harmful-artifact distinction with reasoning
- **External reviewer (advisor)** drawn from senior unaffiliated AI safety, research-ethics, or domain-specific community members, in cases where the Approver pool concludes additional perspective is warranted
- **Public statement** of the elevated review (its existence and outcome, not necessarily contents) in the Approver decision record

**High-risk experiments cannot proceed under the Approver-pool bootstrap fallback** described in `GOVERNANCE.md` §5.2. They require a fully-formed Approver pool with floor of 2 external members. If a high-risk application arrives during bootstrap, it is held pending Approver-pool formation. Researchers with high-risk research needs should plan accordingly; AuspexAI prioritizes Approver-pool formation in part because of this constraint.

### 6.4 Periodic review of approved experiments

Long-running experiments (more than 30 days continuous operation) are subject to periodic Approver-pool check-ins, every 90 days minimum, covering:

- Has the experiment behaved as anticipated?
- Have unexpected harmful outputs surfaced that exceed the approved containment plan?
- Have research findings emerged that change the dual-use risk profile?
- Are the containment mechanisms still adequate given accumulated outputs?

Researchers are expected to surface novel ethics issues proactively, not only at the periodic checkpoint.

### 6.5 Concerns about approved experiments

Anyone may raise an ethics concern about an approved experiment by:

- Filing an issue in the relevant tenant repo (if in `auspexai` org) or in `auspexai/.github` if not
- Emailing `research@auspexai.network`
- Following the Code of Conduct reporting procedure if the concern involves human conduct as well as experiment design

The Approver pool reviews raised concerns within 14 calendar days. Outcomes range from "no action — concern addressed" to "experiment paused pending re-review" to "approval revoked." Concerns and outcomes are documented in the public record.

### 6.6 Revocation

The Approver pool may revoke a previously-granted approval for cause:

- Containment failure
- Significant departure from approved experiment design
- Ethics concerns surfaced during operation
- Changed circumstances (newly-discovered capabilities, changed legal landscape, new community-norm developments)

Revocation requires the same procedure as initial approval (standard or elevated, matched to the experiment's risk classification). Revocation decisions are documented publicly with rationale; revoked tenants may re-apply with revised designs.

---

## 7. First tenant as the worked example

AuspexAI's first tenant — a multi-agent LLM behavioral drift research project carrying forward the Maintainer's prior work in the [Sentinel](https://github.com/jasongagne-git/sentinel) research program — is the worked example of this policy in operation. Walking through the policy as it applies to the first tenant makes the abstract criteria concrete.

### 7.1 Application of §3 (permitted research)

The first tenant falls under "Behavioral drift detection" — it studies how multi-agent LLM systems drift in their behavior over sustained operation, including drift toward harmful behavior such as toxicity, unaligned reasoning, or destabilized agent identity. (Sentinel's published research established the methodological foundation this tenant builds on.) The research subject is the drift phenomenon itself; the research outputs are scientific understanding of when, how, and why such drift occurs and what conditions exacerbate or attenuate it.

### 7.2 Application of §4 (prohibited designs)

The first tenant is not within any prohibited category:

- Not designed primarily to produce harmful content for non-research use — deliverables are research papers, datasets of drift evidence (redacted), and analytical findings, not harmful outputs
- Not directed at non-consenting human targets — agents are evaluated against each other and against benchmark prompts; no real users are recruited as experimental subjects
- Not designed to evade containment — the experiment's containment is part of the design, not a barrier to be tested or bypassed
- Does not aim to produce categorically-prohibited content — the experiment design includes filters and termination conditions for runs that approach categorically-prohibited domains (CSAM, mass-casualty content, etc.)

### 7.3 Application of §5 (containment)

First tenant containment plan:

- **Output handling**: all experiment outputs (agent transcripts, drift logs, evaluation results) reside in tenant-owned storage with audit logging; access is limited to the tenant team (the Maintainer's Sentinel research collaborators) and named additional collaborators
- **Public reporting**: published research uses representative excerpts (redacted where harm potential exists), aggregated metrics, and synthetic illustrations of drift patterns; raw transcripts are not publicly released by default
- **Audit logging**: storage layer distinguishes "drift evidence" (research-internal, potentially harmful) from "drift findings" (publicly distributable, anonymized/aggregated)
- **Worker tier alignment**: the first tenant runs on T1+ workers during Phase 1 lab-mode operation; routing escalates to T2+ when public-network operation begins (Phase 2+)
- **Incident response**: containment failures trigger immediate experiment pause and notification per §5.5

### 7.4 Application of §6 (review)

The first tenant is classified **medium dual-use risk** by the Maintainer's self-assessment. The reasoning: drift outputs include harmful content categories (toxic remarks, manipulative reasoning, identity-destabilization patterns) that require containment, but the research design does not approach categorically-prohibited content and the legitimate-study purpose is well-established in the AI safety literature (see Sentinel's published findings for the prior-art basis).

Until the Approver pool seats with floor of 2 external members, the first tenant's approval proceeds under the Maintainer-as-tenant-author procedure (`GOVERNANCE.md` §6.3): the Maintainer recuses from approving their own tenant, and an external advisor (named in the public record) must concur for the approval to stand. Once the permanent Approver pool seats, the first tenant's standing approval is reviewed under the standard process and subject to the §6.4 periodic check-ins.

### 7.5 What the first tenant does NOT do — making the dual-use distinction concrete

The first tenant's agents may produce harmful outputs during a drift cascade — toxic remarks, manipulative suggestions, agent-on-agent abuse patterns (the same categories Sentinel's prior research has documented). These outputs are research evidence, not deliverables. They are logged, analyzed, and reported on at the level of "observed drift toward toxicity in X% of experimental conditions, with the following triggering factors observed across the failure cases..." The raw outputs are not published, distributed, or made available outside research-controlled access.

A counterfactual tenant that would fail this policy under similar agent behavior: a tenant whose deliverable is "100,000 examples of multi-agent toxic dialog" intended for distribution as a corpus, with the toxic content as the output product. Even framed in research language, that tenant is producing harmful artifacts rather than studying the phenomenon. The dual-use distinction (§2) cleanly rejects such a design while approving the first tenant.

---

## 8. Amendment process

Amendments to this policy follow the same process as substantive amendments to `GOVERNANCE.md`, per `GOVERNANCE.md` §8.2: public RFC, 30-day comment period, Maintainer-team consensus or vote, version recorded in §10 changelog below.

Categorically-prohibited-content additions (§4 last bullet) require a higher bar: 60-day comment period, explicit Maintainer-team consensus (no simple-majority pathway), documented dual-use review of the proposed addition. This is to prevent scope creep that would block legitimate research domains by accumulation.

---

## 9. Out of scope

This policy does not cover:

- **Research conducted by AuspexAI Researchers off-network** — work outside AuspexAI is the Researcher's own ethics regime; AuspexAI takes no position
- **Research involving human subjects in ways beyond what is incidental to AI-agent experiments** — for human-subjects research, AuspexAI defers to the Researcher's institutional IRB or equivalent and does not provide a substitute
- **Volunteer Worker conduct on the network** — see the volunteer Terms of Participation ([`BETA_TERMS.md`](BETA_TERMS.md), closed-beta)
- **Maintainer / Approver / Platform Contributor / community member conduct in community spaces** — see [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md)
- **Security disclosure for vulnerabilities** — see [`SECURITY.md`](SECURITY.md)
- **Tenant code's own internal ethics processes** — large tenant teams may have their own ethics boards; AuspexAI Approver-pool review is complementary to, not a substitute for, such processes

---

## 10. Versioning and changelog

| Version | Date | Changes |
|---------|------|---------|
| v1 | 2026-04-27 | Initial. Establishes dual-use distinction, permitted/prohibited research types, containment requirements, application and review process, periodic review for long-running experiments, Sentinel as worked example. |
| v1.1 | 2026-05-22 | §7 reframing: first tenant identified as carrying forward the Sentinel methodological lineage rather than as Sentinel itself, reflecting the 2026-05-22 first-tenant rebuild decision. Substantive policy positions (permitted categories, prohibited designs, containment requirements, dual-use classification, recusal procedure) unchanged; Sentinel research program retained throughout §7 as the methodological prior work. Editorial clarification, not a §8 substantive amendment. |

---

## 11. Open questions

These are surfaced for future revision rather than resolved here.

- **Cross-jurisdictional content categorization** — §4's "categorically prohibited content" list references illegality "under applicable law in major jurisdictions (US, EU/UK)." How to handle conflicts where content is legal in one major jurisdiction and illegal in another, and whether to extend the major-jurisdiction list, is deferred to a future revision (likely Phase 2/3 when global volunteer base is real).
- **External reviewer pool for high-risk applications** — §6.3 calls for external reviewers in some high-risk cases. Whether to maintain a small pre-arranged pool of reviewers (analogous to the deferred CoC Ombud question) or to recruit per-case is unresolved. The first high-risk application's review will likely settle this in practice.
- **Capability-disclosure vs containment tension** — when research surfaces a previously-unknown harmful capability, when does responsible disclosure require publishing details (so defenders can prepare) vs containment require withholding them (so attackers cannot weaponize)? AuspexAI defers to AI safety community norms for now (e.g., established responsible-disclosure practices in security research, with model-capability adaptations). Explicit policy may be needed as cases arise.
- **Tenant-internal ethics processes** — large tenant teams may have their own ethics boards. The default position is that AuspexAI Approver-pool review is complementary to existing tenant-side ethics processes, not exclusive. How conflicts between the two are resolved (if AuspexAI Approver-pool review concludes differently from a tenant's own ethics board) is unspecified.
- **Approver-pool vetting of dual-use expertise** — `GOVERNANCE.md` §5.2 vetting criteria emphasize research credibility but do not require dual-use ethics expertise specifically. As the Approver pool forms, whether this should be a vetting criterion or whether ad-hoc external advisor consultation per §6.3 is sufficient remains to be tested.
