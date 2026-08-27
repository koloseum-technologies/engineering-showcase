# Data architecture

The database is the strongest engineering asset on the platform. Rules and constraints are primarily enforced there rather than being re-implemented on the frontend.

## Size (declarative schema under `supabase/schemas/`)

| Measure                 | Count                                   |
| ----------------------- | --------------------------------------- |
| Lines of SQL            | 31,024 across six schema files          |
| Tables                  | 145, six PostgreSQL schemas, RLS on all |
| RLS policies            | 351                                     |
| PL/pgSQL functions      | 390                                     |
| Triggers                | 202                                     |
| Indexes                 | 299                                     |
| Named CHECK constraints | 183                                     |
| Enum types              | 45                                      |

Since our backend is managed on Supabase, the database runs on PostgreSQL 17, with extensions in use including `pg_net`, `pg_cron`, `btree_gist`, `pgcrypto`, and `supabase_vault`. All amounts are recorded as integers in cents and the default timezone is `Africa/Nairobi`.

## Where the rules live

Examples of business rules that are database-enforced:

- Rolling 24-hour top-up limits for Koloseum Credits
- Lounge "regular" status: four distinct weeks of ≥ 60-minute sessions at any of a Lounge's branches
- Session lifecycle state machine, including a block on checkout while eatery orders are open
- Conversion of prize money from competitions to Koloseum Credits
- Auto top-up firing through `pg_net` into an Edge Function, with secrets read from Vault

The app layer still handles workflows, retries, user-facing errors, and calls to providers that are not trigger-driven. It does not get a second chance to invent a top-up limit.

## Caching

Selected services put high-frequency reads in Valkey. Keys are tenant-scoped, TTLs are categorised by data class, mutations invalidate by pattern, and a cache miss is never an application failure. Some paths still rely on TTL expiry rather than event-driven invalidation; see [`product/limitations.md`](../product/limitations.md) for more info.
