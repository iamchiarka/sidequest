# SideQuest

A free, open-source companion app for people with ADHD/AuDHD — reminders,
mood tracking, quick notes, fun-fact breaks, and a skill/XP progression
system, built to be self-hostable so NGOs and communities can run their own
instance.

## Stack

| Layer | Choice |
|---|---|
| Mobile | Flutter/Dart (`de.sakurasi.sidequest`) |
| Backend | Laravel + Octane (Swoole), self-hosted |
| Database | PostgreSQL |
| Local dev | Docker Compose (API + Postgres + Redis); Flutter runs natively on the host |
| Deploy | Dokku on a dedicated Hetzner Cloud VM, images built in CI and deployed via `git:from-image` (no build step on the production box) |
| Infra monitoring | Beszel |
| App monitoring | Laravel Pulse (prod), Telescope (local/staging only) |
| Push | On-device local notifications; UnifiedPush (Android) / APNs (iOS) if server-triggered push is ever needed |
| License | AGPL-3.0 |

The reasoning behind every choice above — including alternatives considered
and trade-offs — is recorded in [docs/](docs/README.md), one decision per
file under [docs/implementation/adr/](docs/implementation/adr/). See
[docs/implementation/open-questions.md](docs/implementation/open-questions.md)
for what's still undecided or not yet done.

## Repo layout

```
api/      Laravel API, Octane-ready, runs entirely via Docker Compose
mobile/   Flutter app (not yet scaffolded — see mobile/README.md)
docs/     Product vision and implementation decision records (ADRs)
```

## Getting started

### API

Composer/PHP don't need to be installed on the host — everything runs
through Docker. The Laravel app is already scaffolded, so day to day this is
just:

```bash
cd api
docker compose up -d --build
```

First run only: generate an app key and run migrations.

```bash
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

Requires Docker Desktop's WSL integration to be enabled for this distro.

If you ever need to bootstrap a Laravel project from scratch into a
non-empty `api/` directory again, see the notes in
[docs/implementation/adr/0015-local-dev-environment.md](docs/implementation/adr/0015-local-dev-environment.md) —
`composer create-project` refuses to run in a non-empty directory, and a
few Docker/PHP-extension gotchas aren't obvious from the error messages
alone.

### Mobile

See [mobile/README.md](mobile/README.md) — requires the Flutter SDK
installed natively (not in Docker) since it needs emulator/simulator and
USB device access.

### Secret scanning

This repo ships a pre-commit hook that blocks commits containing likely
secrets, via [gitleaks](https://github.com/gitleaks/gitleaks#installing).
After cloning, install gitleaks and point git at the repo's tracked hooks
once:

```bash
git config core.hooksPath .githooks
```

## License

AGPL-3.0 — see [LICENSE](LICENSE). Chosen specifically so that anyone
running a modified or hosted version (e.g. an NGO's own instance) is
required to share their source too.
