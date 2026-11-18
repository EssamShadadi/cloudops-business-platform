# Subnet Plan

VNet: `vnet-cloudops-dev-weu-01` · Address space `10.0.0.0/16` · Region West Europe

> **Status: unreconciled.** Two plans exist and they disagree. The deployed VNet is the
> source of truth. Run `az network vnet subnet list` (see below) and keep whichever
> plan matches, then delete the other section from this file.

## Plan A: repository table (August 2026)

Azure reserves 5 addresses per subnet, so each /24 has 251 usable IPs.

| Index | Subnet name | CIDR | Usable IPs | Purpose | Internet exposure |
| ----: | --- | --- | ---: | --- | --- |
| 01 | `snet-app` | `10.0.1.0/24` | 251 | Application workloads, Docker hosts, web/API services | Private |
| 02 | `snet-db` | `10.0.2.0/24` | 251 | Databases and data services | **No public access** |
| 03 | `snet-mgmt` | `10.0.3.0/24` | 251 | Administration, Bastion/jump-host functionality | Restricted |
| 04 | `snet-monitoring` | `10.0.4.0/24` | 251 | Prometheus, Grafana, monitoring components | Private |
| 05 | `snet-services` | `10.0.5.0/24` | 251 | Future shared services: Redis, queues, APIs | Private |
| 06 | `snet-gateway` | `10.0.6.0/24` | 251 | Application Gateway | Restricted |
| 07 | `snet-aks` | `10.0.7.0/24` | 251 | Future Kubernetes/AKS workloads | Private |
| 08 | `snet-private-endpoints` | `10.0.8.0/24` | 251 | Azure Private Endpoints | Private |
| 09 | `snet-backup` | `10.0.9.0/24` | 251 | Future backup/recovery services | Private |
| 10 | `snet-reserved` | `10.0.10.0/24` | 251 | Reserved for future architecture changes | N/A |

## Plan B: Master Blueprint v2.0, section 4.2

| Subnet | CIDR | Purpose |
| --- | --- | --- |
| Application Gateway | `10.0.1.0/24` | Dedicated AppGW subnet |
| Application | `10.0.2.0/24` | Application and container workloads |
| Database | `10.0.3.0/24` | Private database services |
| Management | `10.0.4.0/24` | Administrative access |
| Monitoring | `10.0.5.0/24` | Monitoring workloads, if separately hosted |

## Differences

| Purpose | Plan A | Plan B |
| --- | --- | --- |
| Application Gateway | `10.0.6.0/24` | `10.0.1.0/24` |
| Application | `10.0.1.0/24` | `10.0.2.0/24` |
| Database | `10.0.2.0/24` | `10.0.3.0/24` |
| Management | `10.0.3.0/24` | `10.0.4.0/24` |
| Monitoring | `10.0.4.0/24` | `10.0.5.0/24` |
| Extra subnets | services, aks, private-endpoints, backup, reserved | none |

## How to reconcile

```bash
az network vnet subnet list \
  --vnet-name vnet-cloudops-dev-weu-01 \
  --resource-group <rg-name> \
  --query "[].{name:name, prefix:addressPrefix, nsg:networkSecurityGroup.id}" -o table
```

Notes for the decision:

- Moving a subnet's prefix requires it to be empty, so whatever is deployed is cheapest to keep.
- Empty subnets cost nothing, but each one needs an NSG association and documentation, so
  creating `snet-aks`, `snet-backup` and `snet-reserved` now adds work without benefit. The
  address space can be reserved on paper instead.
- Application Gateway v2 needs a dedicated subnet; /24 is recommended for autoscaling headroom.
- While MariaDB runs in containers on the app VM, the database subnet stays empty. That is
  acceptable, but say so in the network documentation and NSG matrix.
