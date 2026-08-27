# Service architecture

Koloseum is a set of independently deployable SvelteKit applications, grouped by who they are intended to serve, sharing one PostgreSQL database and three npm packages.

## Why microservices?

Microservices offer horizontal scaling (Player-facing traffic will dominate), isolation of failure, and containerised deployments, for which we use [DigitalOcean Kubernetes (DOKS)](https://docs.digitalocean.com/products/kubernetes/). Each service is a container with a multi-stage non-root Dockerfile. Group services (Players, Lounges, Backroom) share an ingress in a single designated repo, while public services own their host (e.g. `auth.koloseum.ke`).

This is not a distributed monolith pretending to be services: there is **one database**. Service boundaries come from UI, authorisation gates, and deployment units, not from splitting data across networks. Cross-service contracts live in `@koloseum/types` and `@koloseum/utils`, not in ad-hoc HTTP between apps.

## The four user groups

| Group    | Host pattern                  | Auth                                          | Typical job                                                                                    |
| -------- | ----------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Public   | `<name>.koloseum.ke`          | Unauthenticated entry; registration and login | Introduce the platform, create an account, provide supporting documentation (i.e. help, legal) |
| Players  | `players.koloseum.ke/<slug>`  | Optional on some routes; SSO                  | Compete, sit a session, manage an account                                                      |
| Lounges  | `lounges.koloseum.ke/<slug>`  | Mandatory; `microserviceGroup: "lounges"`     | Run a gaming lounge, manage staff access                                                       |
| Backroom | `backroom.koloseum.ke/<slug>` | Mandatory; staff roles                        | Platform administration (i.e. compliance), manage staff access                                 |

## What actually exists

Eleven microservices spanning 74,031 lines of code under `src/`. Five services run Valkey with tenant-scoped keys, per-class TTLs and pattern invalidation. Ten have `.doks/` manifests.

Completed in the current MVP slice: **Players Sessions, Lounges Operations, Lounges Account, and Lounges Staff**. Public Authentication is the most developed repo and the only service that has met real users. Competitions is the product core and currently in active development.

## Shared platform

- **PostgreSQL 17** with RLS, RPCs, `pg_net`, `pg_cron`, `btree_gist`, `pgcrypto`, `supabase_vault`
- **Supabase** Auth, Storage, Edge Functions (Deno)
- **`@koloseum/types` / `utils` / `components`**: see [ADR-003](../decisions/ADR-003-shared-typescript-packages.md)
- **Sentry** in the apps; health probes that respect `paths.base`

A seven-step roadmap governs each new service: specification review → visualisation → database review → Edge Functions → implementation → tests → deployment with DOKS.
