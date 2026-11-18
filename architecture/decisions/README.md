# Architecture Decision Records

Each ADR records one decision: context, decision, alternatives, consequences, status and
implementation references (Master Blueprint, section 8.2). Copy [`template.md`](template.md)
for new records. Superseded ADRs stay in place with their status changed.

| ADR | Title | Status |
| --- | --- | --- |
| [001](ADR-001-docker-compose-before-kubernetes.md) | Docker Compose before Kubernetes | Accepted |
| [002](ADR-002-azure-primary-cloud.md) | Azure as the primary cloud platform | Accepted |
| [003](ADR-003-separate-app-and-db-subnets.md) | Separate application and database subnets | Accepted |
| [004](ADR-004-entra-id-identity-foundation.md) | Entra ID as the identity foundation | Accepted |
| [005](ADR-005-terraform-for-infrastructure.md) | Terraform for infrastructure provisioning, adopted from Phase 1 | Proposed |
| [006](ADR-006-application-gateway-entry-point.md) | Application Gateway as the external entry point | Proposed (deferred by ADR-009) |
| [007](ADR-007-single-vm-hosting.md) | Initial single-VM application hosting | Accepted |
| [008](ADR-008-helpdesk-selection.md) | Frappe Helpdesk as the ITSM candidate | Proposed |
| [009](ADR-009-reverse-proxy-before-appgw.md) | Caddy reverse proxy before Application Gateway | Proposed |

Status values: Proposed, Accepted, Superseded, Rejected.
