# ADR-008: Frappe Helpdesk as the ITSM candidate

- **Status:** Proposed
- **Date:** 2026-11-16
- **Phase:** 3

## Context

The blueprint shortlists Zammad and GLPI. ERPNext and HRMS already run on the Frappe framework,
and the VM is sized for a small footprint.

## Decision

Evaluate Frappe Helpdesk first. It installs as an app on the same Frappe bench as ERPNext and
HRMS, sharing the database, users, roles and SSO configuration. Keep GLPI as the fallback if
classic ITSM features (CMDB, inventory agents) turn out to be required.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| Zammad | Requires Elasticsearch, PostgreSQL and Redis; roughly 4 GB extra RAM. |
| GLPI | Separate stack and user store; overlaps with Snipe-IT for asset inventory. |

## Consequences

- One fewer stack to back up, patch and integrate with Entra ID.
- Helpdesk tickets can link to ERPNext/HRMS records natively.
- Less mature than Zammad for omnichannel support.

## Implementation references

- `containers/frappe/` (Step 5)
