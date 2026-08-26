# ADR-001 — Service boundaries

**Status:** Accepted  
**Date:** 2024–2026 (estate-wide)

## Context

Koloseum serves four audiences (public visitors, Players, Lounges, Backroom) with different auth requirements, visual shells and scaling profiles. Player-facing traffic is expected to dominate; Backroom scales with staff headcount. A single application would couple deploy cadence and failure domains.

## Decision

Ship **independently deployable SvelteKit apps** per capability, grouped by audience, in front of **one PostgreSQL database**. Share types, utils and UI primitives through versioned npm packages. Put group ingress in the group's account/compliance repo; give public apps their own host.

Do **not** split the database per service. The interesting invariants (one Player, one Lounge attachment, money in cents, RLS) are cross-cutting. Splitting data would force distributed transactions the team is not staffed to operate.

## Consequences

- Horizontal scaling and isolated deploys work as intended; a Sessions outage does not take Account with it.
- Cross-service consistency depends on package versioning and the schema, not on a service mesh.
- The organisation has many repositories. Process (the seven-step roadmap, README template, `.doks/` conventions) has to carry the coherence a monolith would have got for free.
- `players-fgc` and a speculative Backroom Competitions service were allowed to exist as placeholders and are now explicitly out of the MVP, which is the cost of easy repo creation.
