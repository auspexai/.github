# Open-Beta Terms of Participation — Tenants & Researchers

**Status:** Phase 2 open beta — open to public participation since 2026-06 (the closed beta ran from 2026-05). Bridge document — the full counsel-reviewed Terms of Participation lands with Phase 3 public launch per [`AuspexAI_Principles_and_Scope.md` §7](https://github.com/auspexai) (private; public summary in the org profile). The volunteer-facing counterpart is [`BETA_TERMS.md`](BETA_TERMS.md).

**Audience:** Researchers and organizations running experiments on the AuspexAI network as **tenants**. Onboarding is open: connect via [getresearcher.auspexai.network](https://getresearcher.auspexai.network) to bind your identity and run a **certified starter** experiment. Running your **own custom experiments** (bring-your-own-tenant) is gated on research standing and Approver review per [`GOVERNANCE.md`](GOVERNANCE.md) §5.4. The first tenant is the Vigiles research project. Questions? Email **research@auspexai.network**.

Throughout, "you" means the tenant and the researchers acting on its behalf; "we" / "Auspex" means the AuspexAI network operator.

---

## 1. What you're agreeing to

By submitting experiments to the AuspexAI coordinator as an accepted tenant, you agree to:

- Run only research that is permitted under the [Research Ethics Policy](RESEARCH_ETHICS_POLICY.md) — including its §4 prohibition on organizing experiments around tradeable or financialized receipts
- Take **custody of your computed results when you collect them** (§3), and retain your own authoritative copy
- Have a permanent, signed **proof-of-transfer** recorded each time you collect results (§3)
- Have the **receipts** your experiments generate signed and recorded in a public transparency log, permanently (§5)

You are NOT agreeing to:

- Pay anything (the network is donation-funded; there are no tenant fees in the open beta)
- Surrender ownership of your results, your manifests, or your IP — the science is yours
- Let other tenants see your private experiment data (results and per-experiment activity are tenant-scoped)
- Cede control over which volunteers may see your identity (volunteer/worker identities are pseudonymous to you; you are pseudonymous to them)

### Tenancies and your account

Your tenant identity is permanent — you apply once, and every experiment you
run lives under it. A single GitHub account may hold **multiple tenancies**
for genuinely separate research programs; each application receives its own
full review. Your account is the single accountability root: account-level
enforcement (including suspension) reaches **all** tenancies linked to it.

## 2. Getting your results back

Your experiment produces two kinds of output, retrieved through the researcher dashboard, the `auspexai-tenant` SDK/CLI, or the coordinator API directly:

- **Receipts** — signed attestations that prove *that* a unit of work ran, who ran it (by pseudonymous worker key), and a hash anchor of the result. Receipts are the network's trust substrate; see §5.
- **Results** — your actual computed outputs: the **consensus** payload (the one quorum-agreed result per work unit — the science you want) and, optionally, the **raw replicas** (all N per-unit results, including any disagreeing ones, for variance/debugging).

Two distinct events matter for what follows:

- **Delivery** — viewing or fetching individual results (the results list/detail). This starts the short retention clock on the heavy *raw replica* copies (§4).
- **Collection** — pulling the full **export bundle** (the dashboard's *Download bundle* button, or `auspexai-tenant experiment export`). This is the event that transfers custody and issues the proof-of-transfer (§3). Merely browsing results is *not* collection.

## 3. Results custody — the coordinator is a courier, not a warehouse

The AuspexAI coordinator is built to **deliver** your results to you, not to be their long-term home. This is a deliberate posture, and it is enforced in the coordinator's code, not just stated here.

**When you collect (pull the export bundle):**

1. The coordinator assembles a self-contained, offline-verifiable **evidence bundle**: your consensus result payloads, their COSE-signed receipts, your signed experiment manifest, the work-unit inputs they were computed from, and (for a completed experiment) the Rekor-anchored result-set attestation — so the bundle verifies end-to-end, offline, with no dependency on the coordinator (`auspexai-tenant experiment export --verify` runs the full chain, including the per-result worker signatures).
2. It records a **permanent, signed proof-of-transfer** — a `result_transfers` custody record that binds: the public key that collected the results, the collection time, your experiment's manifest hash, a `result_set_root` hash computed over the delivered consensus results, and the receipt count. The record is signed with the network's Ed25519 receipt-signing key (publicly attested in [`security/AUTHORIZED_SIGNERS.md`](security/AUTHORIZED_SIGNERS.md)) and is returned to you inside the bundle, so the transfer is provable by both sides.
3. From that point, **custody of the result data — and responsibility for its storage, security, retention, and lawful handling — passes to you.** The authoritative copy of your results is the bundle you downloaded. Auspex keeps the small signed proof-of-transfer, **not** your result payloads, which become eligible for age-off (§4).

**Keep your downloaded bundle.** After collection we do not guarantee that the coordinator still holds your result payloads — by design, it may discard them once you hold the authoritative copy. The standing doctrine is **re-verify forever, never re-deliver**: the network permanently retains the hashes, receipts, and attestations needed to re-verify any bundle you hold (and the public Rekor log re-verifies the attestation even without us), but once payloads have aged off they are gone — collection is custody transfer, and the network is not an archive.

> **Beta-stage legal note.** This clause states the **operational** custody model the coordinator already enforces; it is **not** the final legal instrument. Formal data-controller / data-processor designation under GDPR and CCPA, and counsel-reviewed liability language, are part of the Phase-3 full Terms of Participation (§9). If the precise legal allocation of responsibility matters to your organization before Phase 3, email **contact@auspexai.network** — we'd rather have that conversation up front.

## 4. Retention and age-off — collection-anchored

Results are stored in three tiers, each with its own horizon. Age-off **never deletes a row** — it blanks the heavy payload bytes while preserving the row, the worker signature, the result hash, and the receipt linkage. An aged-off result still verifies its receipt and still proves the unit ran; only the bulky payload is gone.

| Tier | What it is | Retention |
|---|---|---|
| **Receipts** | signed proof-of-work attestations + hash anchors | **Permanent** — never aged off (§5) |
| **Consensus payload** | the one quorum-agreed result per unit (your science) | Kept until you collect; then per the experiment's consensus TTL (default keeps it) |
| **Raw replicas** | all N per-unit results, incl. disagreeing ones | Short — **30 days after delivery**; a **14-day grace** from completion if never fetched |

The governing principle is **collection-anchored**: once you collect the export bundle, the coordinator may age off **both** payload tiers, because you now hold the authoritative copy. Results you **never** collect are kept (after their grace window) rather than silently discarded — we do not throw away undelivered science. Per-experiment TTL overrides are available on request.

## 5. Receipts are permanent (and so is the proof-of-transfer)

Every quorum-accepted work unit produces a signed receipt, and each completed experiment produces a **result-set attestation** — a Merkle root committing to that run's receipts — that is recorded in the [Sigstore Rekor](https://search.sigstore.dev) public transparency log **permanently** (the coordinator's signing key is itself Rekor-attested). Receipts inherit that public-transparency immutability through the per-experiment attestation. The proof-of-transfer records from §3 are likewise permanent. This is what lets your experiment remain *provably-ran and provably-delivered* long after the payload bytes have aged off. If permanent, publicly-verifiable records of your experiments' execution are incompatible with your research posture, raise it before onboarding.

Worker identities in receipts are **pseudonymous** (a worker public key, and for identity-bound workers an opaque GitHub user ID — never an email or real name). You agree not to attempt to deanonymize volunteers from receipt data.

## 6. Retention-hold — the narrow exception

There is exactly one case in which Auspex retains your actual result **data** after it would otherwise age off: when Auspex is **legally compelled** to preserve it (e.g. litigation hold or a regulatory/legal-process obligation). In that case a maintainer may place a **retention-hold** on the affected experiment, which overrides age-off until released. Every retention-hold requires a recorded reason and is written to the audit log; it is an exceptional measure, not routine operation. Absent such compulsion, the courier model in §3–§4 is what applies.

## 7. No warranty

The AuspexAI network is alpha software run as a small lab deployment. We cannot promise that the coordinator stays available, that experiments complete on any schedule, that volunteer-computed results are correct (quorum/replication mitigate but do not eliminate bad results — that is your experiment design's responsibility), or that result-retrieval schemas won't change across versions. You run experiments on the network **at your own risk**. Report problems at https://github.com/auspexai or email **contact@auspexai.network**.

## 8. Contesting a decision

Most of what the network does to a result is mechanical and self-correcting: a result that fails an integrity check — a bad worker signature, a served-model-digest mismatch, or a sandbox-containment mismatch — is simply refused, and you fix it and resubmit. That is not a penalty and needs no appeal.

A few actions *do* affect your standing, and **you can contest any of them**:

- a **declined application**;
- an **account demotion or suspension**;
- a **worker quarantine** — including the *automatic* quarantine that follows a result signed as having run under weaker containment than the experiment required;
- a **research-standing demotion** or a **revocation of your own-code ("BYOT") eligibility** (your research-standing tier, and your eligibility to supply your own experiment code);
- a **denied or revoked starter-profile certification** (de-certification).

Two commitments come first:

- **You are never penalized for disagreeing.** If your result diverges from others', that costs you nothing — it is recorded as a research finding and earns standing equally. If you ever believe an action was taken because your work *diverged*, treat it as a bug and tell us; it is not a legitimate penalty.
- **Standing is lost only for provable misconduct** — a bad signature, a model- or containment-mismatch, or a breach of these Terms — and **a human reviews every contest.** The system never decides its own appeals; an automatic quarantine is a provisional, reversible response, always subject to human review on request.

To contest, email **governance@auspexai.network** and reference the action — its recorded reason is shown to you, so cite it. A person (recused if they have any conflict) reviews it and replies in writing, normally within five business days, either upholding the decision with its reason or reversing it. Quarantines and suspensions are reversible. Filing a good-faith contest is never itself grounds for any action against you.

## 9. What changes in Phase 3

The full Terms of Participation arrives at public launch (Phase 3), counsel-reviewed, and supersedes this bridge document. It will add: explicit GDPR/CCPA data-controller / data-processor designation for the custody model in §3, a companion privacy policy, formal liability language, and a structured grievance/escalation procedure that formalizes the beta contestation path in §8. This beta document is intentionally short and plain-language.

## 10. Cross-references

- [`BETA_TERMS.md`](BETA_TERMS.md) — the volunteer/worker-facing counterpart to this document
- [`RESEARCH_ETHICS_POLICY.md`](RESEARCH_ETHICS_POLICY.md) — what research may run on the network (binds tenants)
- [`GOVERNANCE.md`](GOVERNANCE.md) — roles, decision rules, the change procedure governing this document
- [`security/AUTHORIZED_SIGNERS.md`](security/AUTHORIZED_SIGNERS.md) — the signing roster you'd consult to verify receipts and proof-of-transfer records

---

**Document state:** v0.1 draft for the Phase 2 beta; updated 2026-06-27 when the beta opened to public participation. Changes follow the change procedure in [GOVERNANCE.md](GOVERNANCE.md). The Phase 3 full Terms of Participation supersedes this document when it lands.
