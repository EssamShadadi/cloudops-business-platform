# ADR-001: Docker Compose before Kubernetes

- **Status:** Accepted
- **Date:** 2026-11-16
- **Phase:** 2

## Context

The platform hosts four to five business applications for a small organization on a
development budget. Operating the platform, not only deploying it, is part of the learning
goal, and every component must justify its operational cost (Blueprint 5, technology selection rule).

## Decision

Run all workloads with Docker Compose on Ubuntu. Kubernetes (AKS) is an optional, later extension.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| AKS | Control plane free, but node pools, ingress controller, storage classes and upgrades add cost and complexity before any app works. |
| Azure Container Apps | Poor fit for stateful MariaDB and Frappe's multi-process bench; less transferable Linux learning. |
| Bare installs on the VM | No isolation, harder upgrades and rollback, nothing reusable. |

## Consequences

- Simple, cheap, easy to debug; skills transfer directly to Kubernetes later.
- No self-healing across hosts and no rolling updates; the VM is a single point of failure (see ADR-007).
- Health checks, restart policies and resource limits must be set explicitly in Compose.

## Implementation references

- `containers/compose/` (Step 4)
