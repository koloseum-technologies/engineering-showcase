# Product status

Koloseum is an **unreleased MVP**. Competitions is the product core and is still at specification. Lounges and Sessions are supporting infrastructure that has been built first.

## Implementation vs roadmap

| Service | MVP roadmap | Implementation |
| --- | --- | --- |
| Lounges Account | In MVP | **Completed** |
| Lounges Staff | In MVP | **Completed** |
| Lounges Operations | In MVP | **Completed** |
| Players Sessions | In MVP | **Completed** |
| Public Authentication | In MVP | **Shipped to production; not currently serving traffic.** Most-developed repo (v0.4.1, 42 tags). |
| Players Competitions | Product core | Specification / active design |
| Players Account | In MVP | Backlog |
| Backroom Compliance | In MVP | Substantial implementation; designated reference repo; still listed backlog on older boards |
| Backroom Staff | In MVP | Backlog |
| Public Legal | In MVP | Implemented as a static app; not a traffic story |
| Public Help, Public Landing | In MVP | Backlog |
| Backroom Competitions | Not in MVP | Do not list as active |
| `players-fgc` | Not in MVP | Deprecated placeholder |

Highest version anywhere is 0.4.3 (`@koloseum/types`); nothing is at v1.0.0, which matches an unreleased MVP.

## Production funnel (qualified 27 August 2026)

Operating window: **4 March 2025 – 5 October 2025**. All 83 `auth.users` rows are phone identities.

| | Raw | After excluding 3 Backroom superusers |
| --- | --- | --- |
| Sign-ups | 83 | 80 |
| Completed Player registrations | 54 (0 soft-deleted) | 51 |
| Abandoned (no Player row) | 29 | 29 |
| Completion rate | 65% | 64% |

The flow includes age gating and Smile ID. A ~64% completion rate through ID verification is a product observation; “83 users” is not traction.

**Lounge path:** 1 `compliance.lounges` row, still active, and **1 branch**. That Lounge superuser also has a Player row — the original “55 completed / 28 abandoned” split double-counted them. Unique completed people = 54.

**Return visits:** 80 of 83 ever signed in; 32 signed in more than a day after signup.

Monthly shape: March 39 (20 completed), then a quiet April–May, a second pulse in July–September, last sign-ups 5 October 2025.

## Screenshots

Lounge and Player Sessions UI: [`screenshots/`](../screenshots/). **Backroom has none.** If a claim needs Backroom, capture it; until then do not imply a public Backroom walkthrough.

## Data while the app is down

The production database is still live. Retention and access are in [`data-protection.md`](data-protection.md).
