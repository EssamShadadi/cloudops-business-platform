# ADR-002: Azure as the primary cloud platform

- **Status:** Accepted
- **Date:** 2026-11-16
- **Phase:** 0

## Context

Target customers (NGOs, SMEs, startups) commonly run Microsoft 365. Identity, Graph API
integration and SSO are core platform features, and the project's learning goal is Azure.

## Decision

Use Microsoft Azure for all infrastructure. Development region: West Europe.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| AWS / GCP | Weaker native fit with Entra ID and Microsoft 365; not the learning target. |
| On-premises / VPS | No cloud engineering evidence; no managed identity, monitoring or IaC providers of the same depth. |

## Consequences

- Entra ID, Key Vault, Monitor and Graph integrate natively.
- Some services (Application Gateway, Bastion, Firewall) have fixed hourly costs that must be justified (ADR-009).

## Implementation references

- Resource group and VNet `vnet-cloudops-dev-weu-01`
