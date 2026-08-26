# Case study — Lounges Account

Lounges Account is where a gaming Lounge manages company identity, Credits, payment methods, entitlements and ownership transfer. It is a completed MVP service, used here as a slice through the whole stack: schema, RLS, payments, caching, E2E and Kubernetes.

## Who it is for

- **Superuser** — everything
- **Manager** — Finances only
- Branch-scoped roles — no access

Unauthenticated users are sent to `public-auth`. Authorisation is `Instance.isUserAuthorised` with `microserviceGroup: "lounges"`.

## Company identity

Business registration number and country are read-only, mirrored from the verified company document. Company name, date of registration and trade name are **sensitive**: a change opens a Backroom data-update request (JPEG or PDF, ≤ 2 MB), one open request at a time, one trade-name change per year — enforced in the database, not in a form rule. Contacts (phone, email, website) save immediately and write an audit row.

## Money

Credits are a ledger, not a wallet. Minimum top-up Ksh 50; Lounge caps Ksh 30,000 per top-up and Ksh 100,000/day, **enforced in SQL in cents**. Pay with a saved card or hosted checkout (Paystack / Flutterwave, including mobile money). Returning with `?topup=processing` bypasses cache so a settled top-up is not hidden by a stale read.

Saved cards can be listed, promoted to default, removed. Auto top-up on the default card covers a shortfall rather than failing a charge; the trigger talks to an Edge Function via `pg_net` with Vault secrets.

## Entitlements

Usage for the period (Sessions time, estimated rolling charges, renewals). A card per subscription — extra branch slots, eatery add-on — with status, price and renewal. Free Backroom grants activate from the card. First branch is free; extras are priced monthly/quarterly/yearly in Credits.

## Ownership

Transfer of the Superuser role is a workflow with statuses (`pending` → `approved` / `rejected` / `completed` / `cancelled`), not a column flip.

## Engineering notes

- **Cache:** tenant-scoped Valkey keys; the top-up return path is the example of “never let TTL hide a mutation”.
- **Uploads:** ConfigMap `BODY_SIZE_LIMIT=4M` so adapter-node's 512 KB default cannot 413 a valid 2 MB document.
- **Ingress:** this repo owns the shared Lounges group ingress; Operations and Staff add paths here rather than shipping their own.
- **E2E:** Playwright + Supawright, seeded users, optional `E2E_PAYMENTS_MOCK=1` against `paystack` with `PAYSTACK_MOCK_SETTLEMENT=true`. Known issues live in `e2e/KNOWN_ISSUES.md`.
- **UI:** screenshots in [`screenshots/lounges/account/`](../screenshots/lounges/account/) (seeded “Kinyanjui Gaming Ltd” / “KG Arena”).
