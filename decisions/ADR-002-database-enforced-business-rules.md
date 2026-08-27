# Architecture Decision Record: Database-enforced business rules

**Status:** Accepted  
**Date:** 2023–2026

## Context

Koloseum meters Credits, administers sessions and computes "regular" status at Lounges, and will eventually settle competition prize pools. Such rules are easy to implement twice (once in SvelteKit, once "for safety" in SQL) and then drift; multiple apps write to the same database.

## Decision

- **PostgreSQL is the system of record.** Supabase manages the database infrastructure and provides authentication, storage, and Edge Functions (Deno) for external provider I/O.
- **RLS is enabled on every table.** Feature flags in the app are not a substitute for RLS policies.
- **Put the rule in the database when the database can evaluate it** with mechanisms such as CHECKs, exclusion constraints, PL/pgSQL functions, and triggers. The app layer only orchestrates.
- **Use `pg_net` only to signal events** (e.g. triggering SuprSend notifications). Heavy I/O, retries, and user-facing errors stay in the app or the Edge Function as appropriate.
- **The default timezone is `Africa/Nairobi`.** Do not set a session `TimeZone` that contradicts that without revisiting the constraints.

## Consequences

- With a maintenance surface of hundreds of functions and policies, schema maps and the database review step of the microservice implementation roadmap exist as a necessity rather than mere presentation.
- Application bugs are less likely to corrupt identity, money, or another user's data; they are more likely to show up as a raised SQLSTATE that must be mapped to an HTTP response. `@koloseum/utils` provides that mapping.
- Local development depends on the Supabase CLI and Snaplet seeds. A fresh `supabase db reset` must reproduce production authorisation; otherwise, public samples are misleading.
