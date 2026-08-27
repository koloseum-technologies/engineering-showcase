# Data protection: Dormant PII in production database

The live production database on the Koloseum Supabase project still holds personal data captured while `public-auth` was serving traffic (currently offline). As part of its compliance obligations under Kenya's Data Protection Act 2019, Koloseum is registered as a data controller and processor with the [Office of the Data Protection Commissioner (ODPC)](https://www.odpc.go.ke/).

## What is held

As of August 2026:

| Store                        | Count                     | Contents                                                                             |
| ---------------------------- | ------------------------- | ------------------------------------------------------------------------------------ |
| `auth.users`                 | 83                        | Phone identities; no emails.                                                         |
| `compliance.players`         | 54 active, 0 soft-deleted | Names, phone numbers, birth dates, nationalities, ID verification artefacts.         |
| `compliance.lounges`         | 1 active, 0 soft-deleted  | Company names, phone numbers, email addresses, other registration details.           |
| `compliance.lounge_branches` | 1 active                  | Branch names, addresses, amenities, and contacts.                                    |
| Abandoned sign-ups           | 29                        | `auth.users` rows with no Player or Lounge registration. Phone numbers only at Auth. |

Three of the 54 registered Players are also Backroom superusers, while another one is also a Lounge superuser. 32 of 83 accounts signed in again more than a day after creation; 80 of 83 have a "last sign-in" timestamp recorded.

## Lawful basis and purpose

`public-auth` was opened to the public to allow people to register as Players or Lounges and establish a foundation for the platform's cold start. Age gating and identity verification are compliance controls, not optional analytics. Abandoned sign-ups never finished fulfilling that purpose.

## Who can reach it

Access to the production database is limited only to the project lead (Davy Kamanzi) and to the platform's own RLS policies. Edge Function secrets and other sensitive credentials remain in the project.

## Retention

- **Completed registrations (51 external Players + 1 Lounge):** retain while the platform may return to service, and in any case no longer than is necessary for a possible relaunch or a lawful request from the data subject. Review by end of Q1 2027; if `public-auth` (and the rest of the MVP) is still down, delete or anonymise external rows unless a documented relaunch date exists.
- **Internal Backroom accounts (3):** retain as operator accounts.
- **Abandoned sign-ups (29):** these people never completed registration. Delete or anonymise on the same Q1 2027 review at the latest.
- **ID verification artefacts:** treat as identity documents; do not export. Delete with the parent Player row.

## Security

- Production secrets are not committed. The `public-auth` README previously contained test-shaped credentials; those values have since been replaced with placeholders.
- No production PII is copied into this showcase. Screenshots are taken using seed data.
- Row-level security remains enabled on all tables.

## If a data subject asks

The platform already has data update request workflows and a Backroom Compliance microservice. While the app is down, requests can be handled directly against the production project, with the same "one open request at a time" rule the schema enforces.
