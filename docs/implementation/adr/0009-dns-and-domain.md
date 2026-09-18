# 0009. DNS and domain: keep IONOS, avoid proxying through Cloudflare

Date: 2026-09-17
Status: Accepted

## Context

The maintainer personally registers domains through IONOS (a German
company) rather than Cloudflare, originally for cost reasons on a
single-domain basis. Separately, professional experience uses Cloudflare
for DNS at a company level. This project needed its own decision given its
self-hosting and data-sovereignty stance (see
[ADR 0004](0004-self-hosted-backend.md)).

Two questions turned out to be conflated: domain *registration* versus DNS
*proxying*. Cloudflare's basic DNS hosting is free regardless of domain
count — the real trade-off isn't cost, it's that Cloudflare (a US company)
proxying traffic terminates TLS at its edge, meaning it would see plaintext
of all API traffic — including mood-tracking/notes data — before it ever
reaches the project's own server. That is the same category of concern as
self-hosting the backend in the first place, just at the network layer
instead of the database layer.

## Decision

Keep domain registration and DNS at **IONOS** (German, GDPR-aligned). Do
**not** proxy production traffic through Cloudflare. TLS is terminated
directly on the project's own Hetzner server via Dokku's Let's Encrypt
plugin.

## Alternatives considered

- **Moving to Cloudflare (proxied)** — rejected: introduces a US company
  into the request path with visibility into all API traffic, undermining
  the self-hosting decision; also provides no functional benefit for a
  private mobile API that isn't a public website under DDoS risk.
- **Cloudflare in DNS-only ("grey cloud") mode** — would avoid the
  traffic-inspection concern, but adds no benefit over IONOS DNS for this
  project's needs, so there's no reason to add the extra moving part.
- **deSEC** (German non-profit, free DNSSEC-enabled DNS) — a values-aligned
  option if DNSSEC or nicer DNS tooling is wanted later; not adopted yet
  since IONOS already works.

## Consequences

- No third party sees plaintext API traffic; the trade-off is forgoing
  Cloudflare's free edge DDoS/WAF protection, which is judged unnecessary
  for a mobile app's private API.
- If a public marketing/landing page is ever built for this project,
  Cloudflare in DNS-only mode for that specific subdomain remains an option
  without revisiting this decision for the API itself.
