# Product Requirements

Derived from Master Blueprint v2.0 (16 November 2026). IDs are stable; reference them in
issues, tests and acceptance evidence.

## Target customers

| Profile | Size | Primary needs |
| --- | --- | --- |
| Small NGO | 20 to 100 employees | Microsoft 365, HR, asset tracking, IT support, controlled access |
| SME | 20 to 200 employees | ERP, inventory, procurement, employee management, workflows |
| Startup | 5 to 50 employees | Fast deployment, affordable infrastructure, onboarding automation |

## Functional requirements

| ID | Requirement | Module | Phase |
| --- | --- | --- | --- |
| FR-01 | Users sign in to every application with their Entra ID account (SSO). | Identity | 4 |
| FR-02 | Access to each application is controlled by CloudOps-* group membership. | Identity | 4 |
| FR-03 | Disabling a user in Entra ID removes their access to all applications. | Identity | 4 |
| FR-04 | Register hardware and software assets with warranty, license and lifecycle data. | Snipe-IT | 3 |
| FR-05 | Assign assets to employees and keep assignment history. | Snipe-IT | 3 |
| FR-06 | Generate QR asset labels and asset reports. | Snipe-IT | 3 |
| FR-07 | Maintain employee records, departments and organizational structure. | HRMS | 3 |
| FR-08 | Manage leave and attendance. | HRMS | 3 |
| FR-09 | Onboarding workflow: new hire in HRMS triggers Entra account, group membership and asset request. | HRMS + Graph | 4 |
| FR-10 | Offboarding workflow: leaver triggers account disable and asset return. | HRMS + Graph | 4 |
| FR-11 | Manage inventory, procurement and projects. | ERPNext | 3 |
| FR-12 | Users submit support requests; IT assigns, escalates and closes them with history. | Helpdesk | 3 |
| FR-13 | Support tickets can reference assets. | Helpdesk | 3 |

## Non-functional requirements

| ID | Category | Requirement | Verification |
| --- | --- | --- | --- |
| NFR-01 | Security | No database port reachable from the internet. | External port scan |
| NFR-02 | Security | SSH by key only, restricted source; password auth disabled. | `sshd -T`, NSG review |
| NFR-03 | Security | No secrets in Git; secrets in Key Vault or `.env` outside the repo. | Secret scanning in CI |
| NFR-04 | Security | All public endpoints served over HTTPS with valid certificates. | SSL Labs grade A |
| NFR-05 | Security | Least-privilege Azure RBAC; no Owner role for day-to-day work. | Role assignment export |
| NFR-06 | Reliability | Containers restart automatically and expose health checks. | `docker compose ps` |
| NFR-07 | Recoverability | RPO 24 h and RTO 4 h for the development environment (to be confirmed in Phase 8). | Timed restore test |
| NFR-08 | Recoverability | Full environment rebuildable from Terraform plus backups. | DR drill evidence |
| NFR-09 | Observability | Alert within 5 minutes on VM down, disk above 85 %, or app health failure. | Alert test |
| NFR-10 | Cost | Monthly Azure spend stays under the agreed budget, with alerts at 50 %, 80 %, 100 %. | Azure budget |
| NFR-11 | Maintainability | All infrastructure changes go through Terraform and pull requests. | Drift check |
| NFR-12 | Portability | Applications are deployable to a new tenant by configuration only. | Phase 9 prototype |

## Open items

- Monthly budget ceiling (NFR-10).
- Whether "Nexkors" is the organization/brand or a separate application to integrate with.
- Microsoft 365 and Entra ID license level for the Nexkors tenant (affects FR-01 to FR-03).
