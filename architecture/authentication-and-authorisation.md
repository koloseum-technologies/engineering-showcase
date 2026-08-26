# Authentication and authorisation

## Public authentication (`public-auth`)

The only Koloseum service that has met real users. It is not currently serving traffic; the production database it wrote to is still live.

Capabilities:

- Phone login with SMS OTP (Twilio)
- Password login
- TOTP (authenticator app) as a second factor when enrolled
- Age-gated registration: Players 13+, Lounges 18+
- Smile ID verification during registration
- Separate **Player** (individual) and **Lounge** (company + primary branch + superuser) paths
- Self-service password reset

Qualified production funnel (March–October 2025): 83 phone sign-ups → 54 completed Player registrations (65%). Three of those 54 are internal Backroom accounts; excluding them, 51 of 80 external sign-ups completed (64%). One Lounge registered and created a branch. See [`product/status.md`](../product/status.md).

## Authorisation after login

SSO: after login the user chooses Player-facing (default `players-competitions`), Lounge-facing or Backroom-facing apps. Default landing is configurable in the Account services.

- **RLS on all 145 tables.** Deny-by-default is the baseline; policies grant by `auth.uid()`, Lounge membership and Backroom role.
- **Role and feature gates** in `@koloseum/utils/platform`, consumed by each app. Lounges additionally require `Instance.isUserAuthorised` with `microserviceGroup: "lounges"`.
- **Sensitive profile fields** (company name, registration date, trade name) do not UPDATE in place: they open a data-update request for Backroom review, with document upload, one open request at a time, and a schema cap of one trade-name change per year.

## Contrast with `man-of-substance` (deliberate)

The public album app logs contributors in by email with a **single shared server-held password**. Knowing a backer's email reaches their rewards. For a 2022 campaign whose blast radius was music downloads, that was a defensible trade-off. `public-auth` is the later answer to a different threat model: per-user credentials, age gating, third-party identity verification, company accounts. Same author, same stack, two threat models, two designs.

Supabase SSR detail that is easy to get wrong, and is done correctly in the public sample: `hooks.server.ts` validates the JWT with `getUser()` (`safeGetSession`) rather than trusting `getSession()`; `+layout.ts` then explains why a subsequent `getSession()` on the client is safe because the server already validated it.
