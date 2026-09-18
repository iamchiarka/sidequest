# 0004. Self-hosted backend, not a third-party BaaS

Date: 2026-09-17
Status: Accepted

## Context

Mood-tracking and personal-notes data is sensitive, health-adjacent data —
under GDPR this falls into special-category data (Art. 9) when it reveals
health information. The project targets EU/German users primarily, so GDPR
compliance is a first-class constraint. Separately, the maintainer is
personally cautious about outsourcing user data to third-party services, and
wants the project to favor free-software and NGO-friendly values.

An earlier direction considered Supabase or Firebase as a managed
backend-as-a-service. Both were reconsidered once the values/data-sovereignty
constraint was made explicit.

## Decision

**Self-host** the backend and its database rather than using a third-party
BaaS. Data stays on infrastructure the maintainer controls end-to-end.

## Alternatives considered

- **Supabase (managed cloud)** — fast to start with, generous free tier, but
  means a third party (Supabase, a US/Singapore-based company) processes and
  stores all user data, which conflicts with the data-sovereignty goal.
- **Firebase (managed cloud)** — same concern, and materially more expensive
  at scale than Supabase for a read-heavy mobile app.
- **Self-hosted Supabase / PocketBase / Appwrite** — considered as the
  self-hosting mechanism rather than the framework; superseded by the choice
  to self-host Laravel directly (see [ADR 0002](0002-backend-framework.md)),
  since it avoids needing to operate a second unfamiliar system.

## Consequences

- The maintainer takes on full operational responsibility: patching,
  backups, uptime, security updates. Self-hosting on Hetzner
  ([ADR 0005](0005-hosting-provider-and-server-sizing.md)) and deploying via
  Dokku ([ADR 0006](0006-deployment-tooling.md)) is the concrete
  implementation of this decision.
- No vendor lock-in and no per-seat/per-row third-party billing risk as the
  user base grows.
- This decision cascades into several others: DNS/proxy choice
  ([ADR 0009](0009-dns-and-domain.md)), push notifications
  ([ADR 0010](0010-push-notifications.md)), and analytics/crash reporting
  ([ADR 0011](0011-analytics-and-crash-reporting.md)) were all evaluated
  against the same "does a third party get to see user data or traffic"
  question.
