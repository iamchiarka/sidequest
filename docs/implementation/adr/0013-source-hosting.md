# 0013. Source hosting: GitHub (working assumption), Codeberg considered

Date: 2026-09-17
Status: Proposed — not finally settled

## Context

Given the project's public, FOSS/NGO-friendly, values-driven stance (see
[ADR 0012](0012-repo-visibility-and-license.md)), the choice of where the
source lives isn't just a technical question. **Codeberg** (a German
non-profit running Forgejo) is more directly aligned with those values than
GitHub, a large US company.

The genuine trade-off: GitHub's secret-scanning and push-protection tooling
(which automatically blocks a commit containing a recognizable secret
pattern before it lands) is more mature than Codeberg/Forgejo's equivalent —
and accidental secret leakage is the main realistic risk a public repo
carries (see [ADR 0012](0012-repo-visibility-and-license.md)).

## Decision (current working assumption)

The CI workflow scaffolded so far ([ADR 0007](0007-cicd-build-strategy.md))
targets **GitHub Actions** and **GitHub Container Registry**, so GitHub is
the working assumption. This has **not** been explicitly finalized against
Codeberg as a deliberate trade-off decision — see
[open questions](../open-questions.md).

## Alternatives considered

- **Codeberg** — better values alignment, weaker automated secret-scanning
  tooling. Could be mitigated with a `gitleaks` pre-commit hook.
- **Mirroring to both** — possible middle ground (GitHub for tooling,
  Codeberg for values-aligned discoverability), not yet evaluated in depth.

## Consequences

- Whichever host is used, a `gitleaks` pre-commit hook is planned regardless
  as a host-independent safety net.
- This decision should be revisited deliberately rather than left as an
  implicit default before the project's first real push to a remote.
