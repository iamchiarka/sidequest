# 0002. Backend framework: Laravel + Octane

Date: 2026-09-17
Status: Accepted

## Context

The mobile app needs an API backend for auth, notes, mood entries, and the
gamification/XP logic. The project's data-sovereignty stance (see
[ADR 0004](0004-self-hosted-backend.md)) already ruled out third-party
backend-as-a-service platforms, so a self-hosted backend needed to be chosen.

The maintainer has strong, current Laravel expertise, including production
experience with Laravel Pulse (performance monitoring) and Telescope
(request/query/job debugging). Taking on Flutter/Dart as a new language
already uses this project's "learn something new" budget (see
[ADR 0001](0001-mobile-framework.md)); adding a second unfamiliar backend
stack at the same time would put shipping speed and maintainability at risk
for a solo, free-time project.

## Decision

Build the backend API in **Laravel, running under Octane (Swoole)**.

## Alternatives considered

- **PocketBase** — extremely lightweight (single Go binary, SQLite-backed),
  the least ops overhead of any option, and open source. Rejected as the
  *default* choice because it would mean learning a second new stack
  alongside Flutter; kept in mind as a lower-maintenance fallback if Laravel
  ever proves too heavy operationally.
- **Self-hosted Supabase** (Postgres + PostgREST + GoTrue) — more powerful
  (proper relational DB, row-level security) but a heavier Docker Compose
  stack to operate solo.
- **Rust (Axum/Actix-web) from day one** — see
  [ADR 0014](0014-rust-as-opportunistic-satellite.md) for the full reasoning;
  rejected as the *starting* backend because it would front-load significant
  plumbing work (auth, jobs, observability) in an unfamiliar language before
  the product idea is even validated.
- **Laravel without Octane** — plain PHP-FPM rebuilds the framework on every
  request, which is the main source of the "PHP feels slow" perception.
  Octane keeps the app booted in memory between requests, closing most of
  that gap for typical CRUD workloads.

## Consequences

- Backend observability is strong from day one via Pulse (production) and
  Telescope (local/staging) — see
  [ADR 0008](0008-observability.md).
- Auth, background jobs (reminders, XP/streak calculation), and scheduling
  come from Laravel's built-in tooling (Sanctum, queues, scheduler) rather
  than needing to be built from scratch.
- Octane introduces a new failure class to be careful about: shared state
  between requests (static properties, singleton services holding
  request-specific data) can leak across requests since the app stays booted
  in memory. This needs to be kept in mind when writing application code.
- Running Octane requires the `swoole` and `pcntl` PHP extensions, which are
  not present in a base PHP image and had to be added explicitly (see
  [ADR 0015](0015-local-dev-environment.md)).
