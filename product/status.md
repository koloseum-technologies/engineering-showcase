# Product status

Koloseum is an **unreleased MVP**. The Players Competitions microservice is the product core and is currently in active development. Players Sessions and Lounges microservices are supporting infrastructure that were built first due to shifting priorities in the earlier stages of the project. See [`screenshots/`](../screenshots/) for UI previews of completed services.

## Implementation

| Service                  | Current version | Status                                  |
| ------------------------ | --------------- | --------------------------------------- |
| Public Authentication    | _v0.4.1_        | Shipped; not currently serving traffic  |
| **Players Competitions** | N/A             | **In active development**               |
| Players Sessions         | _v0.2.2_        | Completed                               |
| Players Account          | N/A             | Backlog                                 |
| Lounges Operations       | _v0.1.1_        | Completed                               |
| Lounges Staff            | _v0.1.0_        | Completed                               |
| Lounges Account          | _v0.1.1_        | Completed                               |
| Backroom Compliance      | _v0.1.14_       | Backlog (shipped; awaiting MVP updates) |
| Backroom Staff           | N/A             | Backlog                                 |
| Public Legal             | _v0.1.3_        | Backlog (shipped; awaiting MVP updates) |
| Public Help              | N/A             | Backlog                                 |
| Public Landing           | N/A             | Backlog                                 |

## Production funnel

Operating window: **Q1 to Q4 2025**. All 83 users registered with their phone number, which is the platform default. The flow includes ID verification and age gating.

|                                | Raw                 | After excluding Backroom superusers |
| ------------------------------ | ------------------- | ----------------------------------- |
| Sign-ups                       | 83                  | 80                                  |
| Completed Player registrations | 54 (0 soft-deleted) | 51                                  |
| Abandoned (no Player row)      | 29                  | 29                                  |
| Completion rate                | 65%                 | 64%                                 |

**Lounge path:** 1 registered Lounge (still active), with 1 branch. The Lounge superuser also has a Player row.

**Return visits:** 80 of 83 ever signed in; 32 signed in more than a day after signup.

**Monthly shape:** 39 in March (20 completed), then a quiet April–May, a second pulse in July–September, and final sign-ups in October.

## Data while apps are offline

The production database is still live. Details on retention and access can be found in [`data-protection.md`](data-protection.md).
