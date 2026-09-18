# 0015. Local dev environment: Docker for the backend, Flutter native on the host

Date: 2026-09-17 to 2026-09-18
Status: Accepted

## Context

The team has existing Docker experience for local development. Flutter,
however, needs direct host access to the Android emulator/iOS simulator and
USB debugging for physical devices, and iOS builds require Xcode, which
cannot run in a Linux container at all.

## Decision

- **Backend** (Laravel + Postgres + Redis): run in **Docker Compose**,
  matching the production runtime and avoiding host/container PHP version
  drift.
- **Mobile** (Flutter): run **natively on the host machine**, talking to the
  Dockerized API over `localhost`. The one exception is a one-off `flutter
  create` scaffold command, which can run through a throwaway container if
  the Flutter SDK isn't installed on the host yet — day-to-day development
  still needs the SDK installed natively.
- Composer/PHP are **not required on the host at all** — a one-off
  `composer` service in `docker-compose.yml` runs Composer commands against
  the bind-mounted project directory.

## Alternatives considered

- **Containerizing Flutter for daily development** — rejected: breaks
  emulator/simulator and physical-device access, and can't build for iOS at
  all.
- **Installing PHP/Composer on the host** — rejected as unnecessary; the
  one-off Composer container achieves the same thing without adding a
  second place PHP's version could drift from production.

## Implementation notes (gotchas hit while scaffolding)

These were real build failures encountered getting this running, kept here
so they aren't re-discovered from scratch:

- **Composer version pins can go stale fast.** Pinning `laravel/laravel` to
  `^11.0` failed outright because Composer's security-advisory check blocks
  every 11.x release in range once they're all flagged. Installing without a
  version pin (latest stable) resolved it.
- **Swoole needs Brotli headers.** `pecl install swoole` fails on Alpine
  without `brotli-dev` installed alongside `postgresql-dev`/`linux-headers`.
- **The final Docker build stage needs its own `composer` binary.** Using a
  multi-stage build where only the `vendor` stage has Composer means
  `composer dump-autoload` in the final stage fails with `composer: not
  found` unless the binary is explicitly copied in from the `composer:2`
  image.
- **Current Laravel requires PHP ≥ 8.4.1.** The base image had to be bumped
  from `php:8.3-cli-alpine` to `php:8.4-cli-alpine`.
- **Octane needs the `pcntl` extension**, and Laravel's Redis-backed
  session/cache/queue drivers need the `redis` PECL extension — neither is
  present in a base PHP image by default.
- **Files created by root-run containers land root-owned on the host**,
  since Composer/artisan commands run as root inside the container against a
  bind mount. Fixed by running a throwaway container
  (`docker run --rm -v "$(pwd)":/app alpine chown -R "$(id -u):$(id -g)" .`)
  to reclaim ownership — a root process inside a container can chown files
  it doesn't "own" from the host user's perspective, so this works even
  when the host user can't `chown` them back directly.
- **Laravel's default template ships its own `.editorconfig` with `root =
  true`**, which shadows the repository's root `.editorconfig` for
  everything under `api/`. Removed in favor of the single repo-wide config.
- **The default installer's post-install migration runs against a throwaway
  SQLite file**, not the project's real Postgres connection, since it runs
  during `composer create-project` before the project's actual `.env` is in
  place. The stray `database/database.sqlite` file this leaves behind was
  deleted.
- **`APP_KEY` generation runs against whatever `.env` exists at install
  time** — since a pre-existing custom `.env` was intentionally preserved
  during the merge (see below), `php artisan key:generate` had to be
  re-run explicitly against the real project `.env`.
- **Bootstrapping into a non-empty directory:** since this repo already had
  `Dockerfile`/`docker-compose.yml`/`.env.example` in `api/` before Laravel
  was installed, `composer create-project` refused to run there directly
  (it requires an empty target directory). Worked around by installing into
  a temp path inside the container and merging with `cp -rn` (no-clobber),
  which preserves the project's own Docker/env files while adding
  everything Laravel's installer generates.

## Consequences

- The `api/Dockerfile` and `api/docker-compose.yml` reflect all of the fixes
  above; they should not need to be rediscovered.
- Anyone (including an NGO deploying their own instance) can build and run
  the backend using only Docker — no host PHP/Composer/Flutter toolchain is
  required except to develop the mobile client itself.
