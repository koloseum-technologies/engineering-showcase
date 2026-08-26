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
                    SuprSend / GavaConnect (eTIMS) / IGDB / Challonge / Zoho
```

**Players** are individuals. They register once, then may become organisers or be attached to a Lounge or to Backroom. A Player can be attached to only one Lounge at a time.

**Lounges** are company accounts. The person who registers a Lounge is its Superuser. Staff are Players attached to that Lounge.

**Backroom** is Koloseum staff. They are Players who have been assigned Backroom roles.

**Koloseum is not a financial institution.** Credits are a prepaid platform-entitlement ledger; licensed providers move money where Koloseum is in the payment path. Competition entry and spectator tickets in the MVP go Player → organiser off-platform. Prize Fund verification uses organiser deposits, not Credits.

Kenya is the default market: amounts stored in cents, currency KES, database timezone `Africa/Nairobi`.

The interesting constraint is not “many user types”. It is that the same person can wear several hats, the data model has to enforce that without duplicating people, and money, identity and competition integrity cannot be faked in the application layer.
