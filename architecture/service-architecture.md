# Service architecture

Koloseum is a set of independently deployable SvelteKit applications, grouped by who they are for, sharing one PostgreSQL database and three npm packages.

## Why microservices here

Horizontal scaling (Player-facing traffic will dominate), isolation of failure, and containerised deploys onto DigitalOcean Kubernetes. Each service is a container with a multi-stage non-root Dockerfile. Group services (Players, Lounges, Backroom) share an ingress in the group's `*-account` (or compliance) repo; public services own their host (`auth.koloseum.ke`).

This is not a distributed monolith pretending to be services: there is **one database**. Service boundaries are about UI, authz gates and deploy units, not about splitting data across networks. Cross-service contracts live in `@koloseum/types` and `@koloseum/utils`, not in ad-hoc HTTP between apps.

## The four groups

| Group | Host pattern | Auth | Typical job |
| --- | --- | --- | --- |
| Public | `<name>.koloseum.ke` | Unauthenticated entry; registration and login | Create an account, read legal copy |
| Players | `players.koloseum.ke/<slug>` | Optional on some routes; SSO | Compete, sit a session, manage an account |
| Lounges | `lounges.koloseum.ke/<slug>` | Mandatory; `microserviceGroup: "lounges"` | Run a venue |
| Backroom | `backroom.koloseum.ke/<slug>` | Mandatory; staff roles | Compliance, registrations, staff access |

## What actually exists

Eleven non-template services, 74,031 lines under `src/`. Largest: `lounges-operations` at 23,913 lines / 126 files. Five services run Valkey with tenant-scoped keys, per-class TTLs and pattern invalidation. Ten have `.doks/` manifests.

Completed in the current MVP slice: **Lounges Account, Lounges Staff, Lounges Operations, Players Sessions**. `public-auth` is the most-developed repo (1,367 commits, 42 tags, v0.4.1) and is the only service that has met real users. Competitions is the product core and is at specification. `players-fgc` is a deprecated placeholder; there is no Backroom Competitions service in the MVP.

## Shared platform

- **PostgreSQL 17** with RLS, RPCs, `pg_net`, `pg_cron`, `btree_gist`, `pgcrypto`, `supabase_vault`
- **Supabase** Auth, Storage, Edge Functions (Deno)
- **`@koloseum/types` / `utils` / `components`** — see [ADR-003](../decisions/ADR-003-shared-typescript-packages.md)
- **Sentry** in the apps; health probes that respect `paths.base`

A specification-driven seven-step roadmap ([`MS-ROADMAP`](https://github.com/koloseum-technologies)) governs each new service: spec review → visualisation → database review → Edge Functions → implementation → tests → DOKS.
