# ADR-005: Terraform for infrastructure provisioning, adopted from Phase 1

- **Status:** Proposed
- **Date:** 2026-11-16
- **Phase:** 1, 6

## Context

The blueprint schedules Terraform for Phase 6, after the foundation, apps and CI/CD are built.
Every resource created by hand before then must later be imported and reconciled, which is the
riskiest part of adopting IaC. Only a resource group and a VNet exist today.

## Decision

Adopt Terraform (azurerm provider) now. Import the existing resource group and VNet with `import`
blocks; create every new resource as code. State lives in a dedicated Azure Storage account with
blob versioning and lease-based locking. The Phase 6 learning goals (modules, environments,
lifecycle) remain, applied to code that already exists.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| Bicep | Azure-only; Terraform is more transferable for a portfolio. |
| Portal / az CLI until Phase 6 | Large import and drift problem later. |

## Consequences

- Portal changes after adoption cause drift; the rule becomes "change it in code".
- The state storage account is bootstrapped once with a script (chicken-and-egg).
- CI can run `terraform plan` on every PR from the start of Phase 5.

## Implementation references

- `infrastructure/terraform/` (Step 3)
