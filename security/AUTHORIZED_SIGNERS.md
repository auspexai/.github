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

Identities currently authorized to issue Sigstore attestations on behalf of AuspexAI. Sourced from `auspexai/.github/GOVERNANCE.md` §Maintainer roster. The `Sigstore OIDC SAN` column is what verifiers match against `--certificate-identity` when the Maintainer signs a manual attestation (e.g., a coordinator receipt-signing key); this is whatever email GitHub's OAuth returns for the Maintainer at sign-time. The `Sigstore OIDC issuer` is the OAuth issuer URL the Fulcio cert carries.

| GitHub identity | Sigstore OIDC SAN (manual attestations) | Sigstore OIDC issuer | Active from | Active to | Notes |
|---|---|---|---|---|---|
| `jasongagne-git` | `j2w5g6zt9r@privaterelay.appleid.com` | `https://github.com/login/oauth` | 2026-04-26 | *current* | Sole Maintainer. SAN is an Apple Private Relay forwarder — opaque by design (the real email is not exposed in the public Rekor log). Cryptographically equivalent to any other email SAN; binds to whoever controls that relay address (i.e., this GitHub account's Apple-Sign-In linkage). |

Verifiers MUST treat any Sigstore attestation that asserts an identity not listed (or no longer active at the attestation's `IntegratedTime` in Rekor) as untrusted, regardless of whether the underlying cryptographic chain validates.

**Release artifacts vs. manual attestations — different identity formats:** GitHub-Actions-signed releases (the worker `.deb`, etc.) carry workflow-URI SANs like `https://github.com/auspexai/worker/.github/workflows/release.yml@refs/tags/v0.1.1` — match those with `--certificate-identity-regexp` against the auspexai-org workflow pattern (see *Release-signing scope* below). Manual attestations (coordinator receipt-signing key, etc.) carry email-style SANs — match those with `--certificate-identity` against the row above.

## Coordinator receipt-signing keys

Year-by-year roster of coordinator-resident Ed25519 keys authorized to sign contribution receipts. Each row corresponds to one Fulcio attestation in Rekor binding the listed public key to AuspexAI for the listed year. The attestation document, public key, and Sigstore bundle are committed under `security/attestations/`.

| Year | Public key (Ed25519, hex) | Attestation files | Rekor logIndex | Status | Maintainer who attested |
|---|---|---|---|---|---|
| 2026 | `13c3b143c995764663e1016668cb7d8d24f4497fdc18d3f24b54a9a7529df453` | [coord-receipt-key-2026.json](attestations/coord-receipt-key-2026.json) · [.bundle.json](attestations/coord-receipt-key-2026.bundle.json) · [.pub.pem](attestations/coord-receipt-key-2026.pub.pem) | [1615064195](https://search.sigstore.dev/?logIndex=1615064195) | `active` | `jasongagne-git` (SAN: `j2w5g6zt9r@privaterelay.appleid.com`) |

**Status legend:**
- `active` — currently signing receipts; valid for issuance and verification.
- `retired` — superseded by a newer key; valid for verifying historical receipts; not used for new issuance.
- `compromised` — flagged via incident response; receipts signed during the compromise window carry a verification-UI warning per §5.16.
- `not-yet-issued` — slot reserved; signing ceremony pending.

**Verifying the 2026 attestation locally:**

```bash
# Clone or download the three files from security/attestations/ above, then:
cosign verify-blob \
  --bundle coord-receipt-key-2026.bundle.json \
  --certificate-identity 'j2w5g6zt9r@privaterelay.appleid.com' \
  --certificate-oidc-issuer 'https://github.com/login/oauth' \
  coord-receipt-key-2026.json
# expect: "Verified OK"
```

The verified attestation document is the JSON file that asserts "this Ed25519 public key is AuspexAI's coordinator receipt-signing key for 2026." Verifying it confirms the Maintainer's OIDC-bound identity signed that binding, and Rekor pinned the signature at the time recorded.

## Release-signing scope

The Maintainer's GitHub OIDC identity (listed in the *Active Maintainer roster* above) is authorized to sign release artifacts for the following repositories. Each row's "via" column declares whether signing is interactive (Maintainer at a terminal) or automated (GitHub Actions workflow with `permissions: id-token: write`, signing via the Maintainer's repo-scoped OIDC identity).

| Repository | Artifact patterns | Signing via | First signed release |
|---|---|---|---|
| `auspexai/worker` | `auspexai-worker_*.deb`, `auspexai-worker-*.tar.gz`, `auspexai_worker-*.whl` | GitHub Actions (Sigstore-keyless via OIDC) | *(pending — M7c)* |
| `auspexai/platform` | `auspexai-coordinator-*.tar.gz`, `auspexai_coordinator-*.whl` | GitHub Actions (Sigstore-keyless via OIDC) | *(future)* |
| `auspexai/tenant-sdk` | `auspexai_tenant_sdk-*.whl`, `auspexai-tenant-sdk-*.tar.gz` | GitHub Actions (Sigstore-keyless via OIDC) | *(future)* |

Verifiers can confirm a release artifact came from AuspexAI with one of:

```bash
# v0.1.x worker — separate .sig and .cert files (the deprecated cosign output form;
# release.yml will migrate to --bundle in a future version):
cosign verify-blob \
  --certificate-identity-regexp='^https://github\.com/auspexai/.+/\.github/workflows/.+@.+$' \
  --certificate-oidc-issuer='https://token.actions.githubusercontent.com' \
  --signature <artifact>.sig \
  --certificate <artifact>.cert \
  <artifact>

# Future versions — single .bundle.json:
cosign verify-blob \
  --certificate-identity-regexp='^https://github\.com/auspexai/.+/\.github/workflows/.+@.+$' \
  --certificate-oidc-issuer='https://token.actions.githubusercontent.com' \
  --bundle <artifact>.bundle.json \
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
