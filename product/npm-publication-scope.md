# npm publication scope (deferred)

`@koloseum/types` publicly ships the generated database schema (`database-generated.d.ts`), while `@koloseum/utils` ships the role-permission matrix, including entries for unreleased services. The application repositories remain private, so this is not a source leak, but it is still a disclosure of internal structure: table names, enum values, and which roles may call which features.

**Why it stays for now**

- Microservices already depend on the published versions; yanking or splitting the packages mid-MVP implementation would force coordinated bumps across each private repo for no user-facing gain.
- Packaging hygiene (README, `UNLICENSED`, working links) has been implemented without any actual code changes. The documentation for each package now points to this showcase.

**Options when revisited:** trim generated database type definitions from `@koloseum/types` and unreleased entries from `@koloseum/utils`; move to a restricted npm scope; or split public/private packages.

## Packaging patches (August 2026)

As of August 2026:

| Package                | Git    | Registry (still) |
| ---------------------- | ------ | ---------------- |
| `@koloseum/types`      | 0.4.4  | 0.4.3            |
| `@koloseum/utils`      | 0.3.16 | 0.3.15           |
| `@koloseum/components` | 0.2.3  | 0.2.2            |
