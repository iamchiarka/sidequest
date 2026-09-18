# 0001. Mobile app framework: Flutter/Dart

Date: 2026-09-17
Status: Accepted

## Context

The app needs to ship on both iOS and Android. The maintainer's existing
experience is in web development (Nuxt/JavaScript) and PHP/Laravel, with some
Java, Python, and Rust. This project is explicitly also a learning exercise —
both for mobile app development and for agentic coding workflows — so the
framework choice needed to balance shipping speed against genuine learning
value, without taking on too much unfamiliar technology at once (see
[ADR 0002](0002-backend-framework.md) for the same reasoning applied to the
backend).

## Decision

Build the mobile client in **Flutter/Dart**.

## Alternatives considered

- **React Native + Expo** — fastest option given existing JavaScript/React
  experience, but the least new: it wouldn't provide much learning value,
  which was an explicit project goal.
- **Kotlin Multiplatform + Compose Multiplatform** — builds on existing Java
  experience and offers true native performance, but has a smaller
  ecosystem/community and fewer tutorials for full UI sharing, making solo
  development slower.
- **Flutter/Dart** — genuinely new language, but with a gentle learning
  curve, a large ecosystem, and strong community support for exactly this
  kind of gamified, UI-heavy indie app. Best balance of "new and fun to
  learn" against "realistic to ship solo in free time."

## Consequences

- Dart is a new language for the maintainer to learn, which was one of the
  explicit goals of this project.
- Flutter's SDK is open source (BSD-3-licensed) and runs entirely
  client-side — it is a build tool, not a third-party service that
  processes user data, so it doesn't conflict with the project's
  data-sovereignty stance (see [ADR 0004](0004-self-hosted-backend.md)).
- Flutter development runs natively on the host machine rather than in
  Docker — see [ADR 0015](0015-local-dev-environment.md) for why.
