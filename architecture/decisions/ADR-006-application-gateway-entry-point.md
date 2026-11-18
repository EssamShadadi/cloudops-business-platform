# ADR-006: Application Gateway as the external entry point

- **Status:** Proposed (deferred by ADR-009)
- **Date:** 2026-11-16
- **Phase:** 1, 10

## Context

The target architecture puts Azure Application Gateway (v2, optionally with WAF) in front of all
applications for TLS termination, host-based routing and web application firewall protection.

## Decision

Application Gateway remains the target public entry point. Deployment is deferred until the
applications run behind a reverse proxy (ADR-009), then added via Terraform in its dedicated subnet.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| Azure Front Door | Global edge features not needed for one region. |
| Public IP on VM with reverse proxy only | Chosen as the interim step (ADR-009), lacks managed WAF. |

## Consequences

- When added, AppGW and Caddy overlap; Caddy then serves only internal routing and AppGW holds the certificates.
- AppGW v2 bills per hour while deployed, even without traffic.

## Implementation references

- Dedicated subnet in `architecture/networking/subnet-plan.md`
