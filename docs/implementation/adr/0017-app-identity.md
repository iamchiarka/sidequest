# 0017. App identity: working name and bundle identifier

Date: 2026-09-17
Status: Accepted (name is a placeholder; identifier is not)

## Context

Two identifiers were needed before scaffolding could start: a project name
(used for the repo, Flutter package, and Laravel `APP_NAME`) and a
reverse-domain bundle identifier for the iOS bundle ID / Android
`applicationId` — the latter is painful to change once submitted to app
stores, so it needed a deliberate answer even at scaffolding time.

## Decision

- Working codename: **`sidequest`** — chosen as a placeholder that fits the
  RPG/gamification framing (turning tasks into side quests), trivial to
  rename later since nothing depends on it yet.
- Bundle identifier: **`de.sakurasi.sidequest`** — reusing the maintainer's
  existing personal domain (`sakurasi.de`) as the reverse-domain prefix.

## Alternatives considered

- **`com.example.sidequest` placeholder** — deferred the domain decision
  entirely; not chosen since the maintainer already had a domain available
  and reusing it avoids a harder rename later.

## Consequences

- The project name (`sidequest`) can be freely rebranded before any public
  launch — it appears in the repo path, `pubspec.yaml`, and `APP_NAME`, none
  of which are hard to change.
- The bundle identifier is treated as effectively final — changing it after
  a first app store submission would require a new app listing entirely, so
  any future rename should keep `de.sakurasi.sidequest` as the underlying
  identifier even if the user-facing name changes.
