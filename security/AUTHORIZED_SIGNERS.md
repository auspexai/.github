# AUTHORIZED_SIGNERS

The public roster of identities and keys authorized to sign on behalf of AuspexAI. Verifiers consult this file to ground their trust decisions for release artifacts and contribution receipts. See [`AuspexAI Principles & Scope` §5.16](https://github.com/auspexai) for the full signing-infrastructure design.

**Last updated:** 2026-05-23 (M7b — file inaugurated as part of the worker daemon M7 packaging milestone).

## Two-tier trust model

AuspexAI has two signing concerns that share one trust anchor — the **Maintainer's GitHub OIDC identity**:

| Path | Cadence | Mechanism |
|---|---|---|
| **Release-artifact signing** | per release tag | Maintainer (or GitHub Actions on behalf of the Maintainer's identity via `id-token: write`) runs `cosign sign-blob`; browser/OIDC obtains a Fulcio cert; signature + Rekor entry per artifact. No long-lived key. |
| **Contribution-receipt signing** | per receipt (hot path) | Coordinator-resident long-lived Ed25519 key, authorized by a one-time Fulcio attestation from the Maintainer's keyless identity. **Annual rotation.** Old keys remain valid for verifying historical receipts forever. |

## Active Maintainer roster

Identities currently authorized to issue Sigstore attestations on behalf of AuspexAI. Sourced from `auspexai/.github/GOVERNANCE.md` §Maintainer roster.

| GitHub identity | Active from | Active to | Notes |
|---|---|---|---|
| `jasongagne-git` | 2026-04-26 | *current* | Sole Maintainer. Issued first AUTHORIZED_SIGNERS roster 2026-05-23. |

Verifiers MUST treat any Sigstore attestation that asserts an identity not listed (or no longer active at the attestation's `IntegratedTime` in Rekor) as untrusted, regardless of whether the underlying cryptographic chain validates.

## Coordinator receipt-signing keys

Year-by-year roster of coordinator-resident Ed25519 keys authorized to sign contribution receipts. Each row corresponds to one Fulcio attestation in Rekor binding the listed public key to AuspexAI for the listed year.

| Year | Public key (Ed25519, hex) | Attestation Rekor index | Status | Maintainer who attested |
|---|---|---|---|---|
| 2026 | *(pending §5.16 ceremony — no coordinator-side key has yet been attested)* | — | not-yet-issued | — |

**Status legend:**
- `active` — currently signing receipts; valid for issuance and verification.
- `retired` — superseded by a newer key; valid for verifying historical receipts; not used for new issuance.
- `compromised` — flagged via incident response; receipts signed during the compromise window carry a verification-UI warning per §5.16.
- `not-yet-issued` — slot reserved; signing ceremony pending.

## Release-signing scope

The Maintainer's GitHub OIDC identity (listed in the *Active Maintainer roster* above) is authorized to sign release artifacts for the following repositories. Each row's "via" column declares whether signing is interactive (Maintainer at a terminal) or automated (GitHub Actions workflow with `permissions: id-token: write`, signing via the Maintainer's repo-scoped OIDC identity).

| Repository | Artifact patterns | Signing via | First signed release |
|---|---|---|---|
| `auspexai/worker` | `auspexai-worker_*.deb`, `auspexai-worker-*.tar.gz`, `auspexai_worker-*.whl` | GitHub Actions (Sigstore-keyless via OIDC) | *(pending — M7c)* |
| `auspexai/platform` | `auspexai-coordinator-*.tar.gz`, `auspexai_coordinator-*.whl` | GitHub Actions (Sigstore-keyless via OIDC) | *(future)* |
| `auspexai/tenant-sdk` | `auspexai_tenant_sdk-*.whl`, `auspexai-tenant-sdk-*.tar.gz` | GitHub Actions (Sigstore-keyless via OIDC) | *(future)* |

Verifiers can confirm a release artifact came from AuspexAI with:

```bash
cosign verify-blob \
  --certificate-identity-regexp='^https://github\.com/auspexai/.+/\.github/workflows/.+@.+$' \
  --certificate-oidc-issuer='https://token.actions.githubusercontent.com' \
  --signature <artifact>.sig \
  --certificate <artifact>.cert \
  <artifact>
```

The `--certificate-identity-regexp` MUST match a workflow path inside the `auspexai` GitHub org. Verifications that succeed against an identity outside the org indicate a mis-signed artifact; do NOT trust.

## Verification path for a contribution receipt

```
1. Parse the receipt (in-toto v1 Statement, predicate type
   https://auspexai.network/receipt/v0; COSE-signed body).

2. Verify the COSE signature against the public key embedded in
   the envelope (COORD_RECEIPT_PUB_v<N>).

3. Query Rekor for the Fulcio attestation that authorized
   COORD_RECEIPT_PUB_v<N>. The attestation chain reveals the
   Maintainer's GitHub OIDC identity and the year of attestation.

4. Confirm that identity (at that IntegratedTime) appears as an
   active Maintainer in this file, AND that the public key matches
   the row in "Coordinator receipt-signing keys" for that year
   with status ∈ {active, retired}.

5. Receipt is trusted iff all four steps pass. If the matching
   row has status=compromised, render the receipt with a clear
   "signed during a compromise window" warning per §5.16.
```

## Rotation policy

- **Maintainer identity**: no fixed rotation. Identity changes only on compromise or Maintainer-roster changes. Removed identities remain valid for verifying historical attestations forever; new attestations from a removed identity are untrusted.
- **Coordinator receipt-signing key**: **annual**. Each January the Maintainer generates a new keypair on the coordinator host, signs a fresh Fulcio attestation, and records the rotation here. The previous year's key transitions to `retired` status (valid for verifying past receipts; no new issuance).
- **Volunteer worker keypairs**: unaffected by either rotation. Worker keys are local-only OS-keystore entries per §5.11; they never appear in this file.

## Compromise response

If a Maintainer GitHub identity is compromised:
1. Publish a revocation attestation via Sigstore.
2. Mark the affected attestation window here.
3. Recover the GitHub account via GitHub's standard recovery process.
4. There is no further platform-side cryptographic remediation — the entire trust chain anchors at the Maintainer's OIDC identity.

If a coordinator receipt-signing key is compromised:
1. Cut a new coordinator key (regenerate on the coordinator host).
2. Maintainer signs a new Fulcio attestation for the replacement key.
3. Mark the compromised key's row with `status=compromised` and note the discovery date.
4. Old receipts signed during the compromise window stay in the transparency log; downstream verifiers render the "signed during a compromise window" warning per §5.16. Deleting them would break the verifiability property and other workers' replication anchoring.

## Updating this file

Substantial changes (adding/removing a Maintainer identity, rotating the coordinator key, marking a key compromised) follow the GOVERNANCE.md substantial-architectural-change RFC procedure. Routine annual rotation can be a fast-path PR by the current Maintainer; the rotation event itself (new public key, attestation Rekor index) carries the audit trail in Rekor regardless.

For the cryptographic mechanics and design rationale behind this roster, see `AuspexAI_Principles_and_Scope.md` §5.16.
