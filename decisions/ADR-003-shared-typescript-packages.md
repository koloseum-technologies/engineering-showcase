# ADR-003 — Shared TypeScript packages

**Status:** Accepted  
**Date:** 2025 (first published February 2025)

## Context

Eleven apps cannot each invent Player IDs, role checks, or a loading spinner. They also cannot all import from a private monorepo path if they are separately cloned repositories.

## Decision

Three public npm packages, versioned independently:

| Package | Role | Latest (this writing) |
| --- | --- | --- |
| `@koloseum/types` | Generated database types, schema contracts, client models, provider shapes | v0.4.4 |
| `@koloseum/utils` | Client, formatting, general, **platform role matrix**, server helpers | v0.3.16 |
| `@koloseum/components` | Svelte 5 SVG and feedback primitives | v0.2.3 |

Licence: `UNLICENSED` (company IP, no grant of use). Homepage and issue links point at this showcase because the package source repositories are private.

## Consequences

- Cross-repo upgrades are routine (`@koloseum/utils` bumps propagating the same day). That is the coordination cost, and it is visible in git history.
- Publishing types and the role matrix **discloses schema and unreleased Commerce/KLSM entries** to anyone on npm. That is a known, accepted trade-off until after the job search. See [`product/npm-publication-scope.md`](../product/npm-publication-scope.md).
- The packages are the only independently verifiable Koloseum code a stranger can install today.
