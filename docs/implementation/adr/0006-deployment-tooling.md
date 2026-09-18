# 0006. Deployment tooling: Dokku

Date: 2026-09-17
Status: Accepted

## Context

The maintainer has professional experience with Laravel Forge for
provisioning/deploying servers, and some personal experience with Dokku.
Forge is a paid, third-party SaaS that manages the server on the user's
behalf. This project prioritizes being cost-free to operate and consistent
with its self-hosted, FOSS-aligned values (see
[ADR 0004](0004-self-hosted-backend.md)).

## Decision

Deploy using **Dokku**, a free, open-source, self-hosted PaaS, running on the
project's own Hetzner server (see
[ADR 0005](0005-hosting-provider-and-server-sizing.md)).

## Alternatives considered

- **Laravel Forge** — familiar and well-integrated with Laravel, but a
  recurring paid third-party dependency for a project meant to be
  cost-free and self-contained.
- **Raw Docker Compose with manual orchestration** — no deployment
  convenience layer (no git-push/image-based deploy flow, no built-in
  Let's Encrypt/proxy management); Dokku provides this for free while
  still being just Docker underneath.

## Consequences

- Deploys become either a `git push` or, more precisely for this project, a
  `dokku git:from-image` call fed a pre-built container image — see
  [ADR 0007](0007-cicd-build-strategy.md) for why the build step itself
  doesn't happen on the Dokku box.
- Dokku's plugin ecosystem (Postgres, Redis, Let's Encrypt) covers what this
  project needs without additional third-party services.
- Forge remains the maintainer's tool of choice for other, unrelated
  professional work — this decision applies to this project specifically.
