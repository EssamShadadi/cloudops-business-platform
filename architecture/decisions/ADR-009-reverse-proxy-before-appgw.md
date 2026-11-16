# ADR-009: Caddy reverse proxy before Application Gateway

- **Status:** Proposed
- **Date:** 2026-11-16
- **Phase:** 2

## Context

Application Gateway v2 has a fixed hourly charge while deployed, independent of traffic. During
Phases 2 to 8 the platform has no real users, and the VM is shut down outside working hours.

## Decision

Expose the VM on 80/443 through an NSG and terminate TLS with Caddy (automatic Let's Encrypt
certificates) for `*.nexkors.com`. Deploy Application Gateway + WAF later via Terraform for a
time-boxed demonstration, capture evidence, then destroy it unless real traffic justifies it.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| Application Gateway from day one | Ongoing cost with no traffic. |
| Nginx + certbot | Works; Caddy needs less configuration for automatic TLS. Either is acceptable. |
| Cloudflare proxy in front | Adds a third-party dependency outside the Azure learning scope. |

## Consequences

- No managed WAF in the interim; mitigated by NSG, Fail2Ban, and SSO in front of apps.
- Port 22 must stay restricted to a single admin IP or Entra-based SSH.

## Implementation references

- `containers/reverse-proxy/` (Step 4)
