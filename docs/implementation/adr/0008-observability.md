# 0008. Observability: Beszel, Laravel Pulse, and Telescope (scoped carefully)

Date: 2026-09-17
Status: Accepted

## Context

Self-hosting (see [ADR 0004](0004-self-hosted-backend.md)) means the
maintainer is responsible for their own monitoring rather than relying on a
managed platform's dashboards. The maintainer already has production
experience with Laravel Pulse and Telescope, and some experience with
Beszel.

## Decision

- **Beszel** for server/infrastructure-level metrics (CPU/RAM/disk on the
  Hetzner VM).
- **Laravel Pulse** for application-level performance monitoring in
  production (slow queries, jobs, endpoints).
- **Laravel Telescope** for local/staging debugging only — request/query/job
  inspection.

## Alternatives considered

- **Third-party APM/monitoring SaaS** (e.g. Datadog, New Relic) — rejected
  for the same third-party-data-processing reasons as
  [ADR 0004](0004-self-hosted-backend.md); also unnecessary cost for a
  free hobby project.
- **Running Telescope in production** — rejected: Telescope logs a lot,
  including request payloads, which for this app could include mood/notes
  content. Running it in production would mean sensitive user data sitting
  in a debugging tool's storage, and Telescope's UI needs to be tightly
  access-gated if it's ever exposed at all.

## Consequences

- Production observability comes entirely from self-hosted, open-source
  tools — no user or request data leaves the maintainer's own
  infrastructure for monitoring purposes.
- A concrete rule to enforce in configuration: Telescope must be disabled or
  hard-gated behind authentication outside local/staging environments.
