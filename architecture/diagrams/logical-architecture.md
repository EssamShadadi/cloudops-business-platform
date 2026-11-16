# Logical Architecture

Status: **Design target.** Nothing below the Entra ID layer is deployed yet; see the
status table in the [README](../../README.md).

## Request path and layers

```mermaid
flowchart TD
    U[Users] --> E[Microsoft Entra ID<br/>authentication, SSO, groups]
    U --> EDGE

    subgraph EDGE[Edge]
        direction TB
        P["Reverse proxy on VM (Caddy, TLS for *.nexkors.com)<br/>later: Azure Application Gateway + WAF"]
    end

    EDGE --> VNET

    subgraph VNET["vnet-cloudops-dev-weu-01 (10.0.0.0/16)"]
        direction TB
        subgraph APP[Application layer]
            SIT[Snipe-IT<br/>assets]
            subgraph BENCH[Frappe bench]
                ERP[ERPNext<br/>ERP]
                HR[Frappe HRMS<br/>HR]
                HD[Helpdesk<br/>ITSM]
            end
        end
        subgraph DATA[Data layer]
            DB1[(MariaDB<br/>Snipe-IT)]
            DB2[(MariaDB<br/>Frappe)]
        end
        SIT --> DB1
        BENCH --> DB2
    end

    APP -. metrics / logs .-> MON[Azure Monitor + Log Analytics<br/>optional Prometheus/Grafana]
    DATA -. dumps / volumes .-> BK[Backup: Azure Blob Storage<br/>restore-tested]
    E -. OIDC / SAML .-> APP
```

## Layer summary

| Layer | Components | Phase |
| --- | --- | --- |
| Identity | Entra ID, CloudOps-* groups, app registrations | 1, 4 |
| Edge | Caddy on VM now; Application Gateway + WAF as a later, time-boxed extension (ADR-006, ADR-009) | 2, 10 |
| Network | VNet, subnets, NSGs (see [subnet plan](../networking/subnet-plan.md)) | 1 |
| Application | Docker Compose on one Ubuntu VM (ADR-001, ADR-007) | 2, 3 |
| Data | MariaDB containers with private networks only | 3 |
| Monitoring | Azure Monitor Agent, Log Analytics, alerts | 7 (baseline in 2) |
| Backup | DB dumps and volume archives to Blob, restore tests | 8 (baseline in 2) |

The original ASCII sketches (August 2026) are kept in Git history.
