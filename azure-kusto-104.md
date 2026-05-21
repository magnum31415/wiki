[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

Índex

- [Kusto (KQL) Basics for Azure Resource Graph](#kusto-kql-basics-for-azure-resource-graph)
- [🔐 Public IP Security Audit - Azure Resource Graph Queries](#-public-ip-security-audit---azure-resource-graph-queries)
- [Azure Landing Zone – Connectivity Discovery Queries](#azure-landing-zone--connectivity-discovery-queries)

---
# Kusto (KQL) Basics for Azure Resource Graph

## 🧠 What is Kusto Query Language (KQL)?

Kusto Query Language (KQL) is the query language used to explore and analyze data in Azure services such as:

- Azure Resource Graph (ARG)
- Log Analytics
- Azure Monitor

In this context, we use KQL in **Azure Resource Graph** to query Azure resources across subscriptions.

---

## 🧱 Basic Structure of a KQL Query

A KQL query is built as a pipeline of operations:

```kusto
Resources
| where type == "microsoft.compute/virtualmachines"
| project name, location
```

### Key Concepts:

- **Resources** → Main table (all Azure resources)
- **| (pipe)** → Pass results to the next operation
- **where** → Filter data
- **project** → Select columns
- **extend** → Create calculated fields
- **summarize** → Aggregate data
- **mv-expand** → Expand arrays

---

## 🎯 Goal of These Queries

These queries are designed to:

- Analyze **security exposure** (public IPs)
- Understand **infrastructure inventory**
- Detect **cost optimization opportunities**
- Identify **architecture dependencies** (paired regions, DR)
- Support **Landing Zone governance decisions**

---

# 🔐 Public IP Security Audit - Azure Resource Graph Queries

## 1. Public IP Inventory

Lists all Public IP resources.

👉 Purpose:
- Identify internet-facing endpoints
- Starting point for exposure analysis

```kusto
Resources
| where type == 'microsoft.network/publicipaddresses'
| project name, location, sku.name, properties.ipAddress
```

---

## 2. Public IPs Attached to Virtual Machines

Finds public IPs directly linked to VM NICs.

👉 Risk:
- Direct inbound access to compute

```kusto
Resources
| where type == 'microsoft.network/networkinterfaces'
| mv-expand ipconfig = properties.ipConfigurations
| where isnotempty(ipconfig.properties.publicIPAddress)
| project vm = properties.virtualMachine.id, publicIp = ipconfig.properties.publicIPAddress.id
```

---

## 3. Public IPs with NSG Protection

Checks if NICs have NSG protection.

👉 Purpose:
- Validate security controls

```kusto
Resources
| where type == 'microsoft.network/networkinterfaces'
| project nicId = id, nsg = properties.networkSecurityGroup.id, vm = properties.virtualMachine.id
```

---

## 4. Internet-facing VMs without NSGs

Detects VMs without NSG protection.

👉 High-risk scenario

```kusto
Resources
| where type == 'microsoft.compute/virtualmachines'
| where isempty(properties.networkProfile.networkInterfaces[*].properties.networkSecurityGroup)
```

---

## 5. Public IP SKU (Basic vs Standard)

Counts Public IP types.

👉 Purpose:
- Identify insecure legacy SKUs

```kusto
Resources
| where type == 'microsoft.network/publicipaddresses'
| summarize count() by tostring(sku.name)
```

---

# 💰 Licensing & Cost Optimization (Azure Hybrid Benefit)

## 1. Windows Virtual Machines (Azure Hybrid Benefit)

### 📖 Official Documentation
https://learn.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing

### 🔍 Kusto Query

```kusto
resources
| where type == "microsoft.compute/virtualmachines"
| extend license = tostring(properties.licenseType)
| where license == "Windows_Server"
| project name, resourceGroup, subscriptionId, license
```

### 🧠 Explanation

Azure Hybrid Benefit (AHB) for Windows VMs allows you to reuse on-premises Windows Server licenses.

👉 To have AHB **enabled**, the field must be:

```text
licenseType = "Windows_Server"
```

### ⚠️ Important Notes

- If `licenseType` is empty → AHB is **NOT enabled**
- This applies only to Windows VMs
- Helps reduce compute costs significantly

---

## 2. Azure SQL (Azure Hybrid Benefit)

### 📖 Official Documentation
https://learn.microsoft.com/en-us/azure/azure-sql/azure-hybrid-benefit?view=azuresql&tabs=azure-portal

### 🔍 Kusto Query

```kusto
resources
| where type in ("microsoft.sql/servers/databases", "microsoft.sql/managedinstances")
| extend licenseType = tostring(properties.licenseType)
| project name, type, resourceGroup, subscriptionId, licenseType
```

### 🧠 Explanation

Azure Hybrid Benefit for SQL allows reuse of SQL Server licenses.

👉 To have AHB **enabled**, the field must be:

```text
licenseType = "BasePrice"
```

### ⚠️ Important Notes

- `BasePrice` means you are bringing your own license (AHB enabled)
- If empty → full Azure license cost applies
- Applies to:
  - Azure SQL Databases
  - Managed Instances

---

# 🧠 Summary

| Service        | Field           | Value Required     | Meaning                     |
|---------------|----------------|------------------|-----------------------------|
| Windows VM     | licenseType    | Windows_Server   | AHB enabled                 |
| Azure SQL      | licenseType    | BasePrice        | AHB enabled                 |

---

# 🎯 Why This Matters

- Identifies **cost optimization opportunities**
- Detects **misconfigured licensing**
- Supports **FinOps and governance strategies**

---

# 🖥️ VM Inventory & Configuration

## 8. OS Distribution

```kusto
resources
| where type == "microsoft.compute/virtualmachines"
| extend osType = tostring(properties.storageProfile.osDisk.osType)
| summarize count() by osType
```

👉 Purpose:
- Understand workload distribution

---

## 9. Detailed VM Inventory

```kusto
resources
| where type == "microsoft.compute/virtualmachines"
| extend osType = tostring(properties.storageProfile.osDisk.osType)
| extend vmSize = tostring(properties.hardwareProfile.vmSize)
| extend location = tostring(location)
| extend osDiskName = tostring(properties.storageProfile.osDisk.name)
| extend osDiskSizeGB = tostring(properties.storageProfile.osDisk.diskSizeGb)
| extend osDiskType = tostring(properties.storageProfile.osDisk.managedDisk.storageAccountType)
| extend dataDiskCount = array_length(properties.storageProfile.dataDisks)
| extend licenseType = tostring(properties.licenseType)
| extend powerState = tostring(properties.extended.instanceView.powerState.displayStatus)
| extend provisioningState = tostring(properties.provisioningState)
| project 
    name,
    resourceGroup,
    subscriptionId,
    location,
    osType,
    vmSize,
    osDiskName,
    osDiskSizeGB,
    osDiskType,
    dataDiskCount,
    licenseType,
    powerState,
    provisioningState
```

👉 Purpose:
- Full VM inventory for operations and governance

---

# 🔥 Firewall Inventory

```kusto
resources
| where type == "microsoft.network/azurefirewalls"
| project
    name,
    resourceGroup,
    location,
    skuName = tostring(properties.sku.name),
    skuTier = tostring(properties.sku.tier)
```

👉 Purpose:
- Identify deployed Azure Firewalls and SKUs

---

# 🌍 Paired Regions & DR Dependencies

## 10. Storage with GRS / RA-GRS

```kusto
Resources
| where type =~ "microsoft.storage/storageaccounts"
| project name, resourceGroup, location, sku = sku.name
| where sku contains "GRS"
```

👉 Purpose:
- Detect dependency on paired regions

---

## 11. SQL Geo-Replication

```kusto
Resources
| where type =~ "microsoft.sql/servers/databases"
| extend geo = properties.secondaryType
| where isnotempty(geo)
| project name, resourceGroup, location, geo
```

👉 Purpose:
- Identify cross-region DR setups

---

## 12. Key Vault Redundancy

```kusto
Resources
| where type =~ "microsoft.keyvault/vaults"
| project name, location, sku = properties.sku.name
```

👉 Note:
- Geo-redundancy is implicit

---

# 🌐 Networking Inventory

```kusto
Resources
| where type startswith "microsoft.network"
| where type in~ (
    "microsoft.network/virtualnetworkgateways",
    "microsoft.network/connections",
    "microsoft.network/expressroutecircuits",
    "microsoft.network/expressroutegateways",
    "microsoft.network/vpngateways",
    "microsoft.network/vpnsites",
    "microsoft.network/virtualwans",
    "microsoft.network/virtualhubs"
)
| project name, type, location, resourceGroup, subscriptionId
| order by ['location'] asc
```

👉 Purpose:
- Inventory of connectivity components

---

# 📍 Regional Distribution

## 13. Resource Count per Region

```kusto
Resources
| where location != ""
| summarize count() by location
| order by location asc
```

---

## 14. Global Resources

```kusto
Resources
| where location in~ ("global")
| project location, name, type, resourceGroup, subscriptionId
```

---

## 15. Clean Region Count (Exclude noise)

```kusto
Resources
| where location != ""
| where type !in~ (
    "microsoft.network/networkwatchers"
)
| summarize count() by location
```

---

# 🔐 Key Vault CLI (Complementary)

```bash
az keyvault list -o table
```

👉 Purpose:
- Quick CLI inventory outside ARG

---

# ⚠️ Notes & Limitations

- ARG provides **inventory-level visibility**, not runtime behavior
- Does NOT show:
  - actual traffic
  - firewall inspection
  - effective NSG rules at subnet level
- Should be combined with:
  - Azure Monitor
  - Network Watcher
  - Defender for Cloud

---

# 🎯 Final Summary

These queries allow you to:

- Detect **internet exposure risks**
- Understand **infrastructure distribution**
- Identify **cost optimization opportunities**
- Reveal **hidden dependencies (paired regions)**
- Support **Landing Zone design decisions**

---


# LogAnalytics workspace

## Frontdoor 
````
AzureDiagnostics
| where Category startswith "FrontDoor"
| limit 20
````

````
AzureDiagnostics
| where Category == "FrontDoorAccessLog"
| where TimeGenerated >= datetime(2026-05-09T18:00:00Z)
| where TimeGenerated <= datetime(2026-05-11T06:00:00Z)
| where clientCountry_s == "Germany"
| take 20
````

### listar países distintos en el rango temporal
````
AzureDiagnostics
| where Category == "FrontDoorAccessLog"
| where TimeGenerated between (datetime(2026-05-09T18:00:00Z) .. datetime(2026-05-11T06:00:00Z))
| where isnotempty(clientCountry_s)
| distinct clientCountry_s
| order by clientCountry_s asc
````

### País + número de coincidencias (requests)

````
AzureDiagnostics
| where Category == "FrontDoorAccessLog"
| where TimeGenerated between (datetime(2026-05-09T18:00:00Z) .. datetime(2026-05-11T06:00:00Z))
| where isnotempty(clientCountry_s)
| summarize Coincidencias = count() by clientCountry_s
| order by Coincidencias desc
````


---

# Azure Landing Zone – Connectivity Discovery Queries

## 1. Virtual Network Gateways

### Qué información obtenemos

Permite identificar los gateways VPN o ExpressRoute desplegados en Azure.

Información útil:
- Tipo de gateway (VPN / ExpressRoute)
- SKU
- Región
- Resource Group
- Tipo de VPN
- Subscripción

Muy útil para:
- detectar conectividad híbrida existente
- identificar arquitecturas legacy
- localizar hubs existentes
- analizar costes y SKUs antiguos

### Query

```kusto
resources
| where type =~ 'microsoft.network/virtualnetworkgateways'
| project
    subscriptionId,
    resourceGroup,
    name,
    location,
    gatewayType = properties.gatewayType,
    vpnType = properties.vpnType,
    sku = properties.sku.name
```

---

# 2. Connections

## Qué información obtenemos

Permite descubrir conexiones entre:
- VPN gateways
- ExpressRoute gateways
- Local Network Gateways

Información útil:
- tipo de conexión
- gateway Azure utilizado
- gateway on-prem asociado

Muy útil para:
- mapear túneles híbridos
- descubrir conexiones olvidadas
- entender dependencias entre Azure y on-prem

## Query

```kusto
resources
| where type =~ 'microsoft.network/connections'
| project
    subscriptionId,
    resourceGroup,
    name,
    location,
    connectionType = properties.connectionType,
    virtualNetworkGateway1 = properties.virtualNetworkGateway1.id,
    localNetworkGateway2 = properties.localNetworkGateway2.id
```

---

# 3. Local Network Gateways

## Qué información obtenemos

Representan los endpoints on-premises definidos en Azure.

Aquí suelen aparecer:
- IPs públicas on-prem
- rangos corporativos
- MPLS
- sedes
- datacenters

Información útil:
- IP pública del peer on-prem
- rangos CIDR corporativos
- ubicación lógica de conexiones híbridas

Muy útil para:
- inventariar redes corporativas
- detectar overlaps CIDR
- identificar sedes conectadas
- preparar migración a Landing Zone/vWAN

## Query

```kusto
resources
| where type =~ 'microsoft.network/localnetworkgateways'
| project
    subscriptionId,
    resourceGroup,
    name,
    location,
    gatewayIp = properties.gatewayIpAddress,
    addressPrefixes = properties.localNetworkAddressSpace.addressPrefixes
```

---

# 4. ExpressRoute Circuits

## Qué información obtenemos

Permite descubrir circuitos ExpressRoute existentes.

Información útil:
- proveedor
- peering location
- ancho de banda
- SKU
- región

Muy útil para:
- entender conectividad privada enterprise
- identificar dependencias críticas
- validar redundancia
- preparar arquitectura hub/vWAN

## Query

```kusto
resources
| where type =~ 'microsoft.network/expressroutecircuits'
| project
    subscriptionId,
    resourceGroup,
    name,
    location,
    serviceProvider = properties.serviceProviderProperties.serviceProviderName,
    peeringLocation = properties.serviceProviderProperties.peeringLocation,
    bandwidth = properties.serviceProviderProperties.bandwidthInMbps,
    sku = properties.sku.tier
```

---

# 5. Azure Virtual WAN

## Qué información obtenemos

Permite detectar despliegues de Azure Virtual WAN.

Información útil:
- Virtual WANs
- Virtual Hubs
- regiones utilizadas

Muy útil para:
- detectar arquitecturas modernas
- identificar hubs centralizados
- preparar integración con nueva Landing Zone

## Query

```kusto
resources
| where type contains 'virtualwan'
| project
    name,
    type,
    location,
    resourceGroup
```

---

# 6. Public IP Addresses

## Qué información obtenemos

Lista todas las IPs públicas del tenant.

Muy útil para detectar:
- conectividad híbrida no documentada
- NAT manuales
- appliances legacy
- endpoints expuestos a internet

Información útil:
- IP pública
- SKU
- región
- Resource Group

## Query

```kusto
resources
| where type =~ 'microsoft.network/publicipaddresses'
| project
    name,
    location,
    sku = properties.sku.name,
    ipAddress = properties.ipAddress,
    resourceGroup
```

---

# 7. Virtual Networks

## Qué información obtenemos

Inventario completo de VNets y direccionamiento IP.

Información útil:
- nombre VNet
- región
- rangos CIDR
- subscripción

Muy útil para:
- detectar overlaps IP
- preparar Hub & Spoke
- diseñar routing híbrido
- validar segmentación

## Query

```kusto
resources
| where type =~ 'microsoft.network/virtualnetworks'
| project
    subscriptionId,
    resourceGroup,
    name,
    location,
    addressSpace = properties.addressSpace.addressPrefixes
```

---

# 8. Subscripciones visibles en el tenant

## Qué información obtenemos

Permite validar qué subscriptions son visibles para nuestra identidad.

Muy útil para:
- validar cobertura del inventario
- comprobar permisos RBAC
- detectar subscriptions fuera del scope

## Query

```kusto
resourcecontainers
| where type == 'microsoft.resources/subscriptions'
| project subscriptionId, name
```

---

# 9. Número de recursos por subscripción

## Qué información obtenemos

Permite validar rápidamente si estamos consultando múltiples subscriptions.

Muy útil para:
- confirmar alcance tenant-wide
- detectar subscriptions vacías
- validar permisos

## Query

```kusto
resources
| summarize count() by subscriptionId
```

---

# 10. Effective Route Table (CLI)

## Qué información obtenemos

Permite analizar las rutas efectivas aplicadas a una NIC/VM.

Información útil:
- next hop
- UDRs
- propagación BGP
- rutas híbridas
- default routes

Muy útil para:
- troubleshooting híbrido
- validar propagación BGP
- entender routing real

## Query / Command

```bash
az network nic show-effective-route-table
```

---

# 11. Recomendaciones para el inventario de conectividad

## Se recomienda documentar

- Tipo de conectividad
- Región Azure
- CIDRs Azure
- CIDRs on-prem
- Dependencias aplicativas
- Equipo owner
- Criticidad
- Estado HA/redundancia
- Uso de BGP
- Tipo de firewall/NVA
- Dependencia de IPs públicas
- Integración futura con Landing Zone

---

# 12. Elementos frecuentemente olvidados

## También deberían inventariarse

- DNS híbrido
- Private DNS Zones
- Forwarders
- UDRs
- NAT Gateways
- Azure Firewall
- NVAs
- Proxies
- Bastion
- Certificados VPN
- Dependencias GRS/RA-GRS
- ExpressRoute Global Reach
- Routing BGP
- Overlaps CIDR
- Private Endpoints consumidos desde on-prem
