# Architecture Decision Record: Shared TypeScript packages

**Status:** Accepted  
**Date:** 2025

## Context

Multiple apps should not each invent Player IDs, role checks, or a loading spinner. They also cannot all import from a private monorepo path if they are separately cloned repositories.

## Decision

Publish three public npm packages, versioned independently:

| Package                | Role                                                                       | Latest (August 2026) |
| ---------------------- | -------------------------------------------------------------------------- | -------------------- |
| `@koloseum/types`      | Generated database types, schema contracts, client models, provider shapes | v0.4.4               |
| `@koloseum/utils`      | Client, formatting, general, **platform role matrix**, server helpers      | v0.3.16              |
| `@koloseum/components` | Svelte 5 SVG and feedback primitives                                       | v0.2.3               |

Licence: `UNLICENSED` (company IP, no grant of use). Homepage and issue links point to this showcase because the source repos are private.

## Consequences

- The coordination cost of shared packages is routine cross-repo upgrades (e.g. `@koloseum/utils` bumps propagating the same day).
- Publishing types and the role matrix discloses the schema to anyone on npm. That is currently a known, accepted trade-off; see [`product/npm-publication-scope.md`](../product/npm-publication-scope.md) for more details.
- The packages are the only independently verifiable Koloseum code a stranger can install today.
