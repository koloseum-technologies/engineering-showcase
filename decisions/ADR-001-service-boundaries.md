# Architecture Decision Record: Service boundaries

**Status:** Accepted  
**Date:** 2023–2026 (platform-wide)

## Context

Koloseum serves four audiences (public visitors, Players, Lounges, Backroom) with different authentication requirements, visual shells, and scaling profiles (Player-facing traffic is expected to dominate, while Backroom scales with staff head count). A single application would tightly couple deployment cadence and points of failure.

## Decision

**Ship independently deployable SvelteKit apps per capability**, grouped by audience, **operating on one PostgreSQL database.** Share types, utils, and UI primitives through versioned npm packages. Put group ingress in the group's designated repo (usually `account`) and give public apps their own host.

**Do not split the database per service.** The interesting invariants (e.g. one Player, one Lounge attachment, money in cents, RLS) are cross-cutting. Splitting data would force distributed transactions that introduce unnecessary complexity to the platform's operations and the potential for data inconsistencies.

## Consequences

- Horizontal scaling and isolated deployments work as intended; a `players-sessions` outage does not take `players-account` with it.
- Cross-service consistency depends on package versioning and the schema, not on a service mesh.
- The organisation has many repositories. The established microservice implementation process has to carry the coherence a monolith would have got for free.
