# 0016. Editor tooling: VS Code

Date: 2026-09-17
Status: Accepted

## Context

Development spans two very different language ecosystems (Dart/Flutter and
PHP/Laravel) plus Docker. Tooling needed to cover code inspection,
autocompletion, and formatting for both without requiring two entirely
separate IDEs.

## Decision

Use **VS Code** (or VSCodium for a fully open-source, telemetry-free build)
as the primary editor, with:

- **Dart & Flutter** (official extensions) for the mobile client.
- **Intelephense** for PHP autocompletion/inspection.
- **Laravel Pint** for PHP formatting (Laravel's official zero-config
  PHP-CS-Fixer wrapper).
- **Larastan** (PHPStan configured for Laravel) for static analysis.
- **Laravel Extra Intellisense** + **Laravel Blade Snippets** for
  route/view/config autocompletion Intelephense can't infer on its own.
- **Dev Containers** to open the Laravel side of the project inside its own
  Docker container, so autocompletion resolves against the exact PHP
  version/extensions that actually run (see
  [ADR 0015](0015-local-dev-environment.md)), not whatever happens to be on
  the host.
- **EditorConfig** extension, backed by a single repo-root `.editorconfig`.
- A committed `.vscode/extensions.json` so anyone who clones the repo —
  including an NGO adopting it — is prompted to install the same tooling.

## Alternatives considered

- **PhpStorm + Android Studio** — best-in-class PHP refactoring and official
  Flutter tooling, respectively, but means juggling two separate IDEs.
  JetBrains offers free licenses to active open-source maintainers, and
  Flutter/Dart plugins can be installed into PhpStorm directly (it's built
  on the IntelliJ platform) to get one unified IDE — kept as an option if
  Intelephense ever feels limiting, not adopted as the starting point.

## Consequences

- One editor covers both codebases day to day.
- Formatting and linting are enforced by tools (Pint, `dart format`,
  Larastan, `flutter_lints`) rather than by convention, so they're
  consistent regardless of which editor a given contributor uses.
