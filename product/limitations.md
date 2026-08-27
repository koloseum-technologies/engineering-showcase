# Limitations

Honest constraints, so a reviewer does not have to discover them.

- **Unreleased MVP.** Nothing except `public-auth` has been available to the general public. `players-competitions`, the product core, is currently in active development.
- **`public-auth` is down.** It was deployed, took real registrations, and is currently not serving traffic. Language about the app is past tense; language about the database is present tense.
- **No public application source.** This showcase is documentation. The npm packages are installable; all other source code remains private.
- **Cache invalidation is mixed.** Several Valkey paths invalidate on mutation; some still rely on TTL. Do not describe the cache layer as fully event-driven.
- **Mobile-only product screenshots.** MVP service captures are taken on a phone viewport, as mobile users are expected to make up the majority of the user base. Desktop recapture is left optional, while wireframes cover desktop layout.
- **npm schema disclosure.** `@koloseum/types` and `@koloseum/utils` publish schema and role-matrix detail. Accepted for now; see [`npm-publication-scope.md`](npm-publication-scope.md) for more details.
- **Koloseum Credits are not a bank.** Rather, they are non-withdrawable platform entitlements; licensed providers move funds.
- **Timezones.** `Africa/Nairobi` is maintained as the single civil time interpretation for the MVP.
