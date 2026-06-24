# A real Orange Hill week (anonymized)

This is the actual shape of a week here — three live projects at three different stages, one of them on fire. Details are anonymized (no client names, no confidential domains), but the stacks, the complexity, and the fires are real. Read it, then write your plan (the task is in [`application-task.md`](application-task.md)).

---

## Project A — "The Launch" 🔥 (hard, immovable deadline)
A high-traffic live media platform going live for a major event with a launch date that **cannot move**. Built for ~100K concurrent users via CDN-cached polling (no WebSockets, on purpose).

**Stack:** a Laravel headless-CMS API + a Next.js (SSR/ISR) web frontend + two Python/FastAPI services (a live-data importer and an AI news/live-blog pipeline); MySQL + Postgres/pgvector + Redis; AWS ECS Fargate behind CloudFront; FCM/APNs push; an LLM API (Claude) for multi-locale live content.

**On fire this week:**
- The third-party **live-data feed is on a trial key that drops to ~1k calls/day a few days before go-live** — the production license is stuck on the client's side. Hard date.
- **CloudFront-in-front-of-the-API cutover**: an auth-aware feed endpoint where one wrong cache key either leaks a logged-in user's personalized feed to everyone or kills the cache under peak load.
- **Image resizing is silently broken in prod** — every mobile user is downloading full-res images (~6× the bandwidth) on a low-bandwidth audience. Two broken layers, two owners.
- **Push fan-out migration** (per-user loops → topic subscriptions) still needs launch-day validation across 5 locales; the APNs key is unconfirmed.

## Project B — "The Build" 🔒 (stealth, under NDA — technical shape only)
A greenfield product on a fixed 10-week contract. Design is **still churning while the build starts** — classic early-phase.

**Stack:** React Native (Expo) mobile + a Next.js web funnel + a Laravel headless CMS with an auto-generated admin; MySQL; JWT + refresh + RBAC; email-only notifications in phase 1; shipped single-tenant but architected for an eventual multi-tenant white-label.

**In flight this week:**
- Stand up the **core entity + an append-only event-timeline** — one endpoint serving both the admin panel and a *role-filtered* mobile view, with per-transition email triggers.
- **Patch the CMS auth** from its stock email-link verification to an in-app 6-digit code (modifying vendor core, then layering JWT/RBAC on top).
- Build a **fuzzy duplicate-detection service**: multi-field matching with dual-script (Latin/Cyrillic) + diacritic normalization, score-thresholded.
- **Reconcile a mid-flight design revision against the signed scope** (strip phase-2 creep, add missing screens) before scaffolding the mobile shell.

## Project C — "The Live App" (shipped, post-launch)
A shipped consumer mobile app, doing retention and premium work.

**Stack:** native mobile; a real-time on-device feature (camera / vision); in-app subscriptions; a backend; LLM APIs for lookups; push notifications.

**On your plate this week:**
- **The latest build was rejected in app-store review** — a policy issue with how the paywall gates access (the user has to sign in before they can see any value). Ship the smallest change that actually passes review, and scope the deeper fix so it doesn't recur.
- A **pricing mismatch**: the app and site advertise prices that don't match what the store actually charges — an unpleasant surprise at checkout. Fix before any marketing push.
- A **backend-audit finding**: subscription lifecycle events aren't reaching the backend (a misconfigured webhook), so renewals and cancellations are invisible server-side.
- Kick off **a new-language localization** (no localization infra exists yet) for a partner conversation.

---

It's Monday morning and all three are yours. The Launch has the immovable date; the Build has a fixed contract and shifting design; the Live App is steady but review-blocked with a couple of trust bugs. Now go to [`application-task.md`](application-task.md) and write the week.
