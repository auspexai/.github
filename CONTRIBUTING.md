# Contributing to AuspexAI

Thanks for your interest. AuspexAI is a volunteer-driven, open-source distributed compute network for AI safety research. This document covers how to contribute, what's expected, and what to expect from us.

## Phase 0 reality

AuspexAI is in **Phase 0 — Foundation** as of 2026-04-27. There is not yet a platform codebase to contribute to; code begins in Phase 1. What you *can* do today:

- Read [`GOVERNANCE.md`](GOVERNANCE.md) and the public planning artifacts as they appear, and open issues with feedback or questions
- Watch the organization for activity as Phase 1 begins
- If you have specific expertise (distributed systems, sandbox security, OAuth device flow, OS-keystore integration, build signing, AI safety research methodology), drop a note via [`contact@auspexai.network`](mailto:contact@auspexai.network) — we'd like to know you're around as Phase 1 ramps up

When platform code begins shipping, this document will evolve to cover the real PR flow. The contribution model below (DCO, license, CoC) is fixed and applies from day one.

## Before contributing

1. Read the [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md). Participation in AuspexAI community spaces requires acceptance of the Code.
2. For non-trivial changes (anything beyond typo fixes, doc clarifications, or small bug fixes), open an issue first to discuss the approach. This avoids wasted work on changes that might not align with project direction.
3. Check existing issues and PRs to avoid duplicating work.

## Contribution paths

AuspexAI has multiple contribution paths with different processes, IP postures, and review workflows. Identify which path your contribution fits before reading further — they look quite different in practice.

### Path 1: Platform contribution

**What it is**: code, documentation, or other work submitted to AuspexAI infrastructure repositories — `auspexai/platform`, `auspexai/worker`, `auspexai/tenant-sdk`, `auspexai/.github`, `auspexai/website`, and similar.

**Examples**: a bug fix in the Worker, a new feature in the Tenant SDK, a documentation improvement, a build-pipeline enhancement.

**License**: AGPL-3.0, inbound equals outbound. You retain copyright on your contribution; you license it to the project and to all downstream users under AGPL-3.0 by submitting it.

**DCO**: required (see "Developer Certificate of Origin" section below).

**Process**: standard pull request workflow (see below). For substantial architectural contributions, an RFC is required *before* code is written — see "Substantial architectural contributions" under the Pull request workflow.

**Direction control**: the Maintainer team retains discretion to accept, modify, refactor, or replace contributions, and to set the project roadmap. Copyright retention is independent of direction authority — see "Copyright versus direction" below.

### Path 2: Tenant authoring (the Researcher path)

**What it is**: authoring a tenant project module — research code that runs on the AuspexAI network as an experiment. This is the primary contribution path for **Researchers** (`GOVERNANCE.md` §3.3) and is structurally different from contributing to the platform itself.

**Examples**: a new research project module consuming the Tenant SDK; an extension to an existing approved tenant.

**Where the code lives**: typically in a repository owned by the Researcher's team — which may or may not be inside the `auspexai` GitHub organization. AuspexAI does not require tenants to be hosted in the AuspexAI org; tenants can live in any repo as long as the experiment artifacts (job manifests, project module, result schema) integrate with the Tenant SDK contract. The Sentinel tenant — AuspexAI's first tenant — is a worked example.

**License**: the Researcher's choice. The Tenant SDK boundary is designed so that consuming the SDK does *not* AGPL-infect tenant code; Researchers may license their tenant project modules under a permissive, copyleft, or proprietary license, as the science requires. (Caveat: this SDK-boundary design is being validated in Phase 1 with counsel review; until validation, treat the AGPL non-infection of tenant code as a working assumption rather than a final guarantee. See plan §5.2.)

**DCO**: applies to code changes within tenant repos that are inside the `auspexai` GitHub organization. For tenant repos outside the org, the tenant's own contribution policy applies — AuspexAI does not impose DCO on out-of-org tenant repos.

**Process**:

1. Author or update tenant code in the appropriate repo (yours or an in-org tenant repo if you're invited to contribute)
2. Submit a tenant application to the Approver pool — process forthcoming as the Approver pool forms
3. Approver pool reviews against the published acceptance bar (which incorporates the Research Ethics Policy — `RESEARCH_ETHICS_POLICY.md`, forthcoming Phase 0 deliverable)
4. On approval, your tenant becomes eligible to run on the public network

**Direction control**: Researchers retain control over their tenant's scientific direction; AuspexAI does not edit tenant project code. The Approver pool has approval, modification-request, and revocation authority over *what runs on the network*, not authorship authority over what tenant code says. A Researcher whose tenant is approved retains responsibility for the tenant's adherence to project values and ongoing review.

**Most "Researcher contribution" work is in this path, not Path 1.** A Researcher who additionally lands a PR fixing a bug in the SDK is wearing the Platform Contributor hat for that PR; that contribution is governed by Path 1's IP framework, not Path 2's.

### Path 3: Issue reporting, discussion, and feedback

**What it is**: bug reports, design feedback, questions, feature requests, governance comments, RFC discussion participation.

**License**: this is information, not code; GitHub's Terms of Service apply.

**DCO**: not applicable.

**Process**: open an issue or a GitHub Discussion in the relevant repo, or comment on existing threads.

This path is open and welcomed at every phase, including Phase 0 when there is little code to contribute to. Substantive issue participation is one of the things that builds the qualitative engagement bar referenced in `GOVERNANCE.md` §5.1 for co-Maintainer recruitment.

### Path 4: Volunteer participation

**What it is**: running an AuspexAI Worker on your machine to donate compute. Not a "contribution" in the intellectual property sense; it's network participation.

This path is out of scope for this document. See the Volunteer Terms of Participation (forthcoming, Phase 2) when it is published.

## License and IP framework

### License of platform contributions

The AuspexAI platform is licensed under **AGPL-3.0**. By submitting a contribution to a platform repository, you agree to license your contribution under AGPL-3.0 to the project. This is the standard "inbound equals outbound" principle applied to copyleft OSS projects — by contributing to an AGPL-3.0 codebase, you license your contribution under the same AGPL-3.0 terms the project ships under (consistent with FSF guidance for GPL-family licenses; AGPL §10 cascades the license terms to all downstream recipients). No separate Contributor License Agreement (CLA) is required.

Practical implications for you as a Platform Contributor:

- You retain copyright on your contribution
- Your contribution is licensed under AGPL-3.0 to the project and all downstream users
- The project (and downstream users) cannot relicense your contribution under different terms without your consent or removal of your code
- AGPL-3.0 is a strong copyleft license: derivatives, including network-served services, must also be released under AGPL-3.0 to their users

If you do not have authority to license code under AGPL-3.0 (for example, if your employer claims rights to your work), do not submit it as your own. Resolve the authority question first.

### License of tenant code

Tenant project modules are not subject to AGPL-3.0 by virtue of consuming the AGPL-3.0 Tenant SDK; the SDK boundary is designed to prevent license infection. Researchers may license tenant code under whatever terms their science requires — permissive, copyleft, or proprietary — subject to the SDK-boundary validation noted in Path 2 above.

If a tenant lives inside the `auspexai` GitHub organization, the tenant's choice of license is recorded in that repo. If a tenant lives outside the org, AuspexAI takes no position on the tenant's license; the Approver pool reviews the experiment design and ethics, not the tenant's IP arrangements.

### Copyright versus direction

AuspexAI's contribution model **distributes copyright** (each contributor retains rights to their contribution under AGPL-3.0) but **concentrates direction** (the Maintainer team retains authority to accept, modify, replace, or reject contributions and to set the project roadmap).

What this means in practice:

- **You own your code.** You can use it elsewhere under AGPL-3.0. You cannot be forced to relicense without your consent. You have public attribution in commit history.
- **You don't own a piece of project direction.** Accepting your contribution today does not give you a veto over future changes; the Maintainer team may refactor, replace, or remove your code as the framework evolves. Substantial architectural changes require Maintainer-team approval before code is accepted (see RFC subsection below) — this is so the Maintainer team can evaluate fit with project direction before significant effort is invested on either side.
- **Project direction is held by the Maintainer team via governance**, not by individual contributors regardless of contribution size. See `GOVERNANCE.md` §3.5 and §4.

This is the standard open-source distinction between copyright and direction. It's not a "weak welcome" — it's an explicit statement of where authority sits so neither side has misplaced expectations. AuspexAI is a directed open-source project, not a democratically-steered one. Contributions are welcomed and credited; project direction stays with Maintainers.

## Developer Certificate of Origin (DCO)

AuspexAI uses the [Developer Certificate of Origin 1.1](https://developercertificate.org/) instead of a CLA. Every commit must include a `Signed-off-by` line attesting to your right to contribute under the project's license.

### What signing off certifies

By signing off on a commit, you are certifying that:

```
(a) The contribution was created in whole or in part by me and I have
    the right to submit it under the open source license indicated in
    the file; or

(b) The contribution is based upon previous work that, to the best of
    my knowledge, is licensed under an appropriate open source license
    and I have the right under that license to submit that work with
    modifications, whether created in whole or in part by me, under the
    same open source license (unless I am permitted to submit under a
    different license), as indicated in the file; or

(c) The contribution was provided directly to me by some other person
    who certified (a), (b) or (c) and I have not modified it.

(d) I understand and agree that this project and the contribution are
    public and that a record of the contribution (including all
    personal information I submit with it) is maintained indefinitely
    and may be redistributed consistent with this project and the open
    source license(s) involved.
```

### How to sign off

Use the `-s` (or `--signoff`) flag with `git commit`:

```
git commit -s -m "Your commit message"
```

This appends a `Signed-off-by:` line to your commit message using the name and email from your local Git configuration:

```
Signed-off-by: Your Name <your.email@example.com>
```

### Configure once

Set your Git identity if you haven't already:

```
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Use a real name and an email address you control. The `Signed-off-by` line becomes part of the public commit history.

### Forgot to sign off

If you forgot the sign-off on your most recent commit, amend it:

```
git commit --amend --signoff
```

For a series of commits, rebase and add sign-off:

```
git rebase --signoff <base-branch>
```

(If your branch has been pushed and others may have pulled it, prefer additional commits over rewriting shared history.)

### Enforcement

Until automated DCO bot enforcement is configured (planned for Phase 1), DCO sign-offs are checked manually during review. PRs without sign-offs cannot be merged; you'll be asked to add them.

## Pull request workflow

1. Fork the relevant repository
2. Create a feature branch from `main` with a descriptive name (e.g., `feature/worker-keystore-fallback`, `fix/coordinator-restart-race`, `docs/governance-typo`)
3. Make your changes; sign off each commit per DCO above
4. Push your branch to your fork
5. Open a pull request against the upstream `main` branch
6. Fill in the PR description: what changed, why, how it was tested (if applicable), any related issues
7. Respond to review feedback; iterate as needed
8. A Maintainer merges once approved (during sole-Maintainer phase, the sole Maintainer reviews; once co-Maintainers exist, two-reviewer policy applies)

### Substantial architectural contributions: RFC required

Substantial architectural contributions to platform repositories require an RFC (Request for Comments) and Maintainer-team approval **before** code is written. "Substantial architectural" means changes that:

- Introduce new platform-level abstractions (new top-level modules, new SDK contracts, new cross-cutting infrastructure)
- Make breaking changes to existing public interfaces (Tenant SDK, Worker protocol, Coordinator API)
- Change persistence layers, security or trust models, or deployment topologies
- Add new external dependencies that materially affect supply chain, build pipeline, or licensing posture
- Are likely to require sustained review effort (typical heuristic: more than ~500 lines of substantive non-generated code change, or any change that crosses module boundaries in non-obvious ways)

The RFC process:

1. Open an RFC issue or RFC PR in the relevant repository (template forthcoming) describing the problem, motivation, proposed approach, alternatives considered, and any cross-cutting implications
2. The Maintainer team responds with one of: agree to proceed (with any noted constraints), request changes to the proposal, defer (with reasoning), or decline (with reasoning)
3. Once the Maintainer team agrees to proceed, the implementation PR follows the standard workflow

The point of the RFC step is to avoid wasted work — for both you and reviewers. If your proposal doesn't fit project direction, finding out before a week of implementation is better for everyone. Conversely, an RFC that the Maintainer team agrees to is a soft commitment to engage seriously with the implementation when it arrives.

Bug fixes, small features, documentation improvements, refactors that don't change interfaces, and similar focused contributions do not require an RFC. The threshold is judgment-based; if you're unsure, open an issue or RFC anyway — it's cheaper than a stalled PR.

### Review expectations

- During sole-Maintainer phase (currently): one reviewer (the Maintainer) approves before merge
- Once co-Maintainers are seated: two reviewers approve before merge, including at least one who is not the PR author
- Reviewers focus on correctness, alignment with project direction, security implications, and code quality
- Reviewers are responsible for explaining the rationale for any change requests; "I don't like it" is not a review comment
- Reviewers may request scope changes (split a PR, defer part of it, drop a piece) consistent with project direction; this is a normal part of review, not a rejection of the contribution

### Squash, rebase, or merge

The project's preference for merge style will be set per-repository as code begins to land. Until then: prefer rebasing your branch onto current `main` before requesting review, and avoid merge commits in your feature branch.

## Reporting bugs

Open an issue in the relevant repository. Include:

- What you expected to happen
- What actually happened
- Steps to reproduce (if applicable)
- Environment details (OS, version, configuration) when relevant

## Reporting security issues

**Do not file public issues for security vulnerabilities.** Use one of:

- Email [`security@auspexai.network`](mailto:security@auspexai.network)
- GitHub's private security advisory feature on the relevant repository (forthcoming as repos come online)

A formal `SECURITY.md` describing the disclosure process and response timelines will be published in Phase 1.

## Reporting Code of Conduct violations

See [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) for the reporting procedure, response timelines, and escalation pathway when a Maintainer is the subject of a complaint.

## Communication

- **General questions and discussion**: open a GitHub Discussion in the relevant repository (forthcoming as repos come online), or email [`contact@auspexai.network`](mailto:contact@auspexai.network)
- **Bug reports**: GitHub issue
- **Security issues**: see "Reporting security issues" above
- **CoC issues**: see "Reporting Code of Conduct violations" above
- **Research/tenant inquiries**: email [`research@auspexai.network`](mailto:research@auspexai.network)

## Recognition

Contributors are recognized in commit history and (forthcoming) release notes. AuspexAI does not offer financial compensation, tokens, or other transferable benefits for contributions — see [`GOVERNANCE.md`](GOVERNANCE.md) §2 for the donate-only stance and why it's structurally protected.

---

This document evolves as the project grows. Substantive changes follow the amendment process in [`GOVERNANCE.md`](GOVERNANCE.md) §8.
