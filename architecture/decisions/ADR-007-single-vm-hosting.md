# ADR-007: Initial single-VM application hosting

- **Status:** Accepted
- **Date:** 2026-11-16
- **Phase:** 2, 3

## Context

Resource needs of the application set are not yet measured. Rough estimates: Frappe bench with
ERPNext + HRMS + Helpdesk about 4 GB RAM, Snipe-IT about 1 GB, MariaDB instances and proxy
about 2 GB.

## Decision

Host all containers on one Ubuntu LTS VM in the application subnet, sized B4ms (4 vCPU, 16 GB)
or similar, with a separate managed data disk for Docker volumes and an auto-shutdown schedule.
Resize after measuring.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| One VM per app | Multiplies cost and patching for a dev environment. |
| Smaller VM (B2ms, 8 GB) | Likely memory pressure with Frappe and two MariaDB instances. |

## Consequences

- Single point of failure; acceptable for development and documented as such (Blueprint 7.3).
- Recovery depends on tested backups and a scripted rebuild (Step 9 DR drill).

## Implementation references

- `infrastructure/terraform/` VM module (Step 3)
