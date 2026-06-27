# AuspexAI Governance

**Status:** v1, 2026-04-27 (Phase 0 bootstrap)
**Applies to:** the AuspexAI organization on GitHub (`github.com/auspexai`) and all projects under its umbrella, including the Platform, Worker, Tenant SDK, and tenant project modules maintained within the org.

---

## 1. Purpose and scope

This document defines:

- The **roles** that exist in the AuspexAI project and what each can do
- The **rules** by which decisions are made
- The **process** by which roles are filled and people are held accountable
- The **process** by which this document itself changes

Adjacent documents:

- [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) — community standards and CoC enforcement procedure
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — how to make contributions, including DCO sign-off mechanics
- [`SECURITY.md`](SECURITY.md) — security disclosure process

For the project's strategic direction, mission, and roadmap, see the AuspexAI Principles and Scope working document.

---

## 2. Project mission (relevant context for decisions)

AuspexAI is a volunteer-driven, AGPL-3.0-licensed, public distributed compute network for AI research. Decisions taken under this governance are made in service of that mission. **One durable commitment is structurally protected**: the project does not have a crypto-economy layer (no cryptocurrency, no blockchain incentive layer, no on-chain payment, no speculative-token model). Trust runs through signed contribution receipts and named recognition in publications, not through tokens or financial instruments. **Other operational-model questions** — including the volunteer-compensation model (currently donation-only), the project's revenue posture (currently donation-funded), and the donate-only constraint — are not structurally protected; they are the current operating model and are amendable through normal §8.2 governance. The maintainer cannot honestly commit on behalf of the project to forever-no-compensation or forever-no-revenue stances and does not. See `AuspexAI_Principles_and_Scope.md` §4 #7 for the full two-part stance. Governance decisions that would conflict with the durable crypto-economy stance are out of scope under §8.5.

---

## 3. Roles

A single person can hold multiple roles at once. Roles are capability bits, not exclusive categories.

### 3.1 Volunteer

A person running an AuspexAI Worker on their own machine, donating compute. Trust tier T0–T3 governs work eligibility and quorum weight (defined in the AuspexAI Worker documentation). A Volunteer at any tier is bound by this governance and the Code of Conduct insofar as their actions affect AuspexAI community spaces. Conduct on the Worker network itself is additionally governed by the Volunteer Terms of Participation (published as [`BETA_TERMS.md`](BETA_TERMS.md)).

### 3.2 Platform Contributor

A person who submits code or documentation contributions to AuspexAI Platform, Worker, Tenant SDK, or related infrastructure repositories.

Capabilities:

- Open issues and pull requests against any AuspexAI repository
- Participate in code review and technical discussion
- Subject to this governance and the Code of Conduct
- All contributions require DCO sign-off (see `CONTRIBUTING.md`)

Platform Contributors are eligible for promotion to co-Maintainer per the recruitment process in §5.1.

### 3.3 Researcher

A person who authors tenant project code (experiment manifests, project modules, analysis pipelines) intended to run on the AuspexAI network.

Capabilities:

- Submit tenant applications to the Approver pool
- Once approved, ship project module updates per the Tenant SDK contract
- Subject to this governance, the Code of Conduct, and the Approver pool's tenant-acceptance bar

A Researcher whose tenant has been approved retains responsibility for their tenant's adherence to project values and ongoing review.

### 3.4 Approver

A member of the Approver pool.

Capabilities:

- Review tenant applications against the published acceptance bar
- Approve or reject experiments to run on the public network
- Maintain and revise the tenant-acceptance bar (consensus among Approvers)
- Recuse from approving experiments where they have conflicts of interest (see §6)

The acceptance bar incorporates the AuspexAI **Research Ethics Policy** (a top-level governance document at the same level as this one): experiments must demonstrate alignment with the Code of Conduct's design-intent constraint on agent behavior, appropriate containment for research that legitimately requires producing harmful agent behavior as a research subject, and ethics review proportional to dual-use risk.

Approvers do not have platform-code commit authority by virtue of being Approvers.

### 3.5 Maintainer

A person with commit authority on AuspexAI Platform repositories and authority over project direction in lazy consensus with co-Maintainers.

Capabilities:

- Merge PRs to platform repos (after appropriate review)
- Cut releases and sign release artifacts
- Drive security incident response
- Set and revise project policy in consensus with co-Maintainers
- Represent AuspexAI in public statements and external engagements

Maintainers hold themselves to the same DCO and CoC standards as any contributor. Maintainers are subject to recusal under §6 and to CoC accountability per the escalation pathway in `CODE_OF_CONDUCT.md`.

**Release authority on fleet-installed repositories** *(added 2026-06-12)*

Publishing a GitHub release on a fleet-installed repository (today: `auspexai/worker`) is not merely a packaging act — it is the **fleet-announce trigger**: the coordinator's release webhook records the published release, and every volunteer's worker surfaces the update banner on its next heartbeat. The two-human trust gate (principles doc §9 #46) is: *a Maintainer gates what is offered to the fleet; each volunteer gates what is installed on their machine*. Release publication is therefore the first half of that gate, and the authority to exercise it is enumerated here rather than implied by repository access:

- **Only Maintainers may publish releases (including prereleases) on fleet-installed repositories.** GitHub write access alone does not confer this authority. A contributor granted repo write access who is not a Maintainer must not publish releases, and as the Maintainer team grows beyond one, repository protections (release/tag restrictions, environments) should enforce this boundary mechanically, not just socially.
- The coordinator's webhook filters (unknown repos and prereleases ignored; malformed `Fulfils:` linkage skipped-and-audited) are defense-in-depth, **not** the authority boundary — this paragraph is the authority boundary.
- Every fleet-announced release is audited coordinator-side, and the release description is the volunteer-facing announcement; Maintainers write it for that audience.

**Release authority on researcher-installed repositories** *(added 2026-06-12)*

The same boundary applies, for the same reason, to repositories whose packages researchers install on their own machines (today: `auspexai/tenant-sdk` and `auspexai/researcher-dashboard`). These packages are the researcher's **verifier** — the tooling that checks signatures, attestations, and transparency-log inclusion — so their supply chain is part of the trust claim itself: a compromised verifier defeats every guarantee it exists to check.

- **Only Maintainers may publish releases on researcher-installed repositories.** This includes package-index publication (PyPI): publishing rides the tagged-release workflow via trusted publishing (GitHub OIDC, no long-lived credentials), so the release gate and the index gate are the same gate.
- Release artifacts are Sigstore-signed by the release workflow, matching the fleet-installed posture; the PyPI publisher configuration and any signing identities are Maintainer-administered.

There is no standing oversight role (Steering committee or Ombud) at Phase 0–3. CoC accountability when a Maintainer is the subject of a complaint is handled via the escalation pathway in `CODE_OF_CONDUCT.md`. Cross-role tiebreaking is deferred per §4.4. Whether to add an Ombud, Steering committee, or equivalent oversight role is reassessed at Phase 4 entry per §10.

---

## 4. Decision rules

### 4.1 Default: lazy consensus

For most decisions affecting platform code, documentation, processes, or project direction:

1. A proposal is made publicly (PR, issue, or RFC document)
2. Comment period:
   - **Routine changes** (code, docs, minor process): minimum 7 days
   - **Cross-cutting policy** (anything affecting multiple roles or the network publicly): minimum 14 days
   - **Substantive amendments to this document, the license, or role taxonomy**: minimum 30 days (see §8)
3. If no objections from Maintainers or directly-affected role-holders surface during the comment period, the proposal is accepted
4. If objections surface, discussion continues; if consensus cannot be reached, escalation per §4.2, §4.3, or §4.4 as appropriate

### 4.2 Maintainer-team vote (platform decisions)

For decisions affecting platform direction, releases, or other platform-side matters where lazy consensus does not hold:

1. The Maintainer team takes a vote; simple majority decides
2. Tied votes are decided by the longest-tenured Maintainer
3. Vote rationale is documented publicly in the relevant issue or PR
4. During sole-Maintainer phase, the sole Maintainer is the decider (see §4.5 bootstrap addendum)

### 4.3 Approver-pool decisions (tenant matters)

The Approver pool decides:

- Tenant application approvals and rejections
- The tenant-acceptance bar (the published criteria)
- Whether to revoke a previously-approved tenant for cause

Approver-pool decisions use simple majority of the pool. Abstentions count as not-voting (not as no). Tied votes are held pending further deliberation; if a tie persists across two consecutive Approver-pool sessions on the same matter, the question is escalated per the cross-role tiebreaking mechanism in §4.4 (which until defined as Phase 2/3+ amendment per §8.2 means the tied decision is held pending consensus or a broader public RFC initiated by the Maintainer).

### 4.4 Cross-role tiebreaking — deferred

Cross-role tiebreaking between the Maintainer team and the Approver pool is structurally not a problem until both bodies exist concurrently with ≥2 members each. This will not happen before Phase 2/3.

When that configuration emerges, a tiebreaking mechanism will be defined via substantive amendment per §8.2. Until then, this section is reserved.

If a cross-cutting disagreement arises during bootstrap (sole-Maintainer + interim-Approver-Maintainer-with-COI-disclosure), §4.5 governs.

### 4.5 Bootstrap addendum (Phase 0–1)

While the project is sole-Maintainer and pre-Approver-pool:

- Single Maintainer is the decider on platform matters; lazy consensus still applies (community comment, response to objections)
- For decisions normally subject to Approver-pool review (tenant policy, etc.), the Maintainer documents the decision publicly with rationale and explicit "made under bootstrap addendum" disclosure; these decisions are reviewable when the permanent Approver pool seats
- For CoC complaints concerning the Maintainer: the escalation pathway in `CODE_OF_CONDUCT.md` applies. The Maintainer publicly recuses from handling the complaint and the reporter selects an escalation option from those defined there
- The bootstrap addendum is in force until *either* (a) co-Maintainer is seated AND Approver pool reaches floor of 2, *or* (b) Phase 2 entry, whichever first. Once retired, this addendum no longer applies and standard rules govern

---

## 5. Recruitment

Recruitment for governance roles is bootstrap-with-active-pursuit, not deferred. Governance maturity is itself a project feature, not a "wait until we're bigger" concern.

### 5.1 Co-Maintainer

**Open recruitment when** *either*:

- Phase 1 enters its second month, **OR**
- Three or more external Platform Contributors have demonstrated sustained constructive engagement, defined as: contributors who, over a continuous 60-day period or longer, have each (a) made multiple substantive contributions (code or documentation PRs requiring design discussion or review iteration — not trivial fixes), (b) participated in code review of others' work or technical discussion on issues, and (c) shown alignment with project values through their conduct in those interactions. "Substantive" is judged by the Maintainer with rationale documented publicly when recruitment is opened.

**Target**: first additional Maintainer seated by end of Phase 1.

**Vetting**: candidate must (a) meet the sustained-engagement bar above, (b) have a clean CoC track record, (c) be willing to disclose conflicts of interest, (d) be endorsed by the existing Maintainer team. The candidate must explicitly accept the role's responsibilities and time expectations.

**Floor**: even if no candidate emerges by the target, the Maintainer documents the gap publicly each release cycle. **Vetting standards are not lowered to fill the seat.** Continued operation under sole-Maintainership is acceptable.

### 5.2 Approver pool

**Initiate recruitment when** *any of*:

- The first external tenant application is filed, **OR**
- 60 days before the targeted Phase 4 entry, **OR**
- (Future) a standing oversight role, if reinstated under §8.4, requests it

**Target**: 2–3 Approvers seated within 30 days of initiation.

**Vetting**: candidate must satisfy *one of*:

- Demonstrated research/tenant credibility (publications, prior research compute leadership, domain expertise verifiable to a sitting Approver or Maintainer), **or**
- 6+ months of sustained engagement with AuspexAI equivalent to the engagement bar in §5.1

Plus: clean CoC track record, willingness to disclose conflicts of interest.

**Bootstrap fallback**: if a tenant application arrives before the Approver pool is formed, the Maintainer acts solo as Approver with explicit "interim governance" disclosure in the public approval record. Each solo-approved decision is flagged in a public Provisional Approvals log; reviewable when the permanent pool seats. There is no hard clock that blocks new applications during the interim — the project does not stall on Approver-pool formation.

**Transparency obligation**: while the Approver pool is below floor of 2, the Maintainer publishes a quarterly public report covering the pool gap, recruitment status, and the Provisional Approvals log.

### 5.3 Standing oversight roles — explicitly not recruited at Phase 0–3

Steering committee, CoC Ombud, and other standing oversight roles are deliberately not present in this governance at Phase 0–3. Whether to add any of them is reassessed at Phase 4 entry per §10. If reassessment concludes one is needed, recruitment process and triggers will be defined at that time and added to this document via standard amendment per §8.2.

---

## 6. Conflict of interest

### 6.1 Disclosure expectations

All Maintainers and Approvers disclose conflicts of interest at the time of role acceptance and thereafter promptly upon any new conflict arising. Conflicts include:

- **Affiliations** (employer, academic institution, funding source) that could plausibly bias the role-holder's judgment on a specific decision
- **Financial interests** (investment, consulting, paid affiliation) related to a tenant, technology, or party affected by a decision
- **Personal relationships** that could plausibly bias judgment on a specific decision

Disclosure is public and recorded in the AuspexAI public log mapped under §7 — `DISCLOSURES.md` in the `auspexai/.github` repository, created when the first disclosure is needed.

### 6.2 Recusal

A role-holder recuses from a specific decision when the conflict is direct (e.g., an Approver evaluating an experiment from their own institution; a Maintainer who is also the author of a tenant under review). Recusal is mandatory in such cases. Recusal does not remove the role-holder from other unaffected decisions.

### 6.3 Maintainer-as-tenant-author case

A Maintainer who also authors tenant code — common during Phase 0–1 when the same person maintains the platform and authors the first tenant — must recuse from approving their own tenant. Until the Approver pool is formed with floor of 2 external members, the Maintainer's own tenant cannot proceed to public-network operation under sole-Maintainer approval; an external advisor (named in the public record for that approval) must concur for the approval to stand. The advisor is selected ad-hoc per case from senior unaffiliated OSS or research-community members; this is not a pre-named role. This advisor selection is for the tenant-ethics-and-approval review and is distinct from the CoC complaint advisor selection in `CODE_OF_CONDUCT.md` (which uses a separate three-option process driven by the reporter); a single individual could plausibly serve both functions in different cases, but the selection processes do not overlap.

---

## 7. Transparency and public record

Different transparency obligations live where they fit best — there is no single transparency log. The mapping below ties each obligation to its location.

**Static governance state — `auspexai/.github` repository:**

- This document (`GOVERNANCE.md`)
- Conflict-of-interest disclosures (cumulative, current-state) — `DISCLOSURES.md` (created when the first disclosure is needed)
- Provisional Approvals log during the bootstrap interim (§5.2) — `PROVISIONAL_APPROVALS.md` (created on first provisional approval; retired when the permanent Approver pool seats)
- Code of Conduct (`CODE_OF_CONDUCT.md`)
- Contribution guide (`CONTRIBUTING.md`)

**Project communication — `auspexai/website`:**

- Quarterly public reports during periods when the Approver pool is below floor (§5.2) — published as blog posts
- Major recruitment announcements (e.g., co-Maintainer recruitment opening per §5.1) — blog post or pinned issue, cross-linked
- Substantive amendment summaries (link to PR + brief explanation) — blog post

**In-context decision records — the PR or issue thread where the decision was made:**

- Maintainer-team vote rationale and outcomes (§4.2)
- Approver-pool decisions including approvals, rejections, and revocations — labeled and linkable; eventually surfaced via the website's public approvals listing
- Cross-role tiebreaking outcomes (when §4.4 mechanism is defined and used)
- Recruitment opening rationale when the Maintainer judges the §5.1 qualitative engagement bar has been met — published as the issue or pinned discussion that opens recruitment

**This document's revision history — §9 below.**

Some obligations only become active when their preconditions exist (no Approver-pool quarterly report is due before there is an Approver pool to be below floor of; no provisional approvals log exists before the first provisional approval). Files referenced above are created at the time the first record needs to be filed; their absence before that point is expected, not a transparency gap.

---

## 8. Amendment process

### 8.1 Routine amendments

Editorial changes (typos, link fixes, clarifications without semantic change) follow lazy consensus per §4.1 with a 7-day comment period.

### 8.2 Substantive amendments

Changes to roles, decision rules, recruitment triggers, COI rules, or transparency obligations require:

1. RFC published as a PR or issue with explicit "amends GOVERNANCE.md" label
2. 30-day public comment period
3. Maintainer-team consensus or vote per §4.2 to merge
4. New version recorded in §9 changelog

### 8.3 License change

Changing the project's license (currently AGPL-3.0) requires:

1. Public RFC with rationale
2. 30-day public comment period at minimum (longer if community engagement warrants)
3. Maintainer-team consensus
4. Consent from all contributors whose contributions are still in the codebase, *or* documented removal/rewriting of code from non-consenting contributors
5. Coordinated update to all repositories' LICENSE files

### 8.4 Adding standing oversight roles

If the Phase 4 reassessment per §10 concludes a Steering committee, CoC Ombud, or other standing oversight role is needed, the addition follows §8.2 substantive amendment process. The role's composition, recruitment, authority, and relationship to existing roles must be specified in the amendment before the role is formed or filled.

### 8.5 Mission-aligned constraint

Amendments that would conflict with the project's stated durable commitment (§2) — specifically, introducing a crypto-economy layer (cryptocurrency, blockchain incentive layer, on-chain payment, or speculative-token model) — are out of scope for amendment under this governance. Such proposals require either (a) a successful project fork under the AGPL license rather than change to AuspexAI itself, or (b) a renaming/relaunch as a different project. The crypto-economy stance is structurally protected from drift.

**All other operational-model questions** — including the volunteer-compensation model, project-level revenue paths (hosted service, enterprise support, custom integration, premium tooling, certification, grants, sponsorships), and the donate-only operating posture — are *normal* §8.2 amendments, not structurally protected. The maintainer cannot honestly commit on behalf of the project to forever-no-compensation or forever-no-revenue stances. Changes to these would follow the standard substantive-amendment process: RFC, deliberation period, public ratification, doc trail revision. The pre-2026-05-17 form of this section bundled three commitments into structural protection and overclaimed; the narrowing is recorded as a 2026-05-17 amendment.

---

## 9. Versioning and changelog

| Version | Date | Changes |
|---------|------|---------|
| v1 | 2026-04-27 | Initial. Role taxonomy (Volunteer / Platform Contributor / Researcher / Approver / Maintainer), decision rules, recruitment triggers, COI rules, transparency obligations, amendment process. License: AGPL-3.0; CoC: Contributor Covenant 2.1; Contribution: DCO. No standing oversight roles (Steering, Ombud) at Phase 0–3 — deferred to Phase 4 reassessment. CoC accountability when Maintainer is subject of a complaint handled via escalation pathway in `CODE_OF_CONDUCT.md`. |
| v1.1 | 2026-05-17 | **Monetization stance unbundled and narrowed.** §2 mission statement rewritten to separate one durable commitment (no crypto-economy layer) from operational-model statements that are not structurally protected (volunteer-compensation model, project-level revenue posture, donate-only operating posture). §8.5 narrowed: structurally-protected items reduced from three ("introducing token incentives, monetizing volunteer compute, or removing the donate-only constraint") to one ("introducing a crypto-economy layer"); all other operational-model questions become normal §8.2 amendments. Pre-revision form overclaimed by committing the project to forever-no-compensation and forever-no-revenue stances the maintainer cannot honestly make on the project's behalf. Source of truth for the revision: `Documentation/AuspexAI/v0.1.0/AuspexAI_Principles_and_Scope.md` §4 #7 (revised 2026-05-17). |
| v1.2 | 2026-06-12 | **§3.5: release authority on fleet-installed repositories made explicit.** The coordinator's direct-announce release webhook made "GitHub write access to `auspexai/worker`" operationally equal to fleet-announce authority; codified as a Maintainer-only capability before a second Maintainer joins (external design-review recommendation, 2026-06-11). Webhook filters named as defense-in-depth, not the authority boundary. Routine amendment (§8.1). |
| v1.3 | 2026-06-12 | **§3.5: release authority extended to researcher-installed repositories** (`tenant-sdk`, `researcher-dashboard`), including PyPI publication via trusted publishing on the same tagged-release gate. Rationale: these packages are the researcher's verifier — their supply chain is part of the trust claim. Closes the symmetry gap left by v1.2. Maintainer-ratified in session 2026-06-12; routine amendment (§8.1). |
| v1.4 | 2026-06-19 | **New §11 "Contestation & appeals."** Writes the recourse path for adverse actions (declined application, demotion, suspension, worker quarantine — including the new automated quarantine on a signed containment violation) into the public record, with the companion tenant-facing section in `TENANT_TERMS.md` §8. Encodes the standing principles (lost only by provable misconduct; divergence never adverse; a human hears every contest) and the single-Maintainer→expansion authority staging. Closes the recourse gap opened by the containment-violation auto-quarantine; the one documentation prerequisite for tenant-#2. Maintainer-ratified 2026-06-19; substantive amendment (§8.2). |
| v1.5 | 2026-06-24 | **§11 contestable-actions list extended** to name **research-standing demotion** and **own-code ("BYOT") revocation** — companion to `RESEARCH_ETHICS_POLICY.md` RFC 0002 (§5.4 Researcher alignment). Both were already contestable under §11's general principle (every adverse action taken for a recorded reason is appealable, and standing is reduced only by provable misconduct, never by divergence); they are named explicitly to match the section's enumerated pattern and remove ambiguity. Clarifying, routine amendment (§8.1). |
| v1.6 | 2026-06-26 | **Folds RFC 0001 (§6.7) + RFC 0002 (§5.4, §11) into `main`, + documentation currency (A9 internal audit — AUD-14/15/16/17; routine §8.1, no substantive change).** §11: the contestable-actions list also names **denying or revoking a profile certification (de-certification)** — a live reason-gated action (`RESEARCH_ETHICS_POLICY.md` §6.7) that belongs in the enumeration (AUD-14); `TENANT_TERMS.md` §8 and `ONBOARDING.md` §3 synced to the full list. §1: `SECURITY.md` and the Volunteer Terms (`BETA_TERMS.md`) de-staled from "forthcoming" and linked. §3.1: worker trust ladder corrected **T0–T4 → T0–T3** (T4 removed pre-launch; deployed `TrustTier` tops at T3). Filed the §5.2/§7 public **`PROVISIONAL_APPROVALS.md`** log (the first provisional approval — `vigiles-lab`, solo under the vacant Approver pool, 2026-06-12 — had occurred). The §6.7 / §5.4 / §11 content was adopted-in-force on the coordinator earlier (per the RFC entries above) and is now published to `main` per the A9 audit's recommendation to define §6.7 publicly before external Tier-1 onboarding. |

---

## 10. Revisiting this document

The complete governance shape is reassessed at Phase 4 entry. Specific questions for that reassessment:

- Is a standing oversight role (Steering committee, CoC Ombud, or other) load-bearing now? If so, what is its composition, recruitment, and authority?
- Is the Approver pool size adequate for tenant volume?
- Are there role gaps that have emerged from operational experience?
- Are the recruitment triggers calibrated correctly given actual community growth shape?
- Is the bootstrap addendum still retired, or have circumstances regressed?
- Are the transparency obligations sufficient, or do additional public-record requirements need to be codified?
- Has the cross-role tiebreaking mechanism (§4.4) been needed, and if so, has the deferred-amendment approach worked?

The reassessment is conducted publicly; outcome is recorded as a substantive amendment per §8.2.

## 11. Contestation & appeals

Adverse actions — declining an application, demoting or suspending an account, quarantining a worker (including the automated quarantine on a signed containment violation), demoting a Researcher's research-standing, revoking own-code ("BYOT") eligibility, or denying or revoking a starter-profile certification (de-certification, §6.7 of the Research Ethics Policy) — are contestable. Every such action is taken for a **recorded reason** (§7) and may be appealed.

**Standard of action.** Standing is reduced only by **provable misconduct** — a bad signature, a served-weights or containment mismatch, or a Terms breach — never by statistical dissent or divergence (the firewall-#1 corollary). An automated quarantine is a *provisional, reversible* response to a signed admission of non-compliance; it is always subject to human review on request.

**Who hears it.** While the project has a single Maintainer, the Maintainer hears contests, recusing under §6.2/§6.3 where conflicted, and answers on the record. On reaching a second Maintainer or a staffed Approver pool (§4, §5), a contest is heard by someone **other than the original decision-maker**: account and worker actions by a non-deciding Maintainer, tenant-application declines by the Approver pool. The §4.4 cross-role tiebreak resolves on the same trigger.

**Process.** File via **governance@auspexai.network**, citing the recorded reason; you receive a written upholding-or-reversal decision (target: five business days, Phase-1 best-effort), itself entered into the public record (§7). There is no retaliation for a good-faith contest.

---

## Current officeholders (Phase 0 snapshot)

| Role | Currently filled by | Notes |
|------|--------------------|-------|
| Sole Maintainer | Jason Gagne (`@jasongagne-git`) | Phase 0 bootstrap. Co-Maintainer recruitment per §5.1 active from Phase 1 month 2. |
| Approver pool | (vacant) | Bootstrap fallback per §5.2 in force. Recruitment per §5.2 triggers. |
| Platform Contributors | (none external; Maintainer is the sole platform contributor) | Recruitment passively open; explicit recruitment per §5.1 trigger. |
| Researchers | Implicit (first tenant authored by Maintainer) | Bootstrap-addendum recusal per §6.3 applies for Maintainer-authored tenants. |
| Volunteers | Open to the public (open beta, from 2026-06) | Volunteer onboarding is live — enroll via [getworker.auspexai.network](https://getworker.auspexai.network). Anonymous (T0) or identity-bound (T1+). |

This section is updated as roles are filled.
