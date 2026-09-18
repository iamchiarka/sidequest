# 0011. Analytics and crash reporting: none for v1, self-hosted if added later

Date: 2026-09-17
Status: Accepted

## Context

Standard mobile analytics/crash tools (Google Analytics, Firebase
Crashlytics) route user behavior and crash data through third parties,
which conflicts with this project's data-sovereignty stance (see
[ADR 0004](0004-self-hosted-backend.md)) and adds GDPR compliance surface
area for data the app doesn't strictly need to collect.

## Decision

Ship v1 with **no telemetry at all**. If analytics or crash reporting is
needed later, self-host **Matomo** (analytics) and/or **GlitchTip** (a
self-hostable, Sentry-compatible crash/error reporting tool).

## Alternatives considered

- **Google Analytics / Firebase Crashlytics** — rejected outright for the
  third-party data-routing reasons above.
- **Self-hosted Matomo/GlitchTip from day one** — deferred rather than
  rejected: for v1, relying on direct user feedback is simpler, has zero
  infrastructure cost, and avoids collecting data before it's clear it's
  needed.

## Consequences

- Fewer GDPR compliance obligations for v1 (no consent flow needed for
  telemetry that doesn't exist).
- Debugging production issues without crash reporting will rely more on
  Laravel Pulse/Telescope (backend-side, see
  [ADR 0008](0008-observability.md)) and direct user reports for the
  mobile client, at least until GlitchTip is deemed worth adding.
