# 0010. Push notifications: on-device first, UnifiedPush/APNs if needed

Date: 2026-09-17
Status: Accepted

## Context

Reminders are a core feature of this app. The default industry approach is
Firebase Cloud Messaging (FCM) for Android and APNs for iOS, but FCM ties
the app to Google Play Services and routes notification metadata through
Google — in tension with this project's self-hosting and data-sovereignty
stance (see [ADR 0004](0004-self-hosted-backend.md)).

## Decision

Default to **on-device local notifications**, scheduled directly via the OS
notification APIs, requiring no server or third party at all. If a
server-triggered push is ever genuinely needed, use **UnifiedPush** (an
open, decentralized push standard, e.g. via a self-hosted `ntfy` relay) for
Android, and **APNs directly** for iOS.

## Alternatives considered

- **Firebase Cloud Messaging** — the default choice for most apps, but
  requires Google Play Services and routes through Google's infrastructure;
  rejected given the project's values.
- **UnifiedPush as the default from day one** — unnecessary complexity for
  v1, since most of the "fun reminder" use case doesn't need
  server-triggered pushes at all; kept as the fallback for when it's
  actually needed rather than built prematurely.

## Consequences

- Most of the reminder feature ships with zero server dependency and zero
  third-party involvement, which is both the most private and the simplest
  option.
- If server-triggered notifications are added later, Android support
  requires UnifiedPush to be explicitly implemented in the app (client-side
  support isn't universal), which is accepted as a reasonable trade-off.
- iOS has no FOSS alternative to APNs — this is treated as acceptable since
  APNs is Apple's own first-party infrastructure for its own platform, not a
  third-party data broker.
