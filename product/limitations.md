# Limitations

Honest constraints, so a reviewer does not have to discover them.

- **Unreleased MVP.** Nothing except `public-auth` has been generally available. Competitions — the product core — is at specification.
- **`public-auth` is down.** It was deployed, took real registrations, and is not serving traffic. Language about the app is past tense; language about the database is present tense.
- **Single-operator engineering.** Koloseum's 157 pull requests are self-authored and self-merged. The documented PR workflow with a second human contributor sits on the Arkad World client repo, not here.
- **No public application source.** This showcase is documentation. The npm packages are installable; `man-of-substance` is the readable SvelteKit + Supabase sample.
- **Cache invalidation is mixed.** Several Valkey paths invalidate on mutation; some still rely on TTL. Do not describe the cache layer as fully event-driven.
- **Mobile-only product screenshots.** Flagship Operations/Account/Staff captures are a phone viewport. Desktop recapture is outstanding; session wireframes cover desktop layout.
- **No Backroom screenshots.**
- **npm schema disclosure.** `@koloseum/types` and `@koloseum/utils` publish schema and role-matrix detail. Accepted for now; [`npm-publication-scope.md`](npm-publication-scope.md).
- **Credits are not a bank.** Non-withdrawable platform entitlements; licensed providers move funds. Prize and entry-fee behaviour needs counsel before launch (Betting, Lotteries and Gaming Act).
- **Timezones.** One civil-time interpretation (`Africa/Nairobi`) for the MVP. Lounges outside Kenya do not yet get local midnight.
