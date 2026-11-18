# ADR-004: Entra ID as the identity foundation

- **Status:** Accepted
- **Date:** 2026-11-16
- **Phase:** 1, 4

## Context

Each application ships its own user store. Without a central identity, onboarding and
offboarding means touching every app, and access reviews are impossible.

## Decision

Microsoft Entra ID (Nexkors tenant) is the single identity provider. Access is granted through the
groups CloudOps-Admins, CloudOps-IT, CloudOps-HR and CloudOps-Users. Applications use SSO
(OIDC or SAML) where supported. The unrelated CCS group is out of scope and must not be changed.

## Alternatives considered

| Option | Why not chosen |
| --- | --- |
| Keycloak / Authentik | Extra component to run and secure; no native Microsoft 365 link. |
| Per-app local accounts | No central control, no offboarding guarantee. |

## Consequences

- Group-based access maps cleanly to Azure RBAC and to app roles.
- Some SSO features may require Entra ID P1; confirm licensing before relying on them.
- Break-glass local admin accounts must exist per app and be documented.

## Implementation references

- `identity/` (Step 6)
