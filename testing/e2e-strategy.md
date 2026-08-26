# End-to-end testing strategy

Koloseum's completed services are not “unit-tested UI”. They have Playwright suites that log in as real roles against a seeded local Supabase and click through money, sessions and staff workflows.

## Stack

- **Vitest** — unit tests in the apps (`src/lib/helpers`) and 257 passing tests in `@koloseum/utils`
- **Playwright** — 41 spec files, 222 `test` / `describe` blocks across the microservices
- **MSW** — provider HTTP in unit tests
- **Supawright** — Supabase-backed E2E (service-role key, DB port, factories)
- **Snaplet** — deterministic seeds: a 1,457-line helper library, a 6,170-line generated user seed, 897 lines of curated-data rules

The 916 Vitest “blocks” figure counted `test` + `describe` + `it` together; treat package-level 257 passing tests as the hard number.

## How a suite runs

E2E runs against **build + preview**, not `vite dev`. Port conflicts, Safari/WebKit skips, and HS256 JWT pitfalls are documented per repo in `e2e/KNOWN_ISSUES.md` (the README template requires that link).

Seeds live in `koloseum/supabase`. Typical flow: `supabase start`, `supabase db reset` (applies schema + Snaplet), copy `API_URL` / `ANON_KEY` / `SERVICE_ROLE_KEY` into the app `.env`, `npm run test:e2e`.

Payment tests are opt-in (`E2E_PAYMENTS_MOCK=1`) and need the Paystack function served with `PAYSTACK_MOCK_SETTLEMENT=true`, so a production ConfigMap cannot mock settlement by accident.

## What is covered on the completed services

Authentication (OTP, password, registration, reset), role-based gates, session lifecycle, branch operations, inventory, eatery, settings, reputation, finances, ownership transfer, Kubernetes health checks.

## What is not public

The specs live in private repositories. The public stand-in is [`man-of-substance`](https://github.com/DavyK17/man-of-substance): 15 Vitest tests on helpers, Playwright configured, `e2e/locked` and `e2e/unlocked` still empty. That gap is listed as optional follow-up, not as current evidence.
