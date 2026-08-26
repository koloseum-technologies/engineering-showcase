# Data architecture

This is the strongest engineering asset in the estate. The product is a competition platform with a financial ledger, not a CRUD app with a database bolted on. Rules that can be enforced in PostgreSQL are enforced there.

## Size (declarative schema under `supabase/schemas/`)

| Measure | Count |
| --- | --- |
| Lines of SQL | 31,024 across six schema files |
| Tables | 145, six PostgreSQL schemas, **RLS on all 145** |
| RLS policies | 351 |
| PL/pgSQL functions | 390 |
| Triggers | 202 |
| Indexes | 299 |
| Named CHECK constraints | 183 |
| Enum types | 45 |

PostgreSQL 17. Extensions in use: `pg_net`, `pg_cron`, `btree_gist`, `pgcrypto`, `supabase_vault`.

One GiST **exclusion constraint** prevents overlapping special-event windows on the same Lounge branch (`sessions` schema). That is a constraint the application cannot reasonably police with SELECT-then-INSERT.

Amounts are **integers in cents**. Default timezone is **`Africa/Nairobi`**; session `TimeZone` must not contradict it.

## Where the rules live

Examples of business rules that are database-enforced rather than re-implemented in SvelteKit:

- Rolling 24-hour Credits top-up limits in cents (`compliance`)
- “Regular” status: four distinct weeks of ≥ 60-minute sessions at any of a Lounge's branches (`sessions`)
- Session lifecycle state machine, including a block on checkout while eatery orders are open
- Double-entry prize-money → Credits conversion
- Auto top-up firing through `pg_net` into an Edge Function, with secrets read from Vault

The application layer still does workflows, retries, user-facing errors and calls to providers that are not trigger-driven. It does not get a second chance to invent a top-up limit.

## Readable public proof

[`DavyK17/man-of-substance`](https://github.com/DavyK17/man-of-substance) is a small instance of the same idea: contributor tiers computed in a SQL view, RLS on every table, a Storage policy that scopes `mp4/<id>.mp4` to `auth.uid()`, and `parsePostgrestError` mapping `P0001` (PL/pgSQL `RAISE`) to HTTP 400 so a database-raised rule surfaces as a client error. Koloseum industrialises that pattern across 390 functions.

## Caching

Selected services put high-frequency reads in Valkey. Keys are tenant-scoped, TTLs are per data class, mutations invalidate by pattern, and a cache miss is never an application failure. Some paths still rely on TTL expiry rather than event-driven invalidation; that is recorded in [`product/limitations.md`](../product/limitations.md).
