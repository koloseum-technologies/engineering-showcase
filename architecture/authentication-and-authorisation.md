# Authentication and authorisation

## Public Authentication (`public-auth`)

The only Koloseum service that has met real users. It is not currently serving traffic, but the production database it wrote to is still live. Its capabilities include:

- Phone login with password or SMS OTP (Twilio)
- TOTP (authenticator app) as a second factor when enrolled
- Age-gated registration: Players 5+ (ages 5-17 require a registered representative), Lounges 18+
- ID verification (Smile ID)
- Separate **Player** (individual) and **Lounge** (company + superuser + primary branch) paths
- Self-service password reset

Qualified production funnel (March–October 2025): 83 phone sign-ups → 54 completed Player registrations (65%). Three of those 54 are internal Backroom accounts; excluding them, 51 of 80 external sign-ups completed (64%). One Lounge registered and created a branch. See [`product/status.md`](../product/status.md) for more info.

## Authorisation after login

SSO: after login the user chooses Player-facing (default `players-competitions`) or Lounge-facing apps. Default landing is configurable in `players-account`.

- **RLS on all tables.** Deny-by-default is the baseline; policies grant by `auth.uid()`, Lounge membership, and Backroom role.
- **Role and feature gates** in `@koloseum/utils/platform`, consumed by each app. Lounges additionally require `Instance.isUserAuthorised` with `microserviceGroup: "lounges"`.
- **Sensitive profile fields** do not update in place: they open a data update request for Backroom review (one open request at a time), with document upload and schema caps on certain changes.
