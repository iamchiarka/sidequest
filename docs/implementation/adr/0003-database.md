# 0003. Database: PostgreSQL

Date: 2026-09-17
Status: Accepted

## Context

The maintainer has experience across MySQL/MariaDB, PostgreSQL, and MS SQL
Server, so the choice wasn't constrained by unfamiliarity. It needed to fit
Laravel/Eloquent well and support the kind of flexible, evolving data model a
mood-tracking/notes/gamification app is likely to need.

## Decision

Use **PostgreSQL** as the primary database.

## Alternatives considered

- **MySQL/MariaDB** — well-supported by Laravel, but weaker JSON column and
  full-text search support than Postgres for this app's more flexible data
  needs (e.g. mood entries or skill metadata that may not fit a rigid
  schema).
- **MS SQL Server** — no particular advantage here and would be an unusual,
  harder-to-self-host choice for this kind of project.

## Consequences

- Postgres is also the engine most self-hosted FOSS backend platforms
  (Supabase, Appwrite) converge on, which keeps a future migration path open
  if the backend framework ever changes.
- JSONB columns and full-text search are available if the data model needs
  them later, without a database migration.
