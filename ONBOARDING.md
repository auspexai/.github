# Becoming an AuspexAI researcher

AuspexAI runs your deterministic experiments across a volunteer compute
network — replicated, signed, attested, and anchored in a public transparency
log, so your results are citable and re-verifiable forever. This page is the
front door: from nothing to a bound research tenant in four steps.

## 1. Install

```bash
curl -fsSL https://raw.githubusercontent.com/auspexai/researcher-dashboard/main/install.sh | bash
```

One command: dedicated environment, both packages from PyPI, commands on
your PATH (it asks before touching your shell config). Prefer to manage it
yourself? `pipx install auspexai-researcher-dashboard` works identically.

That's the SDK (keys, submission, the evidence verify chain) plus your local
dashboard. Releases are Sigstore-signed; verify-before-install instructions
live on each [release page](https://github.com/auspexai/researcher-dashboard/releases)
and the signer roster is [AUTHORIZED_SIGNERS.md](security/AUTHORIZED_SIGNERS.md).

## 2. Apply

```bash
auspexai-tenant apply
```

This signs you in with GitHub (device flow — you'll confirm a code in your
browser), creates your research keypair locally if you don't have one, and
submits your application (name, affiliation, a short research summary)
signed by that key. Your private key never leaves your machine; the
application itself proves you hold it. No key material to copy-paste.

Track it any time:

```bash
auspexai-tenant apply --status
```

## 3. Review

A Maintainer reviews your application against the
[Research Ethics Policy](RESEARCH_ETHICS_POLICY.md) — what the network runs
and what it refuses is public, as is [how decisions are made](GOVERNANCE.md).
Approval creates your tenant and binds your key in one step; a decline always
carries a written reason. Applications are not public records, but every
decision is audited.

## 4. Confirm and run

```bash
auspexai-dashboard serve
```

The Overview page shows your confirmed identity once your key is bound
(tenant id + key match, checked live against the coordinator). From there:
your first experiment is a signed manifest + work units submitted via the
SDK — the dashboard's integrity panel shows the attestation, transparency-log
anchor, and custody state as results arrive, and `experiment export --verify`
hands you the evidence bundle you can re-verify offline, forever.

Questions, or stuck at any step? Open an issue on any AuspexAI repo; a Maintainer will find it.
