# Closed-Beta Terms of Participation

**Status:** Phase 2 closed beta (2026-05 onwards). Bridge document — the full counsel-reviewed Terms of Participation lands with Phase 3 public launch per [`AuspexAI_Principles_and_Scope.md` §7](https://github.com/auspexai) (private; public summary in the org profile).

**Audience:** People personally invited to run the AuspexAI worker on their machines during the closed beta. If you're reading this without an invitation, the worker `.deb` is publicly downloadable but the coordinator currently runs as a small lab deployment intended for invited participants — please email **contact@auspexai.network** before installing.

---

## 1. What you're agreeing to

By installing and running [`auspexai-worker`](https://github.com/auspexai/worker) configured to talk to `https://coord.auspexai.network`, you agree to:

- Run alpha software on your hardware at your own discretion
- Donate compute time to AI safety research conducted by tenants on the AuspexAI network
- Have your contribution receipts signed and recorded in a public transparency log ([Sigstore Rekor](https://search.sigstore.dev))

You are NOT agreeing to:

- Pay anything (the network is donation-funded; no fees of any kind)
- Receive payment (Phase 2 is volunteer-only; future compensation policy is governed by [GOVERNANCE.md §8.2](GOVERNANCE.md), not by this document)
- Cede ownership of your hardware or your data
- Surrender any IP rights to work products generated on your machine

## 2. What the worker does on your machine

When you run `systemctl --user enable --now auspexai-worker.service`, the worker:

- Generates a fresh Ed25519 keypair, stored in your OS keyring (or, on headless hosts, an encrypted file under `~/.local/share/auspexai-worker/`)
- Sends a heartbeat (~once a minute) to `coord.auspexai.network`
- Polls for assignments
- When it gets one, spawns a sandboxed subprocess (`bubblewrap` on Linux) to run the assigned compute
- Submits the signed result back

The worker does NOT:

- Run any code outside the sandboxed subprocess
- Send your files, your other processes' data, your environment, or anything else from your machine to AuspexAI
- Phone home with telemetry beyond what's needed for the protocol (heartbeat + capabilities you've declared in config)

See [`Documentation/AuspexAI/v0.1.0/worker_daemon_design.md`](https://github.com/auspexai/worker) §3 (process model) and §5.17 (sandbox) for the architecture.

## 3. What we store about you

**T0 anonymous (no `auspexai-worker login` step):**
- A coordinator-generated `worker_id` (no personal data)
- Your Ed25519 public key (random bytes; not personally identifying)
- Coarse capabilities you declared (OS family, CPU count, RAM, GPU presence — not a hardware fingerprint)
- Timestamps of heartbeats and assignments

**T1+ identity-bound (you ran `auspexai-worker login`):**
- All of the above
- Your GitHub user ID (an opaque integer like `246774008`) — NOT your email, GitHub handle, or real name
- The above identifier is what gets shown in receipts

We do NOT store: your email, IP address (beyond what rate-limiting needs in transit; not retained), real name, payment info, hardware serial numbers, OS-keystore-derived attributes, contents of files on your machine, network traffic patterns.

## 4. Receipts are permanent

Every work unit your worker completes produces a **signed receipt**. The receipt is:

- Stored in the coordinator's database
- Available to the tenant that submitted the work
- Verifiable by anyone (the coordinator's signing key is publicly attested in [`security/AUTHORIZED_SIGNERS.md`](security/AUTHORIZED_SIGNERS.md))
- Anchored in the [Sigstore Rekor](https://search.sigstore.dev) public transparency log: each experiment's **result-set attestation** (the Merkle root that commits to the receipts in that run) is recorded in Rekor, and the coordinator's signing key is itself Rekor-attested — so a receipt's provenance is **publicly and permanently verifiable**. (Receipts inherit Rekor immutability through that one per-experiment anchor rather than each receipt being its own Rekor entry.)

**What this means for withdrawal:** when you run `auspexai-worker withdraw`, your local state is purged AND the coordinator severs the binding between your worker pubkey and your account. The **committed record** of receipts you previously earned (via the per-experiment attestation in the transparency log) is permanent. The **attribution** to your identity is removed.

If you can't accept this permanence — and there are legitimate reasons not to: research-record permanence is incompatible with some volunteer postures — please **do not enroll above T0 anonymous**. T0 receipts don't bind to any account, so there's no person-identifying linkage to sever.

## 5. Your right to withdraw, at any time, no questions

```bash
auspexai-worker withdraw
```

This is a deliberate command (you type the word `withdraw` to confirm). It:

1. Calls the coordinator's retire endpoint — your worker stops receiving assignments immediately
2. Deletes your local state (SQLite DB, keystore entry, audit log)
3. Severs the binding between your worker pubkey and your account (if T1+)

Phase 2 makes a **best-effort** commitment: withdrawal requests are typically processed in under a minute. Phase 3 adds the **GDPR-aligned 30-day SLA** to a more formal Terms of Participation document, but for closed beta the volume is too small for formal SLAs to be meaningful.

You can also email **contact@auspexai.network** to request withdrawal if you can't run the CLI for any reason.

## 6. No warranty

The auspexai-worker is alpha software. It may:

- Crash, leak memory, peg your CPU, run a fan loudly
- Get out of sync with the coordinator and need re-enrollment
- Generate runtime errors that need attention
- Refuse work for reasons you didn't expect

We will do our best to fix bugs quickly, but we cannot promise:

- That the network or the coordinator stays available
- That the software causes no harm to your machine (we believe the sandbox is strong but it's untested at scale)
- That the receipts you earn will retain their current shape or schema across future versions (we'll honor any recognized schema version forever; rotation policy in §5.16)

You run the worker **at your own risk**. If something breaks badly, please report it at https://github.com/auspexai/worker/issues. We'll listen.

## 7. Conduct on the network

You are bound by the project [Code of Conduct](CODE_OF_CONDUCT.md). Specifically:

- Don't run malicious workers (intentional bad results, attempts to influence quorum, attempts to compromise the coordinator)
- Don't attempt to deanonymize other volunteers from the receipt data
- Don't share invitation access without checking with the Maintainer first (the closed beta has a tight feedback loop; uninvited participants can disrupt that)

Network-level abuse is handled through technical enforcement (revocation, ban, trust-tier demotion), not Code-of-Conduct processes. CoC processes apply to community interaction (Discussions, issues, PRs, etc.).

## 8. What changes in Phase 3

The full Terms of Participation arrives at public launch (Phase 3) and includes:

- Counsel-reviewed legal language
- Formal 30-day GDPR-aligned withdrawal SLA
- Explicit data-controller / data-processor designation under GDPR + CCPA
- Privacy policy as a separate companion document
- More structured grievance + escalation procedure

This beta document is intentionally short and plain-language. If anything here surprises you, please email **contact@auspexai.network** before installing — we'd rather have a five-minute conversation now than a misunderstood install later.

## 9. Cross-references

- [`GOVERNANCE.md`](GOVERNANCE.md) — roles, decision rules, conflict of interest
- [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) — community conduct standards
- [`RESEARCH_ETHICS_POLICY.md`](RESEARCH_ETHICS_POLICY.md) — what research can run on the network
- [`security/AUTHORIZED_SIGNERS.md`](security/AUTHORIZED_SIGNERS.md) — the signing roster you'd consult to verify receipts you've earned
- [`TENANT_TERMS.md`](TENANT_TERMS.md) — the tenant/researcher-facing counterpart to this document (results custody, retention)
- [`auspexai/worker`](https://github.com/auspexai/worker) — the worker source code (AGPL-3.0)

---

**Document state:** v0.1, ratified 2026-05-23 for the Phase 2 closed beta. Changes follow the substantial-architectural-change RFC procedure in [GOVERNANCE.md](GOVERNANCE.md). The Phase 3 full Terms of Participation supersedes this document when it lands.
