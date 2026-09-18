# 0012. Repository visibility and license: public, AGPL-3.0

Date: 2026-09-17
Status: Accepted

## Context

The maintainer values open source and wants the project to be as
NGO-friendly as possible — including letting an organization fork and
self-host their own instance for their own community. At the same time,
there was a concern that a public repository might expose security
weaknesses in the application code.

The resolution rests on a standard security principle (Kerckhoffs's
principle): an application's security should never depend on its source
code being secret. Secrets (DB credentials, API keys, signing keys) must
never live in the repository regardless of its visibility — they belong in
environment variables injected at deploy time. Given that discipline, a
public repository's main realistic risk is *accidental* secret leakage
(e.g. committing a `.env` file), not "hackers reading the code."

## Decision

The repository is **public**, licensed under **AGPL-3.0**.

## Alternatives considered

- **Private repository** — would raise the bar slightly against casual
  drive-by code scanning, but provides no real protection against an
  attacker probing the live, running API (which is equally visible whether
  or not the source is public), and directly conflicts with the
  NGO-forkability goal.
- **MIT license** — fully permissive, easiest for anyone to adopt, but would
  let a company fork the project closed-source with no obligation to share
  improvements — undermines the "stays open" goal.
- **EUPL-1.2** — an EU-native copyleft license with similar intent to AGPL,
  explicitly designed for European public-sector/NGO adoption. A reasonable
  alternative; AGPL-3.0 was chosen instead as the more widely recognized
  license with the same practical effect.

## Consequences

- Anyone running a modified or hosted version of this app — including an
  NGO running their own instance — is required by the license to share
  their source changes too, preventing a closed-source SaaS fork.
- Secret hygiene becomes a hard requirement, not a nice-to-have: `.env`
  files are gitignored, only `.env.example` templates are committed, and
  GitHub's secret scanning + push protection (automatic on public repos)
  provides a safety net against accidental leaks. A `gitleaks` pre-commit
  hook is planned as an additional, host-independent layer (see
  [ADR 0013](0013-source-hosting.md)).
- If any single piece of the codebase ever turns out to be genuinely
  sensitive (judged unlikely for this app), the plan is to carve *that*
  piece out into a small private package rather than making the whole repo
  private — the same "opportunistic exception" pattern used for
  [ADR 0014](0014-rust-as-opportunistic-satellite.md).
