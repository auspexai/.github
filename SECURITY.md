# AuspexAI Security Policy

**Status:** v1, 2026-05-25
**Applies to:** all repositories under the `github.com/auspexai` organization.

---

## 1. Reporting a vulnerability

Email **security@auspexai.network**. Do **not** open a public GitHub issue for security vulnerabilities.

Include as much of the following as you can:

- **Description** of the vulnerability and its potential impact
- **Reproduction steps** or a proof-of-concept
- **Affected component** (repository name, file, endpoint, version)
- **Severity estimate** (critical / high / medium / low), if you have one

---

## 2. Response timeline

| Stage | Target |
|-------|--------|
| Acknowledge receipt | 48 hours |
| Triage and severity assessment | 7 days |
| Fix — Critical | Best-effort ASAP |
| Fix — High | 30 days |
| Fix — Medium | 90 days |
| Fix — Low | Next release cycle |

These targets are best-effort commitments from a volunteer team. If we expect to exceed a target, we will communicate a revised timeline to the reporter.

---

## 3. Scope

### In scope

- **Coordinator** — `auspexai/platform`
- **Worker** — `auspexai/worker`
- **Operator Console** — `auspexai/operator-console`
- **Tenant SDK** — `auspexai/tenant-sdk`
- **Website** — `auspexai/website`
- **Signing infrastructure** — `AUTHORIZED_SIGNERS.md`, receipt signing, in-toto attestation

### Out of scope

- **Sentinel research codebase** (`github.com/jasongagne-git/sentinel*`) — a separate project with its own maintainer; report issues there directly
- **Third-party dependencies** — report upstream to the dependency maintainer; let us know if you believe AuspexAI's usage is especially exposed
- **Social engineering of maintainers** — not a software vulnerability
- **Denial of service against hosted infrastructure** — report to Cloudflare for sites behind their proxy; report to us only if the vulnerability is in our application code

---

## 4. Disclosure policy

AuspexAI follows **coordinated disclosure**:

1. The reporter notifies us privately via the process above.
2. We work with the reporter to understand and fix the issue.
3. We ask reporters to give us reasonable time to release a fix before any public disclosure.
4. Once a fix is available, we publish a security advisory on the affected repository.
5. We credit the reporter in the advisory unless they prefer to remain anonymous.

---

## 5. Bug bounty

AuspexAI is a volunteer-funded open-source project. We cannot offer financial rewards. We will credit reporters publicly (with their permission) and are grateful for every report that helps keep the network safe.
