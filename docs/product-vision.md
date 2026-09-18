# Product vision

## What this is

SideQuest is a free, mobile companion app for people with ADHD/AuDHD, built
around a small set of features: fun/gamified reminders, emotional (mood)
tracking, quick notes, short "fun fact" break interstitials, and an RPG-style
skill/XP progression system layered over real-life habits and tasks.

## Target audience

Primarily EU users, with a focus on German-speaking users first. This shapes
several downstream decisions: GDPR compliance is a first-class constraint
rather than an afterthought, and infrastructure choices favor EU/German
providers (see [ADR 0005](implementation/adr/0005-hosting-provider-and-server-sizing.md)
and [ADR 0009](implementation/adr/0009-dns-and-domain.md)).

## Market landscape

The ADHD-app space is more crowded than it first appears:

- **Finch** — gamified virtual pet, self-care habits, mood tracking.
- **Tiimo** — visual, time-blindness-friendly routine planner.
- **Llama Life** — single-task timeboxing / focus sessions.
- **Numo**, **Goblin.tools**, **Saner.AI**, **Sunsama** — task breakdown,
  AI-assisted planning, work-focused ADHD tooling.

None of these combine an RPG-style skill/identity progression system (leveling
up real-life skills, not just maintaining a streak) with *involuntary*
dopamine-break interstitials (a fun fact delivered between tasks as a designed
anti-doomscroll substitute, rather than a reward for finishing one). That
combination — quick notes + mood tracking + a skill/XP layer + intentional
fun-fact breaks — is the niche this project is aiming at, rather than trying
to out-feature Finch or Tiimo on their own turf.

## Project goals

- **Cost-free to use.** The app itself should never require payment,
  subscriptions, or a paid tier to be useful.
- **A learning project.** This is being built in the maintainer's free time
  to learn Flutter/Dart and agentic coding workflows, alongside shipping a
  real product — see [ADR 0001](implementation/adr/0001-mobile-framework.md)
  for how that shaped the framework choice.
- **Stable, secure, maintainable, scalable.** Despite being a hobby project,
  it's built to the same engineering standards as production software.
- **FOSS- and NGO-friendly.** Built on open-source tools, self-hosted rather
  than dependent on third-party data-processing services, and licensed so
  that any organization — including an NGO wanting to run their own instance
  for their own community — can freely do so (see
  [ADR 0012](implementation/adr/0012-repo-visibility-and-license.md)).
- **Privacy- and data-sovereignty-conscious by construction.** Mood and notes
  data is sensitive; the architecture defaults to minimizing what leaves the
  user's device and what passes through third parties at all (see
  [ADR 0004](implementation/adr/0004-self-hosted-backend.md),
  [ADR 0010](implementation/adr/0010-push-notifications.md), and
  [ADR 0011](implementation/adr/0011-analytics-and-crash-reporting.md)).
