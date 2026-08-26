# ADR-002 — PostgreSQL, Supabase, and database-enforced business rules

**Status:** Accepted  
**Date:** 2024–2026

## Context

Koloseum meters Credits, tracks sessions against opening hours, computes “regular” status, and will eventually settle prizes. Those rules are easy to implement twice (once in SvelteKit, once “for safety” in SQL) and then drift. Multiple app services write to the same data. Kenya's betting/gaming overlay on competitions with entry fees means the ledger must be boring and auditable.

## Decision

- PostgreSQL 17 is the system of record. Supabase provides Auth, Storage, generated types, and Edge Functions (Deno) for provider I/O.
- **RLS on every table.** Feature flags in the app are not a substitute for row policies.
- **Put the rule in the database when the database can evaluate it:** CHECKs, exclusion constraints, PL/pgSQL RPCs, triggers. The app orchestrates; it does not re-derive a 24-hour top-up cap.
- Use `pg_net` only to **signal** (for example auto top-up → Edge Function). Heavy I/O, retries and user-facing errors stay in the app or the function.
- Civil time is `Africa/Nairobi`. Do not set a session `TimeZone` that contradicts that without revisiting the constraints.

## Consequences

- 390 functions and 351 policies is a real maintenance surface. Schema maps and the database-review step of the roadmap exist because of this choice, not as decoration.
- Application bugs are less likely to corrupt money or leak another Lounge's rows; they are more likely to show up as a raised SQLSTATE that must be mapped to HTTP. `@koloseum/utils` and the public `parsePostgrestError` in `man-of-substance` are that mapping.
- Local DX depends on the Supabase CLI and Snaplet seeds. Fresh `db reset` must reproduce production authorisation, or the public sample (and every microservice) lies.
