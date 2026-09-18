# Open questions / not yet settled

Things that came up during planning and implementation but are either
genuinely undecided or simply not done yet. Kept separate from the ADRs so
the ADRs only record decisions that were actually made.

## Undecided

- **GitHub vs. Codeberg for source hosting** — see
  [ADR 0013](adr/0013-source-hosting.md). Current tooling (CI workflow)
  assumes GitHub; this hasn't been deliberately weighed against Codeberg's
  better values alignment.
- **Laravel Boost.** The default `laravel/laravel` template ships
  `api/AGENTS.md` and `api/CLAUDE.md` files with generic bootstrap
  instructions for AI coding agents, including a step that installs PHP
  directly onto the host machine via a curl script — which conflicts with
  this project's deliberate Docker-only, no-host-PHP setup (see
  [ADR 0015](adr/0015-local-dev-environment.md)). Those instructions also
  offer to install `laravel/boost` (Laravel's official package providing
  AI-agent-friendly tooling — an MCP-style bridge into the app's routes,
  models, and schema), which could be genuinely useful given this project's
  agentic-coding-learning goal. Neither installing Boost nor rewriting
  `AGENTS.md`/`CLAUDE.md` to reflect the actual Docker-based workflow has
  been done yet.

## Not yet done

- **The Hetzner server itself hasn't been provisioned.** ADR 0005 records
  the decision (provider, tier, location); the actual server doesn't exist
  yet, so ADR 0006 and 0007's deployment/CI steps are implemented but
  untested end-to-end.
- **The Dokku app and its deploy secrets aren't set up.** The GitHub Actions
  deploy step (ADR 0007) is present in the workflow but commented out,
  pending `DOKKU_HOST`, `DOKKU_SSH_KEY`, and `DOKKU_APP_NAME` secrets and an
  actual Dokku app to deploy to.
- **The Flutter SDK isn't installed** on the development machine yet, so
  `mobile/` only contains scaffold instructions, not an actual Flutter
  project (see [ADR 0001](adr/0001-mobile-framework.md)).
