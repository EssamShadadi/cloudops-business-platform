# CloudOps Network Foundation

## Environment

Development

## Azure Region

West US

## Virtual Network

Name:
vnet-cloudops-dev-weu-01

Address Space:
10.0.0.0/16

## Subnets

| Name                      | CIDR        | Purpose                |
|---------------------------|-------------|------------------------|
| ApplicationGatewaySubnet  | 10.0.1.0/24 | Application Gateway    |
| ApplicationSubnet         | 10.0.2.0/24 | Application workloads  |
| DatabaseSubnet            | 10.0.3.0/24 | Database workloads     |
| ManagementSubnet          | 10.0.4.0/24 | Administrative access  |
| MonitoringSubnet          | 10.0.5.0/24 | Self-hosted monitoring |

The network uses a /16 address space to provide sufficient room
for future expansion while initially segmenting workloads into
dedicated /24 subnets.

The Application Gateway is isolated in its own subnet.
Application workloads are separated from databases.
Management traffic is separated from application traffic.