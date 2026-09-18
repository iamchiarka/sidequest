# 0007. CI/CD build strategy: build off-box, deploy a prebuilt image

Date: 2026-09-17
Status: Accepted

## Context

By default, Dokku builds the application on the same server it runs on:
`git push` triggers `composer install` / asset compilation / artisan caching
directly on the production box. On a small server (see
[ADR 0005](0005-hosting-provider-and-server-sizing.md)), this competes for
RAM with Postgres and the running Octane workers. Dokku's zero-downtime
deploy also briefly runs the old and new app containers simultaneously,
which can double the app's memory footprint for a few seconds right after a
build has already consumed most of the available headroom. This matches a
failure mode the maintainer has repeatedly hit on other web app deployments
("deploys quickly hit the RAM limit").

## Decision

Build the Docker image in **CI (GitHub Actions)**, push it to a container
registry (GitHub Container Registry), and deploy it to Dokku via
`dokku git:from-image` — so the production server only ever *runs* a
container, never *builds* one.

## Alternatives considered

- **Building on the Dokku box (the default)** — rejected as the direct cause
  of the RAM-pressure failure mode described above.
- **Manually resizing the server up during deploys, back down after** —
  works, but adds operational complexity for no benefit over just not
  building on the box at all.

## Consequences

- CI runners provide far more headroom for the build step (GitHub's standard
  Linux runner: 4 vCPU / 16GB RAM on public repos, 2 vCPU / 8GB on private
  repos) than the production server ever will.
- A swap file is still added on the production server regardless, as a
  safety net against the old/new-container memory overlap during Dokku's
  zero-downtime swap.
- This pattern is unaffected by whether the mobile app build (Flutter, a
  separate concern — see [ADR 0015](0015-local-dev-environment.md)) ever
  touches the same server, since it never does.
- The GitHub Actions workflow currently builds and pushes the image; the
  actual deploy step is present but commented out, pending the Hetzner
  server and Dokku app existing (see [open questions](../open-questions.md)).
