# 0014. Rust: opportunistic satellite services, not the primary backend

Date: 2026-09-17
Status: Accepted

## Context

The maintainer has some Rust experience and a general sense that PHP/Node
backends don't feel as fast as Rust-based ones, even accounting for Laravel
Octane. This needed unpacking: for this app's workload — CRUD on notes/mood
entries, computing XP/streaks, serving a fun-fact list — requests are
I/O-bound (dominated by the Postgres round-trip and mobile network latency),
not by language-level request-handling overhead. Once Octane keeps the app
booted in memory, the PHP-vs-Rust difference for this workload is on the
order of single-digit milliseconds — not perceptible on a phone over LTE/WiFi.

Where Rust genuinely wins is **resource efficiency**: a Rust API uses a
fraction of the CPU/RAM of PHP-FPM or Octane under equivalent load, which
directly serves the project's cost and environmental-footprint goals (a
smaller server serves more users). That's a real argument for Rust, distinct
from "does it feel snappier."

## Decision

Start with Laravel+Octane for the whole backend (see
[ADR 0002](0002-backend-framework.md)). Carve out a **Rust (Axum) satellite
service** later, opportunistically, only for a specific piece that proves to
be a genuine hot path or resource hog under real usage — not as an upfront
architectural split.

## Alternatives considered

- **Rust as the backend from day one** — would give more Rust practice and
  better resource efficiency immediately, but means building auth,
  migrations, background jobs, and observability from scratch in an
  unfamiliar language before the product idea is even validated. Rejected
  as too much unfamiliar-stack risk for a solo, free-time project already
  spending its "new learning" budget on Flutter/Dart
  ([ADR 0001](0001-mobile-framework.md)).
- **Never using Rust on the backend** — would waste a real efficiency
  opportunity and the maintainer's stated interest in more Rust practice,
  once a genuine hot path is identified.

## Consequences

- No architectural commitment is made until real usage data identifies a
  genuine hot path (e.g. a realtime sync engine, or gamification
  calculations running across many users).
- This mirrors the "carve out an exception, don't restructure everything"
  pattern also used for handling a hypothetically sensitive code path in
  [ADR 0012](0012-repo-visibility-and-license.md).
