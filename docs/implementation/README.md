# Implementation decisions

Technical decisions are recorded as lightweight Architecture Decision
Records (ADRs) — one file per decision, written once and superseded (not
rewritten) if a later decision changes course. See
[0000-template.md](adr/0000-template.md) for the format used.

Also see [open-questions.md](open-questions.md) for things that came up but
aren't decided or aren't done yet.

## Index

| ADR | Decision |
|---|---|
| [0001](adr/0001-mobile-framework.md) | Mobile app framework: Flutter/Dart |
| [0002](adr/0002-backend-framework.md) | Backend framework: Laravel + Octane |
| [0003](adr/0003-database.md) | Database: PostgreSQL |
| [0004](adr/0004-self-hosted-backend.md) | Self-hosted backend, not a third-party BaaS |
| [0005](adr/0005-hosting-provider-and-server-sizing.md) | Hosting provider and server sizing: Hetzner Cloud, CAX11 |
| [0006](adr/0006-deployment-tooling.md) | Deployment tooling: Dokku |
| [0007](adr/0007-cicd-build-strategy.md) | CI/CD build strategy: build off-box, deploy a prebuilt image |
| [0008](adr/0008-observability.md) | Observability: Beszel, Laravel Pulse, and Telescope |
| [0009](adr/0009-dns-and-domain.md) | DNS and domain: keep IONOS, avoid proxying through Cloudflare |
| [0010](adr/0010-push-notifications.md) | Push notifications: on-device first, UnifiedPush/APNs if needed |
| [0011](adr/0011-analytics-and-crash-reporting.md) | Analytics and crash reporting: none for v1 |
| [0012](adr/0012-repo-visibility-and-license.md) | Repository visibility and license: public, AGPL-3.0 |
| [0013](adr/0013-source-hosting.md) | Source hosting: GitHub (working assumption), Codeberg considered |
| [0014](adr/0014-rust-as-opportunistic-satellite.md) | Rust: opportunistic satellite services, not the primary backend |
| [0015](adr/0015-local-dev-environment.md) | Local dev environment: Docker for the backend, Flutter native on the host |
| [0016](adr/0016-editor-tooling.md) | Editor tooling: VS Code |
| [0017](adr/0017-app-identity.md) | App identity: working name and bundle identifier |
