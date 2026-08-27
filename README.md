# Koloseum engineering showcase

Koloseum is Kenya's competition platform for esports. Players discover, enter and follow structured competitions; community organisers (themselves Players) administer Leagues, Challenges, and Locals in public; Lounges administer day-to-day operations and host events.

This repository is the public engineering record, while source code remains private. What you can inspect here is the architecture, the decisions, a worked case study, the testing strategy, product status, and screenshots of the completed MVP services, taken from locally seeded data.

## Author

- **Davy Kamanzi** | [@DavyK17](https://github.com/DavyK17) – Head of Technology and Community Relations, Ace Pro Sports Technologies

## What has been built

Eleven non-template microservices (SvelteKit, TypeScript) on a shared PostgreSQL 17 / Supabase backbone, plus three versioned npm packages.

| Domain       | Services (MVP)                       | Implementation                                                                   |
| ------------ | ------------------------------------ | -------------------------------------------------------------------------------- |
| **Public**   | Authentication, Legal, Help, Landing | Authentication shipped to production (not currently live); others on the roadmap |
| **Players**  | Competitions, Sessions, Account      | Sessions completed; Competitions ongoing; Account on the roadmap                 |
| **Lounges**  | Operations, Staff, Account           | All three completed                                                              |
| **Backroom** | Compliance, Staff                    | Compliance awaiting updates; Staff on the roadmap                                |

**Data layer (declarative schema):** 31,024 lines of SQL · 145 tables (RLS on all) · 351 policies · 390 PL/pgSQL functions · 202 triggers · 299 indexes · 183 named CHECKs.

**Integrations:** Paystack, Flutterwave, GavaConnect (KRA eTIMS), Smile ID, SuprSend, Twilio, Zoho, IGDB — ten Edge Functions, approx. 9,500 lines of TypeScript.

**Testing:** Playwright E2E across the completed services, Vitest at the app and package layers, Snaplet-driven deterministic seeds.

**Production:** `public-auth` ran from Q1 to Q4 2025. 83 phone sign-ups produced 54 completed Player registrations (65%) through an age-gated flow with Smile ID. Excluding three internal Backroom accounts: 51 of 80 external sign-ups completed (64%). One Lounge company account registered and created a branch. The application is not currently serving traffic, but the production database is still live. See [`product/status.md`](product/status.md) for more details.

## How it is engineered

```text
                         Public
                            |
               +------------+------------+
               |                         |
            Players                   Lounges
               |                         |
               +------------+------------+
                            |
                         Backroom
                            |
                    Shared platform
                            |
               +------------+------------+
               |            |            |
          PostgreSQL    Supabase      Shared npm
          + RLS         + Edge        packages
          + RPCs        Functions
```

Interesting problems to explore:

1. Business rules in the database: [`architecture/data-architecture.md`](architecture/data-architecture.md), [ADR-002](decisions/ADR-002-database-enforced-business-rules.md)
2. Service boundaries without a distributed monolith: [ADR-001](decisions/ADR-001-service-boundaries.md)
3. Authentication and authorisation: [`architecture/authentication-and-authorisation.md`](architecture/authentication-and-authorisation.md)
4. Shared TypeScript packages: [ADR-003](decisions/ADR-003-shared-typescript-packages.md)

A smaller, fully public application that implements several of the same patterns is [`DavyK17/man-of-substance`](https://github.com/DavyK17/man-of-substance) (SvelteKit, Supabase, RLS, generated database types).

## What you can inspect

| Path                                                                 | What it is                                        |
| -------------------------------------------------------------------- | ------------------------------------------------- |
| [`architecture/`](architecture/)                                     | System context, services, data, authz             |
| [`decisions/`](decisions/)                                           | ADRs 001–003                                      |
| [`case-studies/lounges-account.md`](case-studies/lounges-account.md) | One completed microservice, end to end            |
| [`testing/e2e-strategy.md`](testing/e2e-strategy.md)                 | Playwright, Supawright, Snaplet seeds             |
| [`product/status.md`](product/status.md)                             | What is done, what isn't, the registration funnel |
| [`product/limitations.md`](product/limitations.md)                   | Honest constraints                                |
| [`screenshots/`](screenshots/)                                       | Curated UI captures from seeded data              |

**npm (public):** [`@koloseum/types`](https://www.npmjs.com/package/@koloseum/types) · [`@koloseum/utils`](https://www.npmjs.com/package/@koloseum/utils) · [`@koloseum/components`](https://www.npmjs.com/package/@koloseum/components)

## License

All rights reserved; quotation with attribution is welcome. See [LICENSE](LICENSE).
