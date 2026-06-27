# Provisional Approvals Log

This is the public **Provisional Approvals log** that [`GOVERNANCE.md`](GOVERNANCE.md) §5.2
and §7 require: while the Approver pool is below its floor of 2, the Maintainer may act
**solo as Approver** under the §5.2 bootstrap fallback, with explicit *"interim
governance"* disclosure. Each such solo-approved decision is recorded here, is reviewable
when the permanent Approver pool seats, and this file is **retired** at that point.

**Interim-governance disclosure.** During the bootstrap interim, tenant-application
approvals are made by the single Maintainer without a seated Approver pool. These
decisions are provisional and are subject to review by the permanent pool once it seats
(§5.2). The Maintainer's standing quarterly transparency report (§5.2) covers the pool
gap, recruitment status, and this log.

## Approvals

| Application | Tenant | Decision | Decided by | Date (UTC) | Notes |
|---|---|---|---|---|---|
| `tapp-LQePDero` | `vigiles-lab` | approved | Maintainer (solo, interim) | 2026-06-12 | Closed-beta **dogfooding** tenant operated by the Maintainer's own account; low-risk reference work (the AuspexAI-native Vigiles rebuild). Approved under the §5.2 bootstrap fallback with a vacant Approver pool. *Resolution-reason field was left empty in the application record — a process gap in the early dogfooding flow; the interim-governance basis is as stated here.* |

**Not listed (not Approver decisions):** the `demo-tenant`, `vigiles`, and `proof-newcomer`
tenants were **operator-provisioned** directly (maintainer-only `POST /tenants`) for
closed-beta dogfooding and proving the on-ramp — they did not go through a
tenant-application → Approver decision, so they are not provisional *approvals* and are
not logged here.

---

_Maintained per `GOVERNANCE.md` §5.2 / §7. First record filed 2026-06-26 (the
`tapp-LQePDero` approval predates this file; the audit that surfaced the missing log is
recorded in the AuspexAI roadmap's A9 internal-audit section, finding AUD-17)._
