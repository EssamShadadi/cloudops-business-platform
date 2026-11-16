# ADR-003: Separate application and database subnets

- **Status:** Accepted
- **Date:** 2026-11-16
- **Phase:** 1

## Context

Databases hold HR and asset data and must never be reachable from the internet. Network
segmentation lets NSGs enforce that independently of application configuration.

## Decision

Reserve dedicated application, database and management subnets in `10.0.0.0/16`, each with its
own NSG. Exact prefixes are pending reconciliation (see [subnet plan](../networking/subnet-plan.md)).

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| One flat subnet | No network boundary between tiers. |
| Private Endpoints only | Only applies to PaaS databases; not yet in use. |

## Consequences

- While MariaDB runs as containers on the app VM (ADR-007), the database subnet is empty and
  isolation is provided by Docker internal networks. This must be documented honestly.
- The subnet becomes active if databases move to a separate VM or Azure Database for MySQL Flexible Server.

## Implementation references

- `architecture/networking/subnet-plan.md`
