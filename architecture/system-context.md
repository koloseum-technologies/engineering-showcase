# System context

Koloseum sits between three groups of people and a small set of licensed external providers.

```text
 Players ──┐
           ├── public-auth (SSO) ──► Player apps | Lounge apps | Backroom apps
 Organisers┤                              │
           │                              ▼
 Lounges ──┘                         PostgreSQL + RLS
                                          │
                    Paystack / Flutterwave / Smile ID / Twilio /
                    SuprSend / GavaConnect (eTIMS) / IGDB / Zoho
```

**Players** are individuals. They register once, then may become organisers and/or be attached to a Lounge or to Backroom. A Player can be attached to only one Lounge at a time.

**Lounges** are company accounts. The person who registers a Lounge is its Superuser. Staff (including Superusers) are Players attached to that Lounge.

**Backroom** refers to Koloseum staff. They are Players who have been assigned Backroom roles.

**Koloseum is not a financial institution.** Credits are a prepaid platform entitlement ledger; licensed providers move money where Koloseum is in the payment path. Competition entry, spectator tickets, and Lounge session charges in the MVP are paid off-platform to organisers/Lounge branches as appropriate. Competition organisers deposit prize pools directly rather than using Credits.

**Kenya is the default market**, with amounts stored in cents (KES) and the database timezone set to `Africa/Nairobi`.

Since the same person can wear several hats, the data model has to enforce that without duplicating people. Identity, money, and competition integrity cannot be faked in the application layer.
