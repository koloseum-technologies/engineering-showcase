# Data protection position — dormant production PII

**Applies to:** the live Koloseum production Supabase project (`kesrafjetdhzervvkbdk`), which still holds personal data captured while `public-auth` was serving traffic. The application is not currently serving traffic. Kenya's Data Protection Act 2019 applies regardless.

This is an operational position, not a CV claim.

## What is held

Qualified 27 August 2026:

| Store | Count | Contents |
| --- | --- | --- |
| `auth.users` | 83 | Phone identities (no emails). Created 4 March 2025 – 5 October 2025. |
| `compliance.players` | 54 active, 0 soft-deleted | Names, phone (`NOT NULL`), birth dates, nationality, Smile ID verification artefacts. |
| `compliance.lounges` | 1 active, 0 soft-deleted | Company name, phone, email, registration details. |
| `compliance.lounge_branches` | 1 active | Primary branch created for that Lounge. |
| Abandoned sign-ups | 29 | `auth.users` rows with no Player or Lounge registration. Phone number only at Auth. |

Three of the 54 Player rows hold `backroom_superuser` (internal). One Player row is also the Lounge superuser.

32 of 83 accounts signed in again more than a day after creation; 80 of 83 have a `last_sign_in_at`.

## Lawful basis and purpose

Accounts were created so people could register as Players or Lounges on a competition platform. Identity verification (Smile ID) and age gating (13+ Players, 18+ Lounges) were compliance controls, not optional analytics. Abandoned sign-ups never finished that purpose.

## Who can reach it

Access is limited to people with production Supabase credentials for this project (currently the author as Head of Technology) and to the platform's own RLS policies on all 145 tables. The application that used to write this data is not serving traffic. Edge Function secrets remain in the project.

## Retention

- **Completed registrations (51 external Players + 1 Lounge):** retain while the platform may return to service, and in any case no longer than is necessary for a possible relaunch or a lawful request from the data subject. Review by **end of Q1 2027**; if the application is still down, delete or anonymise external rows unless a documented relaunch date exists.
- **Internal Backroom accounts (3):** retain as operator accounts.
- **Abandoned sign-ups (29):** these people never completed registration. Delete or anonymise on the same Q1 2027 review at the latest; sooner if no relaunch is planned.
- **Smile ID artefacts:** treat as identity documents. Do not export. Delete with the parent Player row.

## Security

- Production secrets are not committed. The `public-auth` README previously contained test-shaped credentials; those values have been replaced with placeholders. The Twilio token published there does not authenticate; the Twitch secret published there is rejected by Twitch.
- No production PII is copied into this showcase. Screenshots are from seeds.
- Row-level security remains enabled on all tables.

## If a data subject asks

The platform already has data-update request workflows in `compliance` and a Backroom compliance microservice. While the app is down, requests can be handled directly against the production project by the author, with the same “one open request at a time” discipline the schema enforces.

Author: Davy Kamanzi, August 2026.
