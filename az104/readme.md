# AZ-104 - Manual de Estudio

> Manual organizado por categorías basado en preguntas reales de simulacros del examen **Microsoft Azure Administrator (AZ-104)**.
>
> El objetivo es disponer de una referencia rápida para estudiar la teoría más preguntada, eliminando duplicados y agrupando todos los conceptos por tecnología.

---

# Índice

## 1. Storage

📄 **[01-Storage.md](01-Storage.md)**

**Contenido**

- Storage Accounts
- Tipos de Storage
- Blob Storage
- Azure Files
- Queue Storage
- Table Storage
- Storage Explorer
- Storage Browser
- Storage Firewall
- Shared Access Signature (SAS)
- User Delegation SAS
- Service SAS
- Account SAS
- Stored Access Policy
- Blob Versioning
- Soft Delete
- Container Soft Delete
- Change Feed
- Snapshots
- Lifecycle Management
- Archive Tier
- Cool Tier
- Hot Tier
- Replication (LRS, ZRS, GRS, RA-GRS, GZRS)
- Encryption
- Encryption Scope
- Customer Managed Keys
- Infrastructure Encryption
- Private Endpoint
- Service Endpoint
- Routing Preference
- Storage Networking
- Storage Performance
- AzCopy

---

## 2. Azure Backup

📄 **[02-AzureBackup.md](02-AzureBackup.md)**

**Contenido**

- Recovery Services Vault
- Backup Vault
- Backup Policy
- Recovery Points
- Backup Retention
- Instant Restore
- Restore VM
- Restore Disk
- File Recovery
- Azure Backup Reports
- MARS Agent
- Azure VM Backup
- Soft Delete
- Multi-User Authorization (MAU)
- Resource Guard

---

## 3. Networking

📄 **[03-Networking.md](03-Networking.md)**

**Contenido**

- Virtual Network
- Address Spaces
- Subnets
- Reserved IPs
- Network Interface
- Public IP
- Private IP
- VNet Peering
- Global VNet Peering
- Gateway Transit
- User Defined Routes
- Route Tables
- Service Chaining
- IP Forwarding

---

## 4. Network Security Groups

📄 **[04-NSG.md](04-NSG.md)**

**Contenido**

- NSG
- Default Rules
- Priorities
- Inbound Rules
- Outbound Rules
- Application Security Groups
- Effective Security Rules
- IP Flow Verify
- NSG Flow Logs
- Traffic Analytics

---

## 5. Load Balancing

📄 **[05-LoadBalancer.md](05-LoadBalancer.md)**

**Contenido**

- Azure Load Balancer
- Basic vs Standard
- Internal Load Balancer
- Public Load Balancer
- Backend Pool
- Health Probe
- Load Balancing Rules
- Inbound NAT Rules
- Outbound Rules
- Application Gateway
- Web Application Firewall
- Traffic Manager

---

## 6. Azure Bastion

📄 **[06-Bastion.md](06-Bastion.md)**

**Contenido**

- Azure Bastion
- Bastion SKUs
- AzureBastionSubnet
- Native Client
- Standard SKU
- Public IP Requirements
- VNet Peering
- JIT

---

## 7. Azure Firewall

📄 **[07-Firewall.md](07-Firewall.md)**

**Contenido**

- Azure Firewall
- Firewall Policy
- DNAT
- Network Rules
- Application Rules
- Threat Intelligence
- AzureFirewallSubnet
- Routing Intent

---

## 8. Azure Container Registry

📄 **[08-ContainerRegistry.md](08-ContainerRegistry.md)**

**Contenido**

- Azure Container Registry
- Docker
- Login Server
- Docker Login
- Docker Tag
- Docker Push
- Docker Pull
- AcrPull
- AcrPush
- Content Trust
- Tasks
- Geo-Replication
- Private Endpoint

---

## 9. Containers

📄 **[09-Containers.md](09-Containers.md)**

**Contenido**

- Azure Container Instances
- Azure Container Apps
- Azure Kubernetes Service
- Windows Containers
- Linux Containers
- Container Groups

---

## 10. Microsoft Entra ID

📄 **[10-EntraID.md](10-EntraID.md)**

**Contenido**

- Users
- Groups
- Dynamic Groups
- Microsoft 365 Groups
- Security Groups
- Role Assignable Groups
- Guest Users
- Group-Based Licensing
- Self-Service Password Reset
- Authentication Methods
- Administrative Units
- Access Packages

---

## 11. Azure RBAC

📄 **[11-RBAC.md](11-RBAC.md)**

**Contenido**

- RBAC
- Built-in Roles
- Custom Roles
- Scope
- Inheritance
- Role Assignments
- Conditions
- IAM
- Least Privilege

---

## 12. Azure Policy

📄 **[12-AzurePolicy.md](12-AzurePolicy.md)**

**Contenido**

- Azure Policy
- Initiative
- Assignment
- Exemption
- Deny
- Audit
- Append
- Modify
- DeployIfNotExists

---

## 13. ARM Templates

📄 **[13-ARMTemplates.md](13-ARMTemplates.md)**

**Contenido**

- ARM Templates
- Parameters
- Variables
- Outputs
- copy
- copyIndex
- resourceId
- Incremental
- Complete
- Resource Group Deployment
- Subscription Deployment

---

## 14. Virtual Machines

📄 **[14-VirtualMachines.md](14-VirtualMachines.md)**

**Contenido**

- Virtual Machines
- Availability Sets
- Availability Zones
- Resize
- Redeploy
- Reapply
- Extensions
- Managed Disks
- Encryption
- Scale Sets
- VM Backup
- Quotas

---

## 15. Azure Monitor

📄 **[15-AzureMonitor.md](15-AzureMonitor.md)**

**Contenido**

- Azure Monitor
- Alerts
- Alert Rules
- Alert Processing Rules
- Metrics
- Activity Log
- Log Analytics
- Diagnostic Settings
- Data Collection Rules
- Action Groups
- Connection Monitor
- Network Insights

---

## 16. Azure App Service

📄 **[16-AppService.md](16-AppService.md)**

**Contenido**

- App Service
- App Service Plan
- Web Apps
- Runtime Stack
- Deployment Slots
- Autoscale
- Custom Domains
- Certificates
- Backup

---

## 17. DNS

📄 **[17-DNS.md](17-DNS.md)**

**Contenido**

- Azure DNS
- Private DNS Zones
- Public DNS Zones
- Auto-registration
- Resolution VNets
- Registration VNets
- Azure-provided DNS
- Custom DNS

---

## 18. VPN y ExpressRoute

📄 **[18-VPN.md](18-VPN.md)**

**Contenido**

- VPN Gateway
- GatewaySubnet
- Site-to-Site VPN
- Point-to-Site VPN
- VNet-to-VNet VPN
- ExpressRoute
- Gateway Transit
- BGP

---

## 19. Resource Management

📄 **[19-ResourceManagement.md](19-ResourceManagement.md)**

**Contenido**

- Management Groups
- Subscriptions
- Resource Groups
- Resources
- Resource Locks
- Tags
- Resource Providers
- Azure Resource Manager

---

## 20. Storage Permissions

📄 **[20-StoragePermissions.md](20-StoragePermissions.md)**

**Contenido**

- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner
- Storage File Data SMB Share Reader
- Storage File Data SMB Share Contributor
- Storage File Data SMB Share Elevated Contributor
- Reader
- Contributor
- Owner

---

# Orden recomendado de estudio

Se recomienda estudiar los temas en el siguiente orden:

1. Storage
2. Networking
3. Virtual Machines
4. Azure Backup
5. RBAC
6. Microsoft Entra ID
7. NSG
8. Load Balancer
9. Azure Monitor
10. App Service
11. Azure Policy
12. ARM Templates
13. Bastion
14. Azure Firewall
15. Azure Container Registry
16. Containers
17. DNS
18. VPN y ExpressRoute
19. Resource Management
20. Storage Permissions

---

# Objetivo

Este manual resume la teoría necesaria para responder correctamente a las preguntas más frecuentes del examen **AZ-104**, agrupando conceptos relacionados y eliminando duplicados presentes en los distintos simulacros.
