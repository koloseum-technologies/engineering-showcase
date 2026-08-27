# Case study: Lounges Account

Lounges Account (`lounges-account`) is where a gaming Lounge manages company identity, finances (Credits, payment methods, entitlements), and ownership transfer. It is a completed MVP service, used here to peek into the whole stack: schema, RLS, payments, caching, E2E and Kubernetes.

## Who it is for

- **Superuser:** everything
- **Manager:** finances only
- Branch-scoped roles: no access

Unauthenticated users are redirected to `public-auth`. Authorisation is handled by `Instance.isUserAuthorised` (a helper from the shared `@koloseum/utils` package) with `microserviceGroup: "lounges"`.

## Company identity

Business registration number and country are read-only, mirrored from the verified company document. Company name, date of registration and trade name are **sensitive**: an attempted change opens a data update request for Backroom to review, with allowances of one open request at a time and one trade name change per year; these are enforced in the database. Contacts (phone, email, website) save immediately and changes are logged to an audit row.

## Money

Koloseum Credits are a ledger of entitlements, not a wallet. Minimum top-up is Ksh 50, with Lounges capped at Ksh 30,000 per top-up and Ksh 100,000 per day; these are also enforced in the database. Users may pay with a saved card or hosted checkout (using Paystack by default, including mobile money). Returning to the top-up page with `?topup=processing` bypasses the cache so a settled top-up is not hidden by a stale read.

Saved cards can be set as default or removed. Users may enable auto top-up on the default card to cover a shortfall rather than failing a charge; the database trigger talks to an Edge Function via `pg_net` with Vault secrets.

## Entitlements

Users can monitor their Lounge's platform usage for the current period, including Sessions time (by Players), estimated rolling charges, and renewals due. Each subscription (extra branch slots or eatery add-on for a branch), including free Backroom grants, is represented as a card with status, price, and renewal details. The first branch is always free, while extras are priced monthly, quarterly, or yearly in Credits.

## Ownership

Users can request a transfer of their Superuser role for the Lounge; the request status (`pending` to `approved`, `rejected`, `completed`, or `cancelled`) changes as per the outcome of the Backroom review.

## Engineering notes

- **Cache:** tenant-scoped Valkey keys; the top-up return path is an example of "never let TTL hide a mutation".
- **Uploads:** ConfigMap `BODY_SIZE_LIMIT=4M` so `adapter-node`'s 512 KB default doesn't block a valid 2 MB document.
- **Ingress:** this repo owns the shared Lounges group ingress; Operations and Staff add paths here rather than shipping their own.
- **E2E:** Playwright + Supawright, seeded users, optional `E2E_PAYMENTS_MOCK=1` against `paystack` with `PAYSTACK_MOCK_SETTLEMENT=true`. Known issues live in `e2e/KNOWN_ISSUES.md`.
- **UI:** See [`screenshots/lounges/account/`](../screenshots/lounges/account/) for seeded samples.
