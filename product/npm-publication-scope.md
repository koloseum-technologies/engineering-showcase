# npm publication scope (deferred)

**Status:** accepted trade-off until after the current job search. Revisit then.

`@koloseum/types` publicly ships the generated database schema (`database-generated.d.ts`). `@koloseum/utils` ships the role-permission matrix, including entries for unreleased Commerce and KLSM services.

The application repositories remain private, so this is not a source leak. It is still a disclosure of internal structure: table names, enum values, and which roles may call which features.

**Why it stays for now**

- Microservices already depend on the published versions; yanking or splitting the packages mid-search would churn every private repo for no user-facing gain.
- Packaging hygiene (README, `UNLICENSED`, working links) is the change that was needed immediately. The payload is unchanged.
- A documented trade-off reads better in diligence than an accidental one.

**Options when revisited:** trim `database-generated.d.ts` and unreleased entries from `platform.js`; move to a restricted npm scope; or split public/private packages.

## Packaging patches (August 2026)

README, `UNLICENSED`, author email and homepage/bugs/repository links now point at this showcase. Those commits are on each package's `main`:

| Package | Git | Registry (still) |
| --- | --- | --- |
| `@koloseum/types` | 0.4.4 | 0.4.3 |
| `@koloseum/utils` | 0.3.16 | 0.3.15 |
| `@koloseum/components` | 0.2.3 | 0.2.2 |

Publication needs a fresh `npm login`; the local npm token returns 401. Until then, `npm view` will show the previous versions.

Author: Davy Kamanzi, August 2026.
