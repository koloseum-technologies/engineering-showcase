# End-to-end testing strategy

Koloseum's completed services are not just unit-tested UI; they have E2E (Playwright) suites that log in as real roles against a seeded local Supabase instance and click through established workflows.

## Stack

- **Vitest:** unit tests in the apps (`src/lib/helpers`) and 257 passing tests in `@koloseum/utils`
- **Playwright:** 41 spec files, 222 `test` / `describe` blocks across the microservices
- **MSW:** provider HTTP in unit tests
- **Supawright:** Supabase-backed E2E (service-role key, DB port, factories)
- **Snaplet:** deterministic seeds: a 1,457-line helper library, a 6,170-line generated user seed, 897 lines of curated-data rules

## How a suite runs

E2E runs against **build + preview**, not `vite dev`. Port conflicts, Safari/WebKit skips, and HS256 JWT pitfalls are documented in `e2e/KNOWN_ISSUES.md` in each repo.

Seeds live in the Supabase repo. Typical flow: `supabase start`, `supabase db reset` (applies schema + seeds), copy `API_URL` / `ANON_KEY` / `SERVICE_ROLE_KEY` into the app `.env`, `npm run test:e2e`.

Payment tests are opt-in (`E2E_PAYMENTS_MOCK=1`) and need the Paystack function served with `PAYSTACK_MOCK_SETTLEMENT=true`, so a production environment cannot mock settlement by accident.

## What is covered on the completed services

Authentication (OTP, password, registration, reset), role-based gates, session lifecycle, Lounge operations (branches, reputation, finances, ownership transfer), and Kubernetes health checks.
