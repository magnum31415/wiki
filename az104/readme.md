# AZ-104 - Índice de Teoría

> Manual organizado por categorías para preparar el examen **Microsoft Azure Administrator (AZ-104)**.

---

# Índice

## 1. Storage

- [Storage Accounts](01-Storage.md)
- Blob Storage
- Azure Files
- Table Storage
- Queue Storage
- Redundancy (LRS, ZRS, GRS, RA-GRS, GZRS)
- Lifecycle Management
- Blob Versioning
- Soft Delete
- Change Feed
- Encryption
- Storage Firewall
- SAS
- Access Keys
- Private Endpoint
- Service Endpoint
- Replication

---

## 2. Azure Backup

- [Azure Backup](02-AzureBackup.md)
- Recovery Services Vault
- Backup Vault
- Backup Policy
- File Recovery
- Restore VM
- Restore Files
- Azure Backup Reports
- MARS Agent
- Resource Guard (MAU)
- Soft Delete

---

## 3. Networking

- [Networking](03-Networking.md)
- Virtual Network (VNet)
- Subnets
- Address Spaces
- VNet Peering
- Global VNet Peering
- Service Chaining
- User Defined Routes (UDR)
- Route Tables
- Network Interface (NIC)
- Public IP
- Private IP

---

## 4. Network Security Groups

- [Network Security Groups (NSG)](04-NSG.md)
- NSG Rules
- Priorities
- Default Rules
- IP Flow Verify
- Effective Security Rules
- NSG Flow Logs
- Traffic Analytics

---

## 5. Load Balancing

- [Load Balancing](05-LoadBalancer.md)
- Azure Load Balancer
- Internal Load Balancer
- Standard Load Balancer
- Basic Load Balancer
- Health Probe
- Load Balancing Rules
- Inbound NAT Rules
- Application Gateway
- Web Application Firewall (WAF)
- Traffic Manager

---

## 6. Azure Bastion

- [Azure Bastion](06-Bastion.md)
- AzureBastionSubnet
- Native Client
- Standard SKU
- Public IP Requirements

---

## 7. Azure Firewall

- [Azure Firewall](07-Firewall.md)
- AzureFirewallSubnet
- Firewall Policy
- DNAT
- Network Rules
- Application Rules

---

## 8. Azure Container Registry

- [Azure Container Registry](08-ContainerRegistry.md)
- ACR Tasks
- Geo-replication
- Private Endpoint
- Dedicated Data Endpoint
- Content Trust
- Docker Push
- Docker Tag
- Login Server
- AcrPull
- AcrPush

---

## 9. Containers

- [Containers](09-Containers.md)
- Azure Container Instances
- Azure Container Apps
- Azure Kubernetes Service (AKS)
- Windows Containers
- Linux Containers

---

## 10. Microsoft Entra ID

- [Microsoft Entra ID](10-EntraID.md)
- Users
- Groups
- Dynamic Groups
- Group-Based Licensing
- SSPR
- External Collaboration
- Guest Users
- Access Packages

---

## 11. Azure RBAC

- [Azure RBAC](11-RBAC.md)
- Built-in Roles
- Role Assignments
- Scope
- Inheritance
- RBAC Conditions
- IAM

---

## 12. Azure Policy

- [Azure Policy](12-AzurePolicy.md)
- Deny
- Audit
- Append
- DeployIfNotExists
- Exclusions
- Assignments

---

## 13. ARM Templates

- [ARM Templates](13-ARMTemplates.md)
- resourceId()
- copy
- copyIndex()
- Incremental Mode
- Complete Mode
- Resource Group Deployment
- Subscription Deployment

---

## 14. Virtual Machines

- [Virtual Machines](14-VirtualMachines.md)
- Redeploy
- Reapply
- Resize
- Scale Sets
- Extensions
- Protected Settings
- vCPU Quotas
- Automatic Updates

---

## 15. Azure Monitor

- [Azure Monitor](15-AzureMonitor.md)
- Alerts
- Action Groups
- Alert Processing Rules
- Activity Log
- Metrics
- Log Analytics
- DCR
- Diagnostic Settings
- Connection Monitor
- Network Insights

---

## 16. App Service

- [Azure App Service](16-AppService.md)
- Web Apps
- App Service Plan
- Deployment Slots
- Custom Domains
- Autoscale

---

## 17. DNS

- [DNS](17-DNS.md)
- Private DNS Zones
- Auto-registration
- Custom DNS
- Azure-provided DNS
- Name Resolution

---

## 18. VPN y ExpressRoute

- [VPN & ExpressRoute](18-VPN.md)
- VPN Gateway
- ExpressRoute Gateway
- GatewaySubnet
- Site-to-Site VPN
- VNet-to-VNet VPN
- Point-to-Site VPN

---

## 19. Resource Management

- [Resource Management](19-ResourceManagement.md)
- Management Groups
- Resource Groups
- Resource Locks
- Resource Providers
- Tags

---

## 20. Storage Permissions

- [Storage Permissions](20-StoragePermissions.md)
- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner
- Storage File Data SMB Share Contributor
- Storage File Data SMB Share Reader
- Storage File Data SMB Share Elevated Contributor

---

# Recomendación de estudio

Orden recomendado para preparar el AZ-104:

1. Storage
2. Networking
3. Virtual Machines
4. Azure Backup
5. RBAC
6. Entra ID
7. NSG
8. Load Balancer
9. Azure Monitor
10. App Service
11. Azure Policy
12. ARM Templates
13. Bastion
14. Firewall
15. Container Registry
16. Containers
17. DNS
18. VPN
19. Resource Management
20. Storage Permissions
