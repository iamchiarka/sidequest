# 0005. Hosting provider and server sizing: Hetzner Cloud, CAX11 to start

Date: 2026-09-17
Status: Accepted

## Context

Self-hosting (see [ADR 0004](0004-self-hosted-backend.md)) requires a server.
The maintainer already runs a small personal VPS on IONOS, used for personal
projects and their own site. A separate decision was needed for this
project: whether to reuse that VPS or provision dedicated infrastructure, and
which provider/tier to use.

Requirements: EU/German jurisdiction (GDPR alignment with the target
audience), a provider whose environmental practices fit the project's stated
values, and a cost appropriate for a free, self-funded side project.

## Decision

Provision a **dedicated Hetzner Cloud server**, separate from the
maintainer's personal IONOS VPS, starting on the **CAX11** tier (2 vCPU / 4GB
RAM, ARM/Ampere Altra, ~€4/month), in a **Falkenstein or Nuremberg**
(Germany) location, with Hetzner's automated backup add-on enabled.

## Alternatives considered

- **Reusing the existing IONOS VPS** — rejected to keep this project's
  infrastructure, billing, and access cleanly separated from other personal
  projects hosted there.
- **Hetzner CPX/CCX tiers (AMD shared/dedicated vCPU)** — significantly
  repriced upward in 2026 (roughly 2.4x–2.75x increases on CPX, more on
  CCX), no longer the value tier they used to be; unnecessary for an early
  MVP's traffic level.
- **Hetzner CX22/CX23 (Intel, cost-optimized)** — a reasonable fallback if
  the ARM-based CAX tier ever hits a compatibility snag (e.g. a Composer
  package that only ships x86_64 binaries for a compiled extension).

## Consequences

- ARM (CAX) is both the cheapest tier post-2026-price-changes and the most
  power-efficient, which fits the project's environmental stance directly
  rather than only being a cost optimization.
- Resizing (CAX11 → CAX21, or falling back to CX22 on an architecture issue)
  is a low-risk, low-downtime operation via Hetzner's console, so starting
  small carries little risk.
- **Follow-up / not yet done:** the server itself has not been provisioned
  yet as of this writing — see [open questions](../open-questions.md).
